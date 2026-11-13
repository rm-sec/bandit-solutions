# Bandit Level 7 → 8

## Problem
A large file (data.txt, 10+ MB) contains the password on the line
that also contains the word millionth.

## Approach
1. cat data.txt | grep "millionth" filter for lines containing
   the keyword
2. The output is one line containing both the password and the word

## Wrong Turns
- Tried cat data.txt and watched the terminal scroll for ~30 seconds
  before Ctrl-C  the file is too large to read raw
- Tried grep "millionth" data.txt without the pipe first no idea
  why I didn't think of piping from cat; this is a beginner mistake
  worth noting

## Concept Extracted
grep is the fundamental text-search tool. When you need to find
specific content inside a large file, piping (|) or passing the
filename directly both work, but piping teaches the **Unix pipeline
philosophy**: each command does one thing well, and | connects them.
Real-world parallel: log analysis, threat hunting, and incident response
all start with "grep for the indicator."

## Key Commands
- cat data.txt | grep "millionth"
- grep "millionth" data.txt (equivalent, more efficient)

## Verification
Password obtained successfully and verified by SSH login as bandit8.