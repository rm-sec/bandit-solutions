# Bandit Level 6 → 7

## Problem
The password file could be anywhere on the entire filesystem not just
under inhere. Three exact attributes: owner bandit7, group bandit6,
size 33 bytes.

## Approach
1. Search from / (root) using find with three stacked filters:
   -user bandit7 -group bandit6 -size 33c
2. Redirect stderr to /dev/null because most of the filesystem is
   unreadable and floods the output with "Permission denied" errors
3. Exactly one file matches: /var/lib/dpkg/info/bandit7.password

## Wrong Turns
- Ran find / ... without 2>/dev/null and got ~500 lines of permission
  errors before any results appeared nearly gave up thinking nothing
  matched
- Searched only /home first, assuming the file would be there; it was
  buried in a system package directory instead

## Concept Extracted
Privilege escalation reconnaissance almost always starts with a broad
filesystem search filtered by ownership, permissions, or size. The
2>/dev/null trick is essential, you can't read stderr noise and real
results at the same time. Also note that the password lived inside
/var/lib/dpkg/, a package-management directory. Real-world lesson:
sensitive files often end up in unexpected places when installed via
package managers.

## Key Commands
- find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
- cat /var/lib/dpkg/info/bandit7.password

## Verification
Password obtained successfully and verified by SSH login as bandit7.