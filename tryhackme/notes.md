## From room Hacker vs Hacker
1. Brute force server executables ( in this case .php ) from parameters. I executed command via something like this: http://IP/cvs/shell.pdf.php?cmd=id.
   In this case parametr was "cmd". For future pentesting remember this and try to brute frce parameters.
   
2. Look at all crontab files ( /etc/crontab, /etc/crontab.d and etc.). Also look at PATH variable inside of crontab files. If some executable is not using full path and you can crate another executable with same name inside PATH directories and execute whatever you want.
   In this case I had something like that:
   
   <img width="873" alt="image" src="https://user-images.githubusercontent.com/99633184/200292002-9bf06741-a2cb-424a-ba6c-3286b28f4fc7.png">

   since I had write permission in "/home/lachlan/bin" I crated script named "pkill" and this script executed by root as cron job.
   
   By the way this crontab kills ssh sessions. It was so interesting that when I try to login with ssh after successfull authentication my session was ended. So I logged in as "su lachlan".
