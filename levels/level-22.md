# Bandit Level 22 → 23

## Problem
Another cron job runs as bandit23 every minute. This one uses a shell
variable $myname (set to whoami) to build a filename dynamically.
The password ends up in /tmp/<md5hash>, where the hash is of the
string "I am user bandit23".

## Approach
1. cat /etc/cron.d/cronjob_bandit23 find the script path
2. cat /usr/bin/cronjob_bandit23.sh reveals it computes an MD5 hash
   of "I am user $myname" and writes the password to that filename
3. Compute the same hash manually:
   echo "I am user bandit23" | md5sum
4. cat /tmp/<computed_hash> read the password

## Wrong Turns
- Ran the script directly (/usr/bin/cronjob_bandit23.sh) hoping it
  would run as bandit23 got Permission denied because I was still
  bandit22 when executing it
- Assumed the temp file name was random like Level 21 didn't realize
  it was derived from a predictable string until reading the script
carefully
- Tried guessing filenames in /tmp with ls -la /tmp | grep bandit
  nothing matched; the filename was a hash, not a readable name

## Concept Extracted
Shell scripts often use *variables derived from system state* (like
whoami) to generate dynamic paths. If you can predict the variable's
value, you can predict the output location. This is called a
*predictable resource identifier* vulnerability real-world examples
include predictable temp files, session tokens, and API keys generated
from enumerable inputs. Always check whether a "random-looking" value
is actually deterministic.

## Key Commands
- cat /etc/cron.d/cronjob_bandit23
- cat /usr/bin/cronjob_bandit23.sh
- echo "I am user bandit23" | md5sum
- cat /tmp/<resulting_hash>

## Verification
Password obtained successfully and verified by SSH login as bandit23.