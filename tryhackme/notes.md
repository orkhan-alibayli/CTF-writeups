## From room Hacker vs Hacker
1. Brute force server executables ( in this case .php ) from parameters. I executed command via something like this: http://IP/cvs/shell.pdf.php?cmd=id.
   In this case parametr was "cmd". For future pentesting remember this and try to brute frce parameters.
   
2. Look at all crontab files ( /etc/crontab, /etc/crontab.d and etc.). Also look at PATH variable inside of crontab files. If some executable is not using full path and you can crate another executable with same name inside PATH directories and execute whatever you want.
   In this case I had something like that:
   
   <img width="873" alt="image" src="https://user-images.githubusercontent.com/99633184/200292002-9bf06741-a2cb-424a-ba6c-3286b28f4fc7.png">

   since I had write permission in "/home/lachlan/bin" I crated script named "pkill" and this script executed by root as cron job.
   
   By the way this crontab kills ssh sessions. It was so interesting that when I try to login with ssh after successfull authentication my session was ended. So I logged in as "su lachlan".
-----------------------------------------------------------------------------

## From room IDE
1. This can be achived via running linpeas.sh script. But for manual recon looking at these was helpfull.
   - look at .bash_history for any interesting command which executed by user previously
   - check which commands can be executed as sudo
   - in this case there was a service which I could execute as sudo user:
![image](https://user-images.githubusercontent.com/99633184/201297102-1dbe67be-9445-4404-99c7-035a807a5905.png)

   - also I had write permission for /etc/systemd/system/multi-user.target.wants/vsftpd.service file so I edited this file and when service restarts my shell executes:
![image](https://user-images.githubusercontent.com/99633184/201298665-0b201d88-2470-4168-b867-d408919911aa.png)

----------------------------------------------------------------------------------------------------------
## From room Library
1. Manual username enumeration.
   - I was able to read comments of users in the site. There was 4 user: root, www-data, anonymous and meliodas ( this user took my attention ). So I decided to bruteforce this user and I found ssh password of this user. This was the only way to take initial foothold on the webserver.
2. When new module imported to python script this module is looked for in the current directory. This can cause executig command as another user who runs script:


![image](https://user-images.githubusercontent.com/99633184/201655243-86318968-f42b-4e0c-b60c-9a3624777a79.png)
