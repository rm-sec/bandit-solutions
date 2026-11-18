# Bandit Level 17 → 18

## Problem
Two files exist in the home directory: passwords.old and passwords.new.
Each contains a list of passwords, but the password for bandit18 is
different between the two files, it appears in one but not the other.

## Approach
1. diff passwords.old passwords.new compare the two files line by
   line and show only the differences
2. The output is one line: the unique password that exists in only one file

## Wrong Turns
- Tried grep -F -x -v -f passwords.old passwords.new this works
  logically but overcomplicated a simple diff use case. Good to know
  as an alternative though
- Tried cat passwords.old | sort | uniq -u wrong because this finds
  lines unique to passwords.old, ignoring that the new password might
  be the one that was added to passwords.new

## Concept Extracted
diff is the fundamental tool for comparing two datasets. In security,
this maps directly to *change detection*: comparing a safe baseline
against a current state to spot unauthorized modifications. This same
concept applies to:
- Detecting tampered config files
- Identifying new malware signatures
- Auditing system changes after a security incident

The key insight: diff doesn't just find what's different it tells you
which file each difference came from, which is critical for determining
directionality (added vs removed).

## Key Commands
- diff passwords.old passwords.new
- diff -u passwords.old passwords.new (unified format, more readable)

## Verification
Password obtained successfully and verified by SSH login as bandit18.