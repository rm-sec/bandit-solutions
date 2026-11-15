# Bandit Learning Journal

## Dated Entries

### 2026-11-10
- Set up GitHub, VS Code, Python, MySQL
- Created bandit-solutions repo structure
- Started OverTheWire Bandit

### 2026-11-11
- Completed levels 0-3
- Learned about hidden files and SSH basics

### 2026-11-12
- Completed levels 4-6
- Strings and find commands were new concepts

### 2026-11-13
- Completed levels 7-10
- Grep, sort, uniq and base64 covered

### 2026-11-14
- Level 11 (ROT13)
- Wasted time assuming base64 because the level said "encoded". Character set was
  the actual clue plain letters, not A-Za-z0-9+/
- Also forgot uppercase in my first tr command. Half-decoded output made me doubt
  the whole approach before spotting it

  ### 2026-11-15
- Level 12 (nested compression) slowest level so far, took most of the session
- Kept trying to decompress without renaming to .gz/.bz2. The error message sent me
  hunting for file corruption instead of an extension problem
- Hit tar-inside-bzip2 and used tar -xzf out of habit. z means gzip. Wrong flag
- Biggest takeaway: file reads magic bytes, extensions are decoration