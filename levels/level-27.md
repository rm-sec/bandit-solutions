# Bandit Level 27 → 28

## Problem
A bare Git repository lives on the server, accessible over SSH using a
special user (bandit27-git) that shares bandit27's password. You must
clone it and read the committed contents to find the next password.

## Approach
1. Make a scratch working directory (you can't write in your home dir):
   mkdir /tmp/work && cd /tmp/work
2. Clone with the *non-standard port explicitly stated*:
   git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
3. Enter bandit27's password when prompted
4. cd repo && cat README the password is inside

## Wrong Turns
- Ran git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
  without :2220 Git tried port 22 and the connection was refused.
  Every SSH service on this box uses 2220, including Git-over-SSH
- Tried cloning into my home directory and got permission errors; had
  to use /tmp instead
- Typed bandit27-git's "own" password separately there isn't one; the
  -git account reuses the regular level password

## Concept Extracted
Git repositories are a top-tier source of *secret leakage* in real
bug bounty work. Developers commit API keys, tokens, and passwords, then
delete them in a later commit but the history retains everything. This
level introduces the mechanic (cloning over an unusual transport), and
the next two levels exploit exactly that history problem. Also worth
noting: URLs embedding credentials or non-default ports are easy to
misread. Always double-check the port when SSH services deviate from 22.

## Key Commands
- mkdir -p /tmp/work && cd $_
- git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
- cat README

## Verification
Password obtained successfully and verified by SSH login as bandit28.