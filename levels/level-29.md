# Bandit Level 29 → 30

## Problem
Another Git repo, but this time the current branch (master) contains
no password only a note saying passwords don't belong in production.
The secret lives on a different *branch*.

## Approach
1. Clone into a scratch dir: git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
2. cat README.md says "no passwords in production!" (a deliberate hint)
3. git log -p dead end; history shows no leaked password either
4. git branch -a reveals remote branches including dev
5. Check out the dev branch and read its README:
   git checkout dev && cat README.md

## Wrong Turns
- Ran git log -p first, assuming the answer was in commit history like
  Level 28 the history here is clean, so I burned time re-checking
  diffs that contained nothing useful
- Tried git checkout dev before running git branch -a; didn't know
  the branch name existed until I enumerated them
- Overlooked the word "production" in the README on first read it was
  the actual clue pointing toward non-production branches

## Concept Extracted
Git has *multiple dimensions of hidden data*: commit history (Level 28),
branches (Level 29), and ignored/untracked files (Level 30). Real-world
secret scanning tools like trufflehog and gitleaks deliberately crawl
all branches, not just HEAD, because developers routinely stash test
credentials or debug tokens on feature branches and forget about them.
Reading an error message or comment literally "not in production"
is also a genuine recon skill; hints hide in plain sight.

## Key Commands
- git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
- git branch -a
- git checkout dev
- cat README.md

## Verification
Password obtained successfully and verified by SSH login as bandit30.