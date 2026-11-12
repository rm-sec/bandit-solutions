# Bandit Level 3 → 4

## Problem
Find a hidden file inside the inhere directory, which appears empty.

## Approach
1. cd inhere, then run ls nothing shows up
2. Re-list with ls -a to reveal dotfiles
3. Read the single hidden file found

## Wrong Turns
- Ran plain ls, saw an empty directory, and assumed I was in the
  wrong location, went back and re-checked the objective
- Tried find . -name "*" which missed the target entirely because
  the glob didn't match leading-dot names

## Concept Extracted
Names beginning with . are excluded from default listings and from
unquoted * glob expansion. This isn't cosmetic, it's why config
files like .bashrc, .ssh/, and .gitignore don't clutter normal
viewing, and why recon tools always enumerate dotfiles explicitly.

## Key Commands
- cd inhere
- ls -a
- cat .hidden

## Verification
Password used successfully to SSH in as bandit4.