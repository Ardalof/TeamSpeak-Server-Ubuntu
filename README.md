Introduction
---------------------------------------------------------------------
This tutorial describes how to install a TeamSpeak server on your Ubuntu VPS and set up port forwarding through Cloudflare (I recommend Contabo's VPS)

The tutorial was Tested on Ubuntu 22.04 and 24 using Teamspeak-Server Version 3.13.7!

The tutorial uses the example IP 127.0.0.1
This hostname must be replaced by the name of your own server if you follow the workflow described in this tutorial.
We will also use the domain sirabi.online in this course. Replace this with your own domain.


Requirements
---------------------------------------------------------------------
For a "small channel" (up to about 32 users) a Cloud VPS 10 is sufficient.

I recommend setting up the instance on a Cloud VPS 10. If you feel that the instance is too slow (i dont think so), you can always upgrade to a larger server.

Ubuntu22.04 from the CCP image must already be installed on the server.

You also need a domain, with a domain from a web hosting package being sufficient. (You can still use the domain with your web hosting, as we will be using a subdomain.)

Step 1 - Finding out the IP of your server
---------------------------------------------------------------------
You will receive the server details via email immediately after purchase, or you can view them through the server-portal.

Step 2 - Configuring DNS-Cloudflare
---------------------------------------------------------------------
Enter your domain in Cloudflare, and after completing the verification process, go to the DNS-Records.
![image](https://github.com/user-attachments/assets/4fdc692c-a6d8-4275-84f3-d1371878efdb)

Create an **A** record and set the values.
![image](https://github.com/user-attachments/assets/fd555d3b-2d6c-47fe-9202-afa9044100bc)

Create an **SRV** record and enter **_ts3._udp** in the **Name** (required) field.
In the 'Port' field, enter the port you want the TeamSpeak server to use. (The default port is 9987, but it may be filtered in some countries).
In the 'Target' (required) field, you can enter the endpoint address to be used in TeamSpeak. I didn't use a subdomain here; instead, I assigned a full domain to it.
![image](https://github.com/user-attachments/assets/068e59d7-3753-41c8-839f-60a895b7c705)



