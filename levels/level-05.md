# Bandit Level 5 → 6

## Problem
A nested inhere directory contains many subdirectories and decoy files.
One file matches three exact criteria: human-readable, 1033 bytes, not
executable.

## Approach
1. cd inhere  the directory itself has nothing visible
2. Use find to search recursively under inhere with all three filters
   stacked: type, size, permissions

## Wrong Turns
- Ran ls -R inhere and got flooded with hundreds of paths manual
  scanning was hopeless
- Tried du -ab inhere to get sizes but had no way to combine that
  with a human-readable / permission check in one pass

## Concept Extracted
find is a query language, not just a file searcher. By chaining
-type f (regular files only), -size 1033c (exact byte count), and
! -executable (no execute bit), you reduce a directory tree full of
noise down to a single result. The c suffix on -size means "bytes" omitting it would match 1K blocks, which is almost never what you
want. This is the pattern for "find the needle in the haystack" in
every CTF and every real-world log/file audit.

## Key Commands
- cd inhere
- find . -type f -size 1033c ! -executable
- cat -- ./inhere/maybehere07/.file2

## Verification
Password obtained successfully and verified by SSH login as bandit6.