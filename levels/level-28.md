# Bandit Level 28 → 29

## Problem
A Git repository exists at ssh://bandit28-git@localhost:2220/home/bandit28-git/repo. The current README file is empty, the password was removed from the latest commit but remains in the commit history.

## Approach
1. Clone the repo: git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
2. Enter the bandit28 password when prompted
3. cd repo
4. cat README the file is empty
5. git log shows the commit history; the password was present in an earlier commit
6. git show 5777f7c... (or whichever commit hash has the password in README) — displays the full diff for that commit, including the deleted password

## Wrong Turns
- Only looked at cat README and assumed the password was missing; didn't realize Git keeps everything forever unless you explicitly prune history
- Tried git reflog first it shows branch movements but not file content; git log is the correct starting point
- Didn't realize the password might be in a deletion commit; git show on the commit where README went from containing text to being empty reveals what was removed

## Concept Extracted
*Git history is permanent* deleted secrets are not deleted, only unreachable until you run garbage collection (which the server won't do). In real bug bounty hunting and pentesting, git log and git show are the first commands you run on any repo you find. Developers frequently commit credentials, delete them, push a new commit, and assume they're safe. They're not. This lesson extends to .env files, config backups, and any file that ever lived in a repo. git log is your friend.

## Key Commands
- git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
- cat README
- git log
- git show <commit-hash>

## Verification
Password obtained successfully and verified by SSH login as bandit29.