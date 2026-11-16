# Bandit Level 13 → 14

## Problem
The password for bandit14 is stored in /etc/bandit_pass/bandit14, but
you only have an SSH private key (sshkey.private), not a password.
You need to use key-based authentication to log in as bandit14.

## Approach
1. file sshkey.private confirm it's a valid RSA private key (PEM format)
2. ssh -i sshkey.private bandit14@localhost -p 2220 SSH into the
   server using the private key as identity instead of a password
3. cat /etc/bandit_pass/bandit14 read the password file

## Wrong Turns
- Tried ssh bandit14@localhost without -i SSH prompted for a
  password, which I didn't have, and the connection failed
- Didn't initially realize I was SSHing into localhost (the same
  server) assumed I needed a different IP address
- Tried cat sshkey.private and saw PEM-encoded base64 panicked
  thinking it was encrypted, but it's just the key file format

## Concept Extracted
SSH key-based authentication replaces passwords with asymmetric
cryptographic keys. The private key (sshkey.private) must be kept
secret, while its corresponding public key is stored on the server.
Two critical lessons here:
1. *File permissions matter*: SSH will refuse to use a private key
   if it's readable by others (chmod 600 sshkey.private is mandatory)
2. *The -i flag*: tells SSH which identity file to use instead of
   the default ~/.ssh/id_rsa

In real-world pentesting, stolen private keys are one of the most
dangerous findings they bypass passwords entirely.

## Key Commands
- file sshkey.private
- ssh -i sshkey.private bandit14@localhost -p 2220
- cat /etc/bandit_pass/bandit14
- chmod 600 sshkey.private (should have done this first)

## Verification
Password obtained successfully and verified by SSH login as bandit14.