# Bandit Level 0 → 1

## Problem
Connect to the bandit server and read the password file.

## Approach
1. SSH into the server using the credentials provided on the website
2. List files with ls
3. Read the readme file to find the password

## Wrong Turns
- Ran ls -l first and spent time parsing permission bits instead of just looking for filenames
- Tried cat on a non-existent file before checking what files actually exist

## Concept Extracted
The readme file is a plain text file containing the next level's password. In Linux, files that start with a dot (.) are hidden by default. While readme is not hidden, this pattern will matter in later levels. Always use ls -a to reveal hidden files.

## Key Commands
- bash
- ssh bandit0@bandit.labs.overthewire.org -p 2220
- ls
- cat readme

## Verification
The password obtained from cat readme was used successfully to SSH in as bandit1.