### TRYHACKME / GAMINGSERVER

--------------------------
## Initial reconnaissance
# 1. Nmap scan 

![image](https://user-images.githubusercontent.com/99633184/188276330-c40c14a4-f3ca-4cbb-ae63-ee1bd1ce57ef.png)

According the scan there is a web app and we may be will use port 22 for ssh in future.

# 2. Directory enumeration with Gobuster

![image](https://user-images.githubusercontent.com/99633184/188276464-86dc4465-a04c-4d9d-9307-a98ece5cb9c4.png)

In /uploads there is a file with name dict.lst which is some kind of wordlist. I will use this as wordlist in future possible bruteforces. The other files did not give me any valuable information. In /secret directory there is a rsa private key which seems can be used for ssh login. But it is password protected. So we should crack it:

# 3. Cracking private key passphrase

![image](https://user-images.githubusercontent.com/99633184/188276767-a51afc66-0466-4614-ac85-a0a2fbfc2b4c.png)

Note: I used disct.lst file which is in /uploads directory as a wordlist. 

Now, we have private key and passphrase of this key. But we do not have username for ssh login. I tried found the username via studying files in uploads directory but I could not find anything. Then during inspecting web phage I saw potensial username in html comment:

![image](https://user-images.githubusercontent.com/99633184/188276968-a20bcbb6-1278-47fa-a11c-d5e5c3880469.png)

# 4. User flag

![image](https://user-images.githubusercontent.com/99633184/188277055-15fa1557-37e5-4c5c-af45-483b59c5680c.png)

---------------------------------------------

## Privilage escalation

I tried for checking SUID bits, crontabs, sudo rights but find nothing. Then I realized this user is member of an interesting group:

![image](https://user-images.githubusercontent.com/99633184/188277433-a8bd3623-13fd-4eeb-afed-4951cbec7902.png)

This group is lxd.

After further research about LXD I learnd that LXD is some kind of management tool for LXC ( Linux Containers ). Key point is that any user in lxd group can create an container image. Since this image is running with sistem privilages we can mount filesyatem of host os to this container and read everthing ( This is high level description which I undestand about LXD/LXC. May be everything which I wrote is not correct).

So I used this page for escalating my privilages ( withouth internet method 2 ) : https://github.com/carlospolop/hacktricks/blob/master/linux-hardening/privilege-escalation/interesting-groups-linux-pe/lxd-privilege-escalation.md

![image](https://user-images.githubusercontent.com/99633184/188278000-a21c5dcd-36ca-4165-80a7-42dbab1f3fe1.png)

