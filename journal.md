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

### 2026-11-16
- Levels 13-14 (SSH key auth, then nc to a port)
- chmod 600 on id_rsa after two failed logins. Read the rejection as a passphrase
  problem when it was purely permissions
- Made it worse with chmod 777 before thinking. Access grants and trust checks are
  different things
- Level 14 taught me services don't prompt. Stared at a blank terminal waiting for
  one that never came

### 2026-11-17
- Levels 15-16 (TLS connection, then port scanning)
- Retried nc repeatedly at level 15 instead of reading "SSL enabled" properly.
  Also forgot -quiet and lost the response under certificate dump
- First real nmap use. Scanned without -sV initially and got a wall of "unknown"
- Echo ports returned my own password back and briefly convinced me I'd found the
  right one
- Copied the RSA key into home directory, hit the permission trap from level 13 again

### 2026-11-18
- Levels 17-19 (diff, non-interactive SSH, first SUID)
- Tried comparing two large files by eye before reaching for diff. Ten minutes lost
- Grabbed the < line instead of > and had to re-read diff's direction convention
- Level 18: logged in four times hoping the logout was flaky. Repeating a failing
  action isn't debugging. Passing the command inline solved it immediately
- Level 19 SUID finally clicked after running id inside the binary

### 2026-11-19
- Levels 20-22 (listener + SUID, cron world-readable leak)
- Two-terminal coordination felt awkward. Started suconnect first and watched it
  hang with no explanation of why
- Port numbers have to match across both commands and mine didn't at first
- Level 21: ran crontab -l and got permission denied. That lists my jobs, not
  system-wide ones. Different location entirely
- Almost dismissed /tmp as noise because the filename was a hash, not something
  obvious like "password"

### 2026-11-20
- Levels 23-24 (predictable hash filename, writing my own script into cron's path)
- Hashed "I am user bandit22" while logged in as bandit22. Got my own password back,
  which was useless. The script's identity determines the target, not mine
- Level 24 was the highlight. Forgot the shebang, got silent failure with no error
- Then left the output directory non-writable so bandit24 couldn't write there
  invisible because stderr went to /dev/null
- Checked after ten seconds, saw empty, nearly abandoned a correct approach. It runs
  once per minute
- First level where I felt like I was attacking something rather than following steps