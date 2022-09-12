# TRYHACKME / TEAM

---------------------------------------

## Initial reconnaissance

### 1. NMAP scan

![image](https://user-images.githubusercontent.com/99633184/189061898-e8eb5a97-3bcb-4f8d-9c43-0b25fc3a9342.png)

### 2. Gobuster

![image](https://user-images.githubusercontent.com/99633184/189062650-062ade4e-6884-4af4-9ec1-ff25e4298925.png)


Both nmap scan and gobuster do not did not give us usefull information.

I saw something interesting during looking at html source of default web page:

![image](https://user-images.githubusercontent.com/99633184/189063425-18a933bf-6dee-410c-8358-b1b484c8303e.png)

So I added team.thm to /etc/hosts file and then opened team.thm in my browser:

![image](https://user-images.githubusercontent.com/99633184/189063921-e6006d07-ac39-487a-8745-dcf94057ddb9.png)

I also loked at html codes of this page but there was nothing meaningfull. For finding directories of the second site I run gobuster again:

![image](https://user-images.githubusercontent.com/99633184/189064435-fd522e22-c9a1-4233-b284-0c6a5eb823cb.png)

It is always worth to look at inside of robots.txt. So I did it. But there only was a string like "dale". It seems username. Thats why I decided brute force this username with hydra for ssh but I was unsuccessfull.

After a while I tried run gobuster for finding something inside of scripts directory:

![image](https://user-images.githubusercontent.com/99633184/189066598-8f8b7910-6ba8-4ae5-a304-2d5d96491eef.png)

![image](https://user-images.githubusercontent.com/99633184/189069643-7f2e80c6-5ff3-4c63-9ff8-8f6f27837d12.png)


According to the highlighted sentence, we can say that the previous extension of this file was script.old. After going to http://team.thm/scripts/script.old we will get file like that:

![image](https://user-images.githubusercontent.com/99633184/189070640-6889b57c-6a1c-43c1-aeec-e51471485649.png)

We found ftp username and password:

![image](https://user-images.githubusercontent.com/99633184/189071245-b91a52d3-8544-46b3-b319-4bead7ae92a1.png)

Under http://dev.team.thm there is path traversal vulnerabilty and because of in note there is some mention about ssh config files we can say that we should use this vulnerability for getting this config files.

![image](https://user-images.githubusercontent.com/99633184/189072907-2d26b112-9d2d-46ed-a982-e62019a6ae89.png)

I found this source for usefull linux config files: https://github.com/hussein98d/LFI-files . Since we are looking for ssh related files I grepped the file and found this: /etc/ssh/sshd_config :

![image](https://user-images.githubusercontent.com/99633184/189334360-698d07b7-03f1-4170-8655-5d2cb8301359.png)

Then I used this key file for logging in via ssh with username dale.


For privilage escalation I could not find any way so I looked at writeups.

NOTE: I think to use a tool for reconnaissance purpose is better than checking vectors manually. So I decided using a tool like linpeas ( https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS )
