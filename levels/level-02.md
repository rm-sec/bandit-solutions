# Bandit Level 2 → 3

## Problem
Read a file whose name contains spaces: spaces in this filename.

## Approach
1. List files with ls and observe the multi-word filename
2. Quote the whole path so the shell treats it as one argument

## Wrong Turns
- Ran cat spaces in this filename — the shell split it into four
  arguments and reported "No such file or directory" for each fragment
- Guessed at wildcards (cat s*) before understanding why quoting works

## Concept Extracted
Whitespace delimits arguments in POSIX shells. Two equivalent fixes:
wrap in quotes (cat "a b c") or escape each space (cat a\ b\ c).
Tab-completion auto-escapes for you, which is faster than typing
backslashes by hand. This applies to every command, not just cat.

## Key Commands
- ls
- cat "spaces in this filename"
- cat spaces\ in\ this\ filename

## Verification
Password used successfully to SSH in as bandit3.