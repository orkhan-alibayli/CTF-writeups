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
