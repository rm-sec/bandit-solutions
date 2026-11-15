# Bandit Command Cheatsheet

## Core Navigation

| Task | Command |
|------|---------|
| List all files including hidden | ls -a |
| List with details | ls -lh |
| Change directory | cd /path/to/dir |
| Show current directory | pwd |
| Go to home | cd ~ |
| Read a file named - | cat ./- |
| Read filename with spaces | cat "file name" or cat file\ name |

*Trap I hit:* .filename files are invisible to plain ls. Cost me time at level 3.

---

## File Reading

| Task | Command |
|------|---------|
| Read text file | cat filename |
| Read large file paged | less filename |
| First 10 lines | head filename |
| Last 10 lines | tail filename |
| Extract readable text from binary | strings filename |

*Trap I hit:* cat on a binary dumps control characters into my terminal. Use
strings instead.

---

## Text Processing

| Task | Command |
|------|---------|
| Search for pattern | grep "word" filename |
| Case-insensitive | grep -i "word" filename |
| Count occurrences | grep -c "word" filename |
| Sort lines | sort filename |
| Remove duplicates | sort \| uniq |
| Lines appearing exactly once | sort \| uniq -u |
| Count unique values | sort \| uniq -c \| sort -n |

*Trap I hit:* uniq without sort gives wrong results silently. It only compares
adjacent lines.

---

## Encoding & Obfuscation

| Task | Command |
|------|---------|
| Base64 encode | base64 filename |
| Base64 decode | base64 -d filename |
| ROT13 both cases | tr 'A-Za-z' 'N-ZA-Mn-za-m' |
| MD5 hash | md5sum filename |
| SHA256 hash | sha256sum filename |

*Traps I hit:*
- Ran base64 data.txt when I needed -d. Encoded instead of decoded.
- Used tr 'a-z' 'n-za-m' and missed uppercase. Password had capitals.
- Assumed "encoded" meant base64. Check the character set before choosing a decoder.

---

## Compression & File Identification

| Task | Command |
|------|---------|
| Reverse a hexdump | xxd -r data.txt > out |
| Identify true file type | file out |
| Decompress gzip | mv out out.gz && gzip -d out.gz |
| Decompress bzip2 | bzip2 -d out.bz2 |
| Extract tar (no compression flag) | tar xf out.tar |
| Create temp workspace | mktemp -d |

*Traps I hit:*
- gzip refuses files not ending in .gz. Rename first; the error message lies about
  corruption.
- tar -xzf on a non-gzipped tar. z means gzip. Plain tar xf handles it.
- Never edit or read from home directory copy to /tmp workspace first.

*Principle:* file reads magic bytes. Extensions are decoration. Trust the header.