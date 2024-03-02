# AD-IT-Infrastructure-Lab
Network hosted on windows server with 1000 users created with powershell, managed my Server Manager and Active Directory

To create this lab setup, I utilized virtual box to create my windows server machine with a client machine. The setup for this lab infrastructure can be found below.

![image](https://github.com/JordanPenaloza/AD-IT-Infrastructure-Lab/assets/113396128/97c2f98e-15cd-43b0-a46f-eb6276a54122)

I had to make it possible for this infrastructure to be isolated but also be able to use the internet
My server machine needed to have 2 network adapters, NAT for internet and INT for all other machines to connect to and use the server as an internet provider
As shown in the diagram, our host machine will connect to the server internally and backpack off of it to connect to the internet

I used 
