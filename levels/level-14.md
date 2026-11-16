# Bandit Level 14 → 15

## Problem
There's a service listening on port 30000 of the local machine. It
validates bandit14's password and, if correct, returns bandit15's
password. This is the first level that requires interacting with a
network service rather than just reading files.

## Approach
1. cat /etc/bandit_pass/bandit14 grab the current password (you'll
   need it as input)
2. nc localhost 30000 open a raw TCP connection to the service
3. Paste/type the bandit14 password into the connection
4. The service responds with "Correct!" followed by bandit15's password

## Wrong Turns
- Tried ssh bandit15@localhost -p 30000 SSH expects an SSH handshake,
  but port 30000 speaks plain text. Connection refused immediately
- Used telnet localhost 30000 first, but telnet wasn't installed on my
  local machine; had to switch to nc (netcat), which was available
- Forgot to send a newline after pasting the password, the service
  waited forever for Enter before processing input

## Concept Extracted
This is your first introduction to *socket interaction*. Not every
service speaks SSH or HTTP some speak raw TCP on custom ports. nc
(netcat) is the universal tool for talking to any TCP/UDP port manually.
This exact skill connecting to arbitrary ports and sending raw data
is how you probe unknown services during reconnaissance in real
pentests. Also note: this is essentially a trivial authentication
bypass pattern. If a service trusts a secret sent over plaintext, anyone
who captures the traffic gets the credential.

## Key Commands
- nc localhost 30000
- echo "$(cat /etc/bandit_pass/bandit14)" | nc localhost 30000 (one-liner version)
- telnet localhost 30000 (alternative client)

## Verification
Password obtained successfully and verified by SSH login as bandit15.