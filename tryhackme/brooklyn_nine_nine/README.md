# TRYHACKME / BROOKLYN_NINE_NINE

----------

## Initial reconnaissance
1. NMAP

![image](https://user-images.githubusercontent.com/99633184/188097802-353b54ec-1573-471a-9181-dfe09d2fdc18.png)

The most valuable information is that tcp port 21 is open and allows anonymous login as described below:

![image](https://user-images.githubusercontent.com/99633184/188098419-09c378f5-e5e3-4a0a-a062-bd0a7293ee28.png)

According to note Jake's password is too week. I assume it is ssh password so I tried to start hydra for brute force:

![image](https://user-images.githubusercontent.com/99633184/188099943-0f38b8aa-4047-454e-9cb2-454417c2c018.png)

