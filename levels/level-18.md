# Bandit Level 18 → 19

## Problem
You can SSH into bandit18, but the server immediately closes the
connection with "Byebye!" before giving you a shell. You cannot run any
interactive commands. Yet the password is still readable from the
readme file in the home directory.

## Approach
1. Instead of requesting an interactive shell, pass a command directly
   through SSH: ssh bandit18@localhost -p 2220 "cat readme"
2. The server executes cat readme, prints the output, then exits
   exactly as it would if you had a shell

## Wrong Turns
- Tried logging in normally and got "Byebye!" repeatedly assumed the
  account was broken or locked out
- Wasted time trying different SSH flags (-t, -T, -X) hoping to
  force a shell; none of them help when the remote side explicitly
  closes the session
- Considered skipping the level entirely until realizing SSH supports
  non-interactive command execution

## Concept Extracted
SSH isn't just for interactive shells. You can execute arbitrary
commands remotely without ever getting a prompt. This is how automation
tools (Ansible, Fabric, CI/CD pipelines) work under the hood they all
use ssh user@host "command". In pentesting, this matters because even
if a service restricts interactive access, you may still be able to run
specific commands remotely. Always test both modes.

## Key Commands
- ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
- scp is another tool that uses this same underlying mechanism

## Verification
Password obtained successfully and verified by SSH login as bandit19.