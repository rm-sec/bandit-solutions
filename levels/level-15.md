# Bandit Level 15 → 16

## Problem
Port 30001 speaks SSL/TLS instead of plain TCP. Netcat from Level 14
fails because it cannot perform the TLS handshake. You must use an
SSL-capable client to submit bandit15's password and receive bandit16's.

## Approach
1. openssl s_client -connect localhost:30001 establish a TLS connection
2. Wait for the certificate dump to finish (ignore the noise)
3. Paste bandit15's password after the prompt appears
4. The server responds with "Correct!" followed by bandit16's password

## Wrong Turns
- Ran nc localhost 30001 connection opened but the server never
  responded; netcat doesn't know how to do a TLS handshake
- Panicked at the wall of certificate output (depth=0 CN = bandit,
  verify error:num=18:self signed certificate) thinking something was
  broken that's just normal self-signed cert behavior
- Tried piping the password immediately without waiting for the
  handshake to complete, which caused the input to arrive before the
  server was ready to read it

## Concept Extracted
SSL/TLS wraps TCP traffic in an encrypted tunnel. A service speaking
TLS will silently reject non-TLS clients, there is no error message,
just silence. openssl s_client is the manual equivalent of opening an
HTTPS connection. This exact command is used daily by security teams to
inspect certificates, check cipher suites, and test whether expired or
misconfigured certs are causing outages. The self-signed certificate
warning is also important context: production systems should have CA-
signed certs; self-signed ones are common in labs and internal tools.

## Key Commands
- openssl s_client -connect localhost:30001
- cat /etc/bandit_pass/bandit15 | openssl s_client -connect localhost:30001 -quiet
- -quiet flag suppresses the certificate dump for cleaner output

## Verification
Password obtained successfully and verified by SSH login as bandit16.