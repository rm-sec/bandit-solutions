# Bandit Level 19 → 20

## Problem
The password for bandit20 is at /etc/bandit_pass/bandit20, but only
bandit20 can read it. In your home directory there's a bandit20-do
binary with the setuid bit set running it executes commands as
bandit20 instead of bandit19.

## Approach
1. ls -la — spot -rwsr-x--- on bandit20-do; the s in the owner
   execute slot means setuid
2. ./bandit20-do (no args) reveals usage: "Run a command as another user"
3. ./bandit20-do id confirms euid switches to bandit20
4. ./bandit20-do cat /etc/bandit_pass/bandit20 read the file with
   elevated effective UID

## Wrong Turns
- Ran cat /etc/bandit_pass/bandit20 directly and got Permission denied
  forgot that setuid only applies when going through the binary
- Tried sudo cat ... no sudo rights for bandit19; wasted time hoping
  for a misconfigured sudoers entry
- Didn't run the binary without arguments first and tried guessing
  subcommands like ./bandit20-do --read before reading its help text

## Concept Extracted
*Setuid (set user ID on execution)* makes a program run with the
privileges of its file owner, not the person who launched it. This is
how passwd works normally you change your own password even though
the shadow file is root-owned. But it's also one of the most common
privilege escalation vectors in real systems: any poorly written setuid
binary can be abused. Spotting the s bit in ls -l output is step
one of every Linux privilege-escalation checklist (GTFOBins catalogs
these exact binaries).

## Key Commands
- ls -la (look for s in permissions)
- ./bandit20-do id
- ./bandit20-do cat /etc/bandit_pass/bandit20

## Verification
Password obtained successfully and verified by SSH login as bandit20.