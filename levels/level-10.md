# Bandit Level 10 → 11

## Problem
The file data.txt appears as binary garbage it contains non-printable
characters mixed with readable text. The password is hidden somewhere
inside among the noise.

## Approach
1. cat data.txt | strings extract only printable character sequences
   from the binary data
2. The output is a list of readable strings; one of them is the password

## Wrong Turns
- Tried cat data.txt and got a terminal full of control characters
  (bell sounds, cursor jumps, garbage) no useful output
- Tried file data.txt confirmed it was "data", but didn't help
  extract the readable content
- Tried grep directly on the binary file it printed "Binary file
  data.txt matches" but didn't show the matching line

## Concept Extracted
strings extracts printable text from binary files. It's the first
command you reach for when you see "binary data" in a CTF. In real
pentesting, strings is essential for:
- Extracting URLs, IPs, and credentials from malformed executables
- Mining malware samples for indicators of compromise (IOCs)
- Forensic analysis of disk images and memory dumps

The password was one of the readable strings embedded in the binary blob.

## Key Commands
- cat data.txt | strings
- strings data.txt (equivalent, more efficient)

## Verification
Password obtained successfully and verified by SSH login as bandit11.