# Bandit Level 11 → 12

## Problem
The file data.txt contains text where every lowercase letter has been
shifted by 13 positions (ROT13 cipher). Uppercase letters and non-letters
are unchanged.

## Approach
1. Recognize ROT13 from the instruction ("rotated by 13 positions")
2. Use tr to translate characters back:
   cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
3. The output is the decoded password

## Wrong Turns
- Tried base64 -d data.txt assumed it was encoding again, but got
  garbage. Not every "scrambled" file is Base64
- Tried strings data.txt same problem as before; this isn't binary
  noise, it's readable text that's just shifted
- Didn't know about tr initially and tried writing a Python script to
  shift each character manually overkill when a one-liner exists

## Concept Extracted
ROT13 is a *substitution cipher* with a fixed key of 13. Because the
English alphabet has 26 letters, applying ROT13 twice returns the
original text, it's its own inverse. This makes it useless as security,
but it's historically used to obfuscate spoilers, email addresses, and
simple secrets. In CTFs, recognizing substitution ciphers early saves
time. The general lesson: always check what kind of transformation
was applied before trying to reverse it.

## Key Commands
- cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
- tr 'a-zA-Z' 'n-za-mN-ZA-M' < data.txt (equivalent)

## Verification
Password obtained successfully and verified by SSH login as bandit12.