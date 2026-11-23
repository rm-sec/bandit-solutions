# Bandit Level 23 → 24

## Problem
Another cron job runs as bandit23 every minute. Unlike Level 22, this
script doesn't use a hardcoded filename it generates one dynamically
by computing an MD5 hash of the string "I am user $myname". You need to
predict that filename by reproducing the hash yourself.

## Approach
1. cat /etc/cron.d/cronjob_bandit23 find the script path
2. cat /usr/bin/cronjob_bandit23.sh read the logic:
   - myname=$(whoami) → becomes "bandit23" when cron runs it
   - mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
     → MD5 hash of "I am user bandit23"
   - Copies /etc/bandit_pass/$myname to /tmp/$mytarget
3. Reproduce the hash manually:
   echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
4. Read the resulting file:
   cat /tmp/<hash>

## Wrong Turns
- Tried reading files in /tmp with random names hoping to stumble on
  the password there were dozens of unrelated files from other levels
- Assumed the filename would be a simple pattern like "bandit23.txt"
  instead of realizing it was computed via hashing
- Ran md5sum without piping input and got stuck waiting for stdin;
  forgot that echo ... | md5sum feeds the string directly

## Concept Extracted
This introduces *deterministic output prediction*. When a script uses
a hash function (MD5, SHA) on known input, you can reproduce its output
without executing the script. This exact technique appears in real-world
scenarios: predicting cache filenames, guessing temporary file paths,
and even some authentication token generation schemes. Also note the
use of cut -d ' ' -f 1 parsing command output into usable values is
a core shell scripting skill.

## Key Commands
- cat /usr/bin/cronjob_bandit23.sh
- echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
- cat /tmp/<hash>

## Verification
Password obtained successfully and verified by SSH login as bandit24.