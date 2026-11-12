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
