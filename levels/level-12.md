# Bandit Level 12 → 13

## Problem
data.txt contains a hexdump (ASCII representation of binary bytes) of
a file that has been compressed multiple times with different algorithms
(gzip, bzip2, tar). You must reverse the process layer by layer.

## Approach
1. Convert the hexdump back to binary: xxd -r data.txt > data.bin
2. Use file data.bin to identify the format (e.g., "gzip compressed")
3. Decompress it (gunzip, bunzip2, or tar -xf) and rename the output
   to match its actual type
4. Repeat steps 2-3 until you reach a plain text file containing the
   password

## Wrong Turns
- Tried cat data.txt | base64 -d assumed encoding again, but this is
  a hex dump, not Base64. Different format entirely
- Ran gunzip data.bin without renaming to .gz first got "not in
  gzip format" error because file said it was actually bzip2 at that
  layer
- Lost track of which file was which after three decompression layers;
  had to start over and name each intermediate file clearly

## Concept Extracted
This level teaches *layered obfuscation*. Real malware and advanced
CTF challenges stack multiple transformations: encode → compress →
encrypt → compress again. The methodology is always the same:
1. *Identify* the current format using file
2. *Reverse* one layer at a time
3. *Rename* outputs so you never lose track

xxd -r converts hex dumps back to binary the inverse of xxd.
Knowing your compression tools (gzip, bzip2, tar) and when to use
each is fundamental Linux knowledge that applies far beyond CTFs.

## Key Commands
- xxd -r data.txt > data.bin
- file data.bin
- mv data.bin data.gz && gunzip data.gz
- tar -xf data.tar
- Repeat until plaintext emerges

## Verification
Password obtained successfully and verified by SSH login as bandit13.