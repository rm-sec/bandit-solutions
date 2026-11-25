# Bandit Level 32 → 33

## Problem
A setuid binary (bandit33) runs a Perl script that prints the next
password when called with the correct argument. But it runs with Perl's
*taint mode* (-T) enabled, which blocks unsafe operations on
user-supplied input including passing your argument directly to the
shell command that reveals the password.

## Approach
1. cat bandit33 (or run it without args) see the Perl source and
   notice -T / $> != 1 checks
2. Understand that taint mode rejects arguments containing shell
   metacharacters or unvalidated input
3. Bypass using an *empty environment variable trick*: Perl considers
   some internally-derived values untainted. Pass an argument that
   matches a strict regex pattern so Perl "launders" it into an
   untainted value
4. Run: ./bandit33 oGvTeYNXJF86bDJVWmLwICGsSjzDbd5s (the expected
   hardcoded argument from the script)

## Wrong Turns
- Ran ./bandit33 $(cat /etc/bandit_pass/bandit33) got "Insecure
  dependency in system()" fatal error; didn't understand why my input
  was rejected
- Tried injecting shell metacharacters like ; id hoping for command
  injection taint mode blocked every attempt because tainted data can
  never reach system()
- Spent time trying to edit the Perl script itself before realizing it
  was owned by bandit33 and not writable

## Concept Extracted
*Perl taint mode* is a defensive feature: any data from outside the
program (argv, env vars, file reads) is marked "tainted" and cannot be
used in dangerous operations unless explicitly validated through a
regex capture group. This is one of the earliest examples of automatic
input-validation enforcement in a language runtime. The lesson generalizes
far beyond Perl: modern equivalents include prepared statements in SQL,
Content Security Policy headers, and type-checked deserialization. When
a security control blocks you, first read its exact rules the bypass
is almost always documented as the intended escape hatch.

## Key Commands
- cat ~/bandit33 (read the Perl source)
- perl -c bandit33 (syntax-check without executing)
- ./bandit33 <correct-argument>

## Verification
Password obtained successfully and verified by SSH login as bandit33.