# Bandit Level 8 → 9

## Problem
The password is the single line in data.txt that appears exactly once
(all other lines are duplicated or triplicated).

## Approach
1. Pipe data.txt through sort | uniq -u this sorts the lines,
   then prints only those that appear exactly once
2. The output is one line: the password

## Wrong Turns
- Tried uniq data.txt got nothing because uniq only removes
   consecutive duplicates; if "AAAA" appears on line 1 and line 500,
   uniq leaves both. You must sort first to group identical lines
   together
- Tried sort data.txt | grep -v with some pattern overcomplicated
  it instead of using the tool designed for this exact job

## Concept Extracted
sort | uniq -u is a classic CTF pattern for "find the unique value."
The order matters: sort groups identical lines, then uniq -u (the
-u flag = "unique only") prints only lines that appeared once. This
pattern shows up everywhere duplicate detection, log analysis, and
even data-cleaning in real Python scripts.

## Key Commands
- cat data.txt | sort | uniq -u
- sort data.txt | uniq -u (equivalent, more efficient)

## Verification
Password obtained successfully and verified by SSH login as bandit9.