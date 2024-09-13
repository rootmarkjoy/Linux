# Postfix Mail Server commands

### Some useful postfix commands

**//postqueue -p** is the same as **mailq**

### List mail queue and MAIL_ID's, list mail queue

```sh
postqueue -p
mailq
```

Reload config

```sh
service postfix reload
```

Restart postfix server

```sh
service postfix restart
```

View the postfix version

```sh
postconf mail_version
```

Show default postfix values

```sh
postconf -d
```

Show non default postfix values

```sh
postconf -n
```

Flush mail queue

```sh
postfix flush
```

Process the queue now

```sh
postqueue -f
```

Process all emails stuck in the queue

```sh
postsuper -r ALL && postqueue -f
```

Read email from mail queue

```sh
postcat -q MAIL_ID
```

To remove MAIL_ID mail from the queue

```sh
postsuper -d MAIL_ID
```

To remove all mail from the queue

```sh
postsuper -d ALL
```

To remove all from mail queue FAST

```sh
find /var/spool/postfix/deferred/ -type f | xargs -n1 basename | xargs -n1 postsuper -d
```

To remove all mails in the deferred queue

```sh
postsuper -d ALL deferred
```

Sort and count emails by "from address"

```sh
postqueue -p | awk '/^[0-9,A-F]/ {print $7}' | sort | uniq -c | sort -n
```

Removing all emails sent by: mailto:user@adminlogs.info

```sh
postqueue -p|grep '^[A-Z0-9]'|grep user@adminlogs.info|cut -f1 -d' '|tr -d \*|postsuper -d -
```

Remove all email sent from user@admin.info

```sh
postqueue -p|awk '/^[0-9,A-F].*user@admin.info / {print $1}'|cut -d '!' -f 1|postsuper -d -
```

Remove all email sent by domain adminlogs.info

```sh
postqueue -p | grep '^[A-Z0-9]'|grep @adminlogs.info|cut -f1 -d' ' |tr -d \*|postsuper -d -
```

Mail queue stats short

```sh
postqueue -p | tail -n 1
```

Number of emails in Mail queue

```sh
postqueue -p | grep -c "^[A-Z0-9]"
```

Fast count of emails in mail queue

```sh
find /var/spool/postfix/deferred -type f | wc -l
```

Watch Log Live

```sh
tail -f /var/log/maillog
```

Count and sort success pop3/imap logins

```sh
grep "\-login"  /var/log/dovecot-info.log |grep "Login:"|awk {'print $7'}|sort|uniq -c|sort -n
```

Count and sort success SMTP postfix logins (useful for tracking spammer)

```sh
grep -i "sasl_username"  /var/log/maillog |awk {'print $9'}|sort|uniq -c|sort -n
```

Count and sort success SMTP postfix logins on exact date "Jun 18"

```sh
grep -i "sasl_username"  /var/log/maillog |grep "Jun 18"|awk {'print $9'}|sort|uniq -c|sort -n
```

Analyze Postfix Logs

```sh
pflogsumm /var/log/maillog | less
```

** If you don't have pflogsumm then you can install this rpm package "postfix-perl-scripts".

#### https://wiki.centos-webpanel.com/postfix-mail-server-commands
