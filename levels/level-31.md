# Bandit Level 31 → 32

## Problem
A bare git repository lives on the server, accessible over SSH. You must
clone it to your *local machine*, inspect its history, and recover a
password that was committed and then removed.

## Approach
1. Clone from your local terminal (not from inside the wargame session):
   git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
2. Password prompt = bandit31's password
3. ls shows README.md; reading it says the password was removed
   ("key redacted" style)
4. git log -p walk the full commit history with diffs
5. An early commit contains the actual password; later commits delete it

## Wrong Turns
- Tried cloning while still SSH'd into the wargame box failed because
  no git client there, and the instructions explicitly say local machine
- Read only the current file contents and concluded the password was
  simply missing nearly gave up before remembering git stores every
  version of every file ever committed
- Ran git log without -p and saw only commit messages, not the
  deleted content

## Concept Extracted
Git preserves complete history. Deleting sensitive data in a later commit
does not remove it from the repository it remains reachable through
earlier commits. This is one of the most common real-world leaks: secrets
committed once stay forever unless the history is rewritten and force-
pushed. In bug bounty work, scanning public repos for accidentally
committed API keys and passwords is a whole category of findings. Tools
like trufflehog and gitleaks automate exactly what you did manually
here with git log -p.

## Key Commands
- git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
- git log --oneline
- git log -p
- git show <commit-hash>

## Verification
Password obtained from a historical commit and verified by SSH login as
bandit32.