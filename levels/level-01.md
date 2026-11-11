# Bandit Level 1 → 2

## Problem
Read a file whose name is a single dash (-).

## Approach
1. List files with ls
2. Notice a file named -
3. Read it using a relative path to bypass cat's special handling

## Wrong Turns
- Ran cat - and the terminal simply hung with no output and no
  error. No idea what happened until I checked man cat.
- Guessed it wanted input, started typing the filename again, then
  had to hit Ctrl-C to break out.

## Concept Extracted
Many Unix commands reserve - to mean "read from stdin" rather than
a literal filename. This isn't a Bandit quirk — it's a convention
across grep, tar, ssh, git, and most CLI tools. Two general fixes:
prefix the path (./-) or signal end-of-options (--). When a
command hangs silently, suspect it's waiting on stdin, not crashed.

## Key Commands
- ls
- cat ./-
- cat -- -
- Ctrl-C to abort a command stuck reading stdin

## Verification
Password from cat ./- used successfully to SSH in as bandit2.