# Bandit Level 26 → 27

## Problem
You're logged in as bandit26 (having escaped the restricted shell in the
previous level). In your home directory sits bandit27-do, a setuid
binary owned by bandit27 identical in concept to Level 19's
bandit20-do. Run it and it executes commands as bandit27.

## Approach
1. ls -la spot -rwsr-x--- on bandit27-do
2. ./bandit27-do (no args) prints usage: "Run a command as another user"
3. ./bandit27-do id confirms euid is now bandit27
4. ./bandit27-do cat /etc/bandit_pass/bandit27 read the password

## Wrong Turns
- Tried logging in fresh as bandit26 without the escape trick from Level
  25, got dumped back out immediately, and thought the account was broken
- Ran cat /etc/bandit_pass/bandit27 directly instead of going through
  the binary Permission denied, same mistake as Level 19
- Forgot to chmod +x after copying the key locally and SSH rejected it

## Concept Extracted
This is a *deliberate repeat of Level 19*, and that repetition is the
lesson. Real security work means recognizing patterns you've seen before
in new contexts. Here the delivery mechanism changed completely (Level
19 gave you a shell for free; Level 26 required escaping a pager into
vim into bash), but the escalation primitive is identical: setuid
binary + arbitrary command execution. Building a mental catalog of
primitives like this setuid, writable cron paths, sudo misconfigs,
capabilities is what separates systematic pentesters from people who
guess randomly.

## Key Commands
- ls -la (look for the s bit)
- ./bandit27-do id
- ./bandit27-do cat /etc/bandit_pass/bandit27

## Verification
Password obtained successfully and verified by SSH login as bandit27.