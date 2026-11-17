# Bandit Level 16 → 17

## Problem
There are multiple services running somewhere between ports 31000–32000
on localhost. Only one speaks SSL/TLS and returns bandit17's private SSH
key when given the correct password. The others echo back whatever you
send them. You must discover, identify, and exploit the right service.

## Approach
1. nmap -sV -p 31000-32000 localhost scan the full port range,
   filter for open ports and service versions
2. Identify which ports use SSL/TLS (nmap shows "ssl/echo" vs plain "echo")
3. Connect to the SSL port with openssl s_client -connect localhost:31790
4. Submit bandit16's password; receive an RSA private key in response
5. Save the key to /tmp/sshme/id_rsa, set permissions to 600, then
   ssh -i id_rsa bandit17@localhost

## Wrong Turns
- Scanned all 1000 ports manually with nc one by one instead of using
  nmap wasted ~20 minutes before realizing nmap existed for this exact job
- Connected to port 31518 first because it showed "ssl/echo" in nmap
  output but it just echoed my password back instead of giving me a key.
  Had to try each SSL port until one returned something useful
- Forgot chmod 600 on the saved key file, SSH refused to use it with
  "Bad owner or permissions" error

## Concept Extracted
This is your first real *reconnaissance workflow*: enumerate → classify
→ target. In professional pentesting, you never guess ports blindly.
You scan, identify what each service does, then focus only on the ones
that match your goal. Also note that not every SSL service gives you
what you want some are decoys. Learning to distinguish useful targets
from noise is a core skill in both CTFs and bug bounty hunting.

## Key Commands
- nmap -sV -p 31000-32000 localhost
- echo "$(cat /etc/bandit_pass/bandit16)" | openssl s_client -connect localhost:31790 -quiet
- mkdir /tmp/sshme && nano /tmp/sshme/id_rsa (paste key here)
- chmod 600 /tmp/sshme/id_rsa
- ssh -i /tmp/sshme/id_rsa bandit17@localhost

## Verification
Password obtained successfully and verified by SSH login as bandit17.