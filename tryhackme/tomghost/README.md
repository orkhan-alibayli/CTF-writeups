# TRYHACKME / TOMGHOST

--------------------------------------------------------------------------------
## Initial reconnaissance
1. NMAP

![image](https://user-images.githubusercontent.com/99633184/187896832-10e04d80-1ef4-4293-ae89-34b8139502c8.png)

According the results of nmap scan there are 4 open ports. Port 8080 and 8009 seems interesting. There is also sh port which I assume we will use it after finding credentials in future steps. But for tcp port 53 for now I am not seeing any attack vector.
I went to http://IP:8080 in my web browser and saw default page for Apache Tomcat. Version information ( Apache Tomcat/9.0.30 ) for tomcat seems valuable.

2. For reconnaissance purpose I could use other tools but because in room creator explicitly mention attack vector ( Identify recent vulnerabilities to try exploit the system or read files that you should not have access to. ) I did not used other tools.

-------------------------------------------------------------------------------
## Attack vector
It is more likely in the Tomcat there is some vulnerability. After googling for "Apache Tomcat/9.0.30 vulnerability" we can see there is CVE-2020-1938 for the version.
As I understood there is some sort of communication between Apache http server and Apache tomcat via port 8009 ( which appear in nmap scan ) and this communication is by default enabled. So by using this we can read files from web server. For more information: https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1938 .

I fond exploit for the vulnerability in this github page: https://github.com/dacade/CVE-2020-1938 . After executing exploit some interesting informations appear which looks like credentials of a user.

![image](https://user-images.githubusercontent.com/99633184/187902149-869044bc-2e05-471b-bdb8-daa54e4e13bf.png)

Since we have open ssh port I tried login using these credentials for logging in via ssh:

![image](https://user-images.githubusercontent.com/99633184/187902501-ecf1cffa-e720-4f78-bae5-aa0419254261.png)

Firstly I thought user flag is related with credential.pgp and tryhackme.asc files but after looking at home directory I saw another user and after going its home directory I fond user flag.

![image](https://user-images.githubusercontent.com/99633184/187903258-9d4f79d0-4bc2-4873-8f78-21d96a32a6e0.png)

-------------------------------------------------
## Privilage Escalation
1. PGP ( Pretty Good Privacy )

After some research I come up with idea that credential.pgp is a file which encrypted with key in tryhackme.asc file. For decrypting file I tried to use this link: https://superuser.com/questions/46461/decrypt-pgp-file-using-asc-key. But tryhackme.asc file is password protected so I had to crack that:

![image](https://user-images.githubusercontent.com/99633184/187906976-7b77dc2e-4768-4663-81a8-868054b015cc.png)

Using credentials which described above I logged in via ssh as merlin. Firstly I tried runninng **sudo -l** command for seeing if there is any command which this user can run under sudo privilages. And this command gave me some valuable information:

![image](https://user-images.githubusercontent.com/99633184/187908131-f0111056-8910-4a20-9bfc-761d851ddad7.png)

It seems this user can run **zip** command as sudo user and probably this is the way which I should follow for escalating privilages. So I did some research about escalating privilages via zip and found that we can run commands with **--unzip-command** switch ( https://www.hackingarticles.in/linux-for-pentester-zip-privilege-escalation/ ) :

![image](https://user-images.githubusercontent.com/99633184/187909633-deffca58-4639-4fad-9d38-8ebe0484ca1a.png)

Actually I can not find meaningfull example of executing commands via **zip** command. So I am not sure why zip have this kind of feature.


