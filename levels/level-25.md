# Bandit Level 25 → 26

## Problem
You have bandit26.sshkey, but logging in as bandit26 immediately drops
the connection. No shell, no prompt just "Byebye!". You need to break
out of whatever the server forces on login.

## Approach
1. cat /etc/passwd | grep bandit26 reveals the login shell is NOT
   /bin/bash but /usr/bin/showtext
2. cat /usr/bin/showtext it runs more ~/text.txt then exits
3. Copy bandit26.sshkey to your local machine (localhost reuse is blocked)
4. *Shrink your terminal window to ~4 rows tall*, then connect:
   ssh -i bandit26.sshkey bandit26@... -p 2220
5. Because the window is smaller than the file, more pauses at --More--
   instead of finishing and exiting
6. Press v, this launches vim from inside more
7. From vim: :set shell=/bin/bash then :shell full bash shell
8. cat /etc/bandit_pass/bandit26

## Wrong Turns
- Tried using the key directly on the server (ssh -i ... localhost)
  and got rejected keys can't be reused from the same host; had to
  copy it locally first
- Connected with a normal-sized terminal and never saw a pause the
  more command scrolled through instantly and exited before I could
  react
- Didn't know more has an interactive escape hatch; tried pressing
  Ctrl-C, q, and Enter repeatedly before reading man more

## Concept Extracted
This is a *restricted shell escape*. When administrators try to lock
down access by replacing the shell with a limited program, they often
overlook that the program itself offers subcommands or editor escapes.
more/less launching vi via v is documented behavior which is
exactly why GTFOBins exists. The broader lesson: security controls built
by restricting interfaces rather than privileges are fragile. Real
pentests routinely escalate from restricted shells using exactly this
pattern (vim, awk, find, nmap all have known escapes).

## Key Commands
- cat /etc/passwd | grep bandit26
- scp -P 2220 bandit25@...:~/bandit26.sshkey ./
- chmod 600 bandit26.sshkey
- ssh -i bandit26.sshkey bandit26@... -p 2220 (with tiny terminal)
- In more: press v; in vim: :set shell=/bin/bash then :shell

## Verification
Password obtained successfully and verified by SSH login as bandit26.