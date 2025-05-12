Introduction
---------------------------------------------------------------------
This tutorial describes how to install a TeamSpeak server on your VPS ((I recommend Contabo's VPS))

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
