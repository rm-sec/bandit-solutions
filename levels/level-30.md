# Bandit Level 30 → 31

## Problem
A git repository is accessible over SSH. Unlike Level 29 (where the
password lived on another branch), here the README says "just an epmty
file... muahaha" and there are no other branches. The hidden data lives
somewhere else in git's object model.

## Approach
1. Create a writable temp directory (mktemp -d) you cannot write to
   your own home directory reliably for this
2. git clone ssh://bandit30-git@localhost/home/bandit30-git/repo
   (enter bandit30's password when prompted)
3. cat README.md taunt message, nothing useful
4. git branch -a only master, no help this time
5. git tag reveals a tag named secret
6. git show secret prints the tag's contents: the password

## Wrong Turns
- Tried cloning directly into my home directory and got permission
  errors; had to make a /tmp working dir first
- Ran git log expecting history-based secrets like Level 28 nothing
  unusual in the commit messages this time
- Nearly gave up after checking branches, not realizing git has three
  namespaces where data hides: commits, branches, and tags

## Concept Extracted
Git stores far more than your working files. *Tags* are lightweight or
annotated pointers into history, often used to mark release versions
but they can carry arbitrary messages and objects that never appear in
the checked-out tree. This is why source-code review for security means
inspecting the whole repository, not just the current branch: secrets
leak via old commits, dangling blobs, stashes, and tags constantly.
Real-world bug bounty hunters routinely recover API keys from forgotten
tags in public repos.

## Key Commands
- mktemp -d && cd $_
- git clone ssh://bandit30-git@localhost/home/bandit30-git/repo
- git tag
- git show secret

## Verification
Password obtained successfully and verified by SSH login as bandit31.