# Bandit Level 20 → 21

## Problem
A setuid binary (suconnect) connects back to a port you specify,
reads one line of text, and if it matches bandit20's password, sends
back bandit21's password. You need to be listening on the other end of
that connection.

## Approach
1. Open a listener on an arbitrary port: nc -lnvp 4444
2. In a second SSH session (or backgrounded), run ./suconnect 4444
3. Back at the listener, type/paste bandit20's password
4. suconnect validates it and replies with bandit21's password

## Wrong Turns
- Ran ./suconnect 4444 without starting a listener first the binary
  just hung waiting for a connection that never came
- Used only one terminal and forgot the & trick to background the
  listener; had to open two separate SSH sessions instead
- Typed the wrong password (used bandit19's instead of bandit20's) and
  got no useful error message just a silent disconnect

## Concept Extracted
This combines two earlier skills into a *reverse-connection pattern*:
setuid privileges + raw TCP sockets. Instead of connecting to a
service, you make a privileged binary connect back to you. This exact
pattern underlies reverse shells when direct inbound connections are
blocked by firewalls, malware and pentest tools have the target machine
initiate an outbound connection instead. Understanding why this works
requires knowing that outbound connections are usually less restricted
than inbound ones.

## Key Commands
- nc -lnvp 4444 (listener; -l listen, -n no DNS, -v verbose, -p port)
- echo "GbKksEFF4yrVs6il55v6gwY5aVje5f0j" | nc -l -p 4444 & (single-terminal version)
- ./suconnect 4444

## Verification
Password obtained successfully and verified by SSH login as bandit21.