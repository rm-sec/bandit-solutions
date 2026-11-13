# Bandit Level 9 → 10

## Problem
The file data.txt contains a single line of Base64-encoded text.
Decoding it reveals the password.

## Approach
1. cat data.txt | base64 -d pipe the encoded content into base64
   with the -d (decode) flag
2. The terminal prints the decoded password

## Wrong Turns
- Tried cat data.txt and got gibberish like "Z2V0d2VsbA==" no idea what to do with
  it until I recognized the pattern (ends with ==, uses A-Z, a-z,
  0-9, +, /)
- Tried base64 data.txt (without -d) this re-encoded the already
  encoded data, making it longer and more confusing

## Concept Extracted
Base64 is *encoding*, not encryption. There is no key, no password,
no brute-force possible. It exists to safely transport binary data
through text-only channels (emails, URLs, JSON). Recognizing the format
is the first step, the second is knowing the right command. In real
pentesting, Base64 shows up constantly: obfuscated payloads in web
requests, encoded credentials in config files, and malware samples.

## Key Commands
- cat data.txt | base64 -d
- base64 -d < data.txt (alternative input method)

## Verification
Password obtained successfully and verified by SSH login as bandit10.