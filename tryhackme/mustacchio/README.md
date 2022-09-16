# TRYHACKME / MUSTACCHIO

----------------------

## Initial reconnaissance

1. NMAP

![image](https://user-images.githubusercontent.com/99633184/190626248-d258f453-e105-4003-a491-a2191b9cfaf5.png)

There is not anything interesting in port 22 and 80. But port 8465 seems extraordinary:

2. Gobuster

![image](https://user-images.githubusercontent.com/99633184/190628160-0f382347-f076-4e5a-8278-abeb513438f6.png)

Inside custom directory there is usefull .bak file:

![image](https://user-images.githubusercontent.com/99633184/190628396-a2db0454-6776-4763-a915-86d870da018e.png)

![image](https://user-images.githubusercontent.com/99633184/190629539-0dfef85d-66bc-42de-abd1-52708c025d85.png)

Then using this credentials I logged in as admin user in this url: http://IP:8476

![image](https://user-images.githubusercontent.com/99633184/190630579-949f5374-b610-433b-b6cf-8627fc87bd77.png)

![image](https://user-images.githubusercontent.com/99633184/190637560-8bcaeb6c-86d6-426b-98a9-c912499d6e6c.png)

I used this private key for logging in as barry via ssh.

-----------------------------------

## Privilage escalation

![image](https://user-images.githubusercontent.com/99633184/190641161-d7745cda-9fdb-4050-aa00-9d86b5a11336.png)

"strace is a diagnostic tool in Linux. It intercepts and records any syscalls made by a command. Additionally, it also records any Linux signal sent to the process. We can then use this information to debug or diagnose a program. It’s especially useful if the source code of the command is not readily available."

After running in the results of strace command we can find that thhis binary is reading log files via tail command. But tail command is not been calling via full binary path. So we can crate fake tail command in other directories (for example /tmp ) and then add thid directory to PATH.

![image](https://user-images.githubusercontent.com/99633184/190644675-9716c433-60fb-4aee-b213-6210d4b57039.png)



-----------------------

## Notes

1. nmap full port scan  ---  Because may be there is some open ports which does not exixts in common pots list.
