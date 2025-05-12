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


Step 3 - Preparing the server
---------------------------------------------------------------------
Here's a step-by-step guide to install a **TeamSpeak 3 server on Ubuntu**, configure it to use a **custom port instead of the default 9987**, and make the necessary **firewall changes** permanent.

1. **Create a new user for TeamSpeak (optional but recommended):**

   ```bash
   sudo adduser --disabled-login teamspeak
   ```

2. **Switch to the new user and download TeamSpeak Server:**

   ```bash
   sudo su - teamspeak
   wget https://files.teamspeak-services.com/releases/server/3.13.7/teamspeak3-server_linux_amd64-3.13.7.tar.bz2
   tar -xjf teamspeak3-server_linux_amd64-3.13.7.tar.bz2
   mv teamspeak3-server_linux_amd64/* .
   rm -r teamspeak3-server_linux_amd64*
   exit
   ```

3. **Accept license agreement and enable the server as a service:**

   ```bash
   sudo touch /home/teamspeak/.ts3server_license_accepted
   ```

4. **Create a systemd service:**

   ```bash
   sudo nano /etc/systemd/system/teamspeak.service
   ```

   Paste the following:

   ```ini
   [Unit]
   Description=TeamSpeak 3 Server
   After=network.target

   [Service]
   WorkingDirectory=/home/teamspeak
   User=teamspeak
   Group=teamspeak
   Type=forking
   ExecStart=/home/teamspeak/ts3server_startscript.sh start
   ExecStop=/home/teamspeak/ts3server_startscript.sh stop
   ExecReload=/home/teamspeak/ts3server_startscript.sh restart
   PIDFile=/home/teamspeak/ts3server.pid
   Restart=on-failure

   [Install]
   WantedBy=multi-user.target
   ```

   Then reload systemd and start the service:

   ```bash
   sudo systemctl daemon-reexec
   sudo systemctl daemon-reload
   sudo systemctl enable --now teamspeak
   ```

---

### **Step 2: Change Default Voice Port (9987) to Custom Port**

1. **Edit `ts3server.ini` (create it if not existing):**

   ```bash
   sudo nano /home/teamspeak/ts3server.ini
   ```

   Add or change the following lines:

   ```ini
   default_voice_port=12345
   ```

   *(Replace `12345` with your custom port)*

2. **Tell the server to use this config file:**
   Edit the startscript (`ts3server_startscript.sh`) or override it in systemd:

   Open `/etc/systemd/system/teamspeak.service` again and change this line:

   ```ini
   ExecStart=/home/teamspeak/ts3server_startscript.sh start
   ```

   To:

   ```ini
   ExecStart=/home/teamspeak/ts3server_linux_amd64/ts3server_minimal_runscript.sh inifile=ts3server.ini
   ```

   Reload and restart:

   ```bash
   sudo systemctl daemon-reload
   sudo systemctl restart teamspeak
   ```

---

### **Step 3: Open Custom Port in the Firewall Permanently**

1. **Allow the custom port using UFW (replace `12345` with your port):**

   ```bash
   sudo ufw allow 12345/udp
   ```

2. **Ensure UFW is enabled:**

   ```bash
   sudo ufw enable
   ```

3. **Check status:**

   ```bash
   sudo ufw status
   ```

---

### **Step 4: Verify**

* Connect from your TeamSpeak client using your server IP and the custom port.
* Example: `yourdomain.com:12345` or `IP:12345`.

---

Would you like me to provide a full script that automates this entire process?
