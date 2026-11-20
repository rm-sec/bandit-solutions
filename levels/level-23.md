# Bandit Level 22 → 23

## Problem
A cron job runs as bandit23 every minute and at system reboot. It
copies bandit23's password to a temporary file in /tmp/ using an
MD5 hash of the username as the filename. Output is redirected to
/dev/null, so you can't see it in logs.

## Approach
1. cat /etc/cron.d/cronjob_bandit23 reveals cron runs
   /usr/bin/cronjob_bandit23.sh as bandit23
2. cat /usr/bin/cronjob_bandit23.sh shows the script uses
   $(whoami) to determine the target, computes an MD5 hash, and
   writes the password to /tmp/<hash>
3. Compute the target hash: echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
4. Read the file: cat /tmp/<computed_hash>

## Wrong Turns
- Tried cat /tmp/* and got flooded with dozens of files didn't
  realize the filename was deterministic based on the username
- Tried running the cron job manually as bandit23 instead of computing
  the hash wasted time before realizing the script's logic was
  transparently visible
- Didn't notice the @reboot line in the cron config only looked
  at the * * * * * line; both lines point to the same script, so
  it runs twice (once at boot, once per minute), which is redundant

## Concept Extracted
Reading cron scripts before running them is *passive reconnaissance*.
Instead of guessing what a scheduled job does, you read the source
and understand it. In real pentesting, many privilege escalation paths
are visible if you know where to look:
- /etc/cron.d/ system cron jobs (often writable by non-root users)
- /etc/crontab system-wide crontab
- /etc/sudoers who can run what as whom
- /etc/passwd and /etc/shadow user accounts and password hashes

Knowing where to look is as important as knowing what to type.

## Key Commands
- cat /etc/cron.d/cronjob_bandit23
- cat /usr/bin/cronjob_bandit23.sh
- echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
- cat /tmp/<hash>

## Verification
Password obtained successfully and verified by SSH login as bandit23.