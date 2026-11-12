# Bandit Level 4 → 5

## Problem
Ten near-identically-named files sit in inhere. Only one contains
readable text; the rest are binary junk.

## Approach
1. cd inhere, list with ls -la ten files, all named -fileNN
2. Run file ./* to classify every file at once instead of opening
   them individually
3. Nine return data; one returns ASCII text read that one

## Wrong Turns
- Started cat ./-file00, then realised I'd have to repeat it nine
  times. Stopped and looked for a way to classify without reading
- Forgot the leading dash again and ran file *, which threw errors
  because the shell fed -file00 to file as an option flag

## Concept Extracted
Filenames tell you nothing about content. The file command reads
magic bytes / actual content rather than trusting extensions or names the same principle behind why malware hides as .jpg and why
defence tooling never relies on filename checks. Also: batch your
commands (file ./*) before iterating manually. Scaling from 10 files
to 10,000 is just good habit.

## Key Commands
- cd inhere
- ls -la
- file ./*
- cat ./-file07
- reset (when binary output corrupts the terminal)

## Verification
Password used successfully to SSH in as bandit5.