# Bandit Level 21 → 22

## Problem
A cron job runs as bandit22 every minute. Its output is the password,
but the cron job redirects output to /dev/null, so you can't see it
in logs. You must read the cron configuration to find the script,
then read the temporary file the script writes to.

## Approach
1. ls /etc/cron.d/ find cronjob_bandit22
2. cat /etc/cron.d/cronjob_bandit22 reveals the script path
3. cat /usr/bin/cronjob_bandit22.sh reveals what the script does:
   copies bandit22's password to /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
4. cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv the password

## Wrong Turns
- Tried crontab -l and got nothing that only shows your cron jobs,
  not system-wide ones in /etc/cron.d/
- Waited for the cron job to "run and show output" didn't realize the
  output was redirected to /dev/null and the result was already sitting
  in /tmp before I even logged in
- Searched all of /etc for cron configs instead of going straight to
  /etc/cron.d/ wasted ~5 minutes

## Concept Extracted
Cron is a *time-based job scheduler* it runs commands automatically
at specified intervals. System cron jobs live in /etc/cron.d/,
user cron jobs in crontab -e. In real pentesting, cron misconfigurations
are a top privilege-escalation vector: any writable file in a cron job's
path can let you inject commands that run as root. This is one of the
most common findings on real Linux systems.

## Key Commands
- ls /etc/cron.d/
- cat /etc/cron.d/cronjob_bandit22
- cat /usr/bin/cronjob_bandit22.sh
- cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

## Verification
Password obtained successfully and verified by SSH login as bandit22.