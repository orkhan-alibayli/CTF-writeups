# All In One / Tryhackme

## Initial reconnaissance
1. nmap scan - (nmap -sC -sV IP)
2. gobuster
3. wpscan - wpscan --url url -e ap
   
   wpscan is scanning tool for wordpress. According scan results there is two plugins ( mail-masta and reflex-gallery) which is vulnerable to these attacks:
   
   - LFI
   - SQLi

--------------------------------------------

## User.txt

### Method 1: LFI

1. Mail-masta LFI

   PoC: http://server/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd
   
   source: https://www.exploit-db.com/exploits/40290
   
2. Getting wp-config.php via LFI: http://ip/wordpress/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=php://filter/convert.base64-encode/resource=/var/www/html/wordpress/wp-config.php
   - what is wp-config.php file?  The wp-config.php file is a WordPress core file that contains the necessary information to make your WordPress website operate, including: MySQL connection settings, WordPress salts and keys, database prefix.
   - for more information: https://ithemes.com/blog/wordpress-wp-config-php-file-explained/
   - after getting password I logged in as www-data user, then found password of main user with find command based on user ownership in files.


### Method 2: SQLi

1. I tried manual sqli but I was unsuccessfull. So I tried sqlmap.

-------------------------------------------


## Privilage escalation

1. run linpeas.sh script. PE vectors are:
   - sudo -l
   - /var/backups/script.sh .  I used GTFO bins for this vector ( https://gtfobins.github.io/ ).
