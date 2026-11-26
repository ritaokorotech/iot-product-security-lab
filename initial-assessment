# Initial IoT Product Security Assessment

**Date:** November 25, 2026  
**Scope:** OWASP IoTGoat, 192.168.100.10, isolated VirtualBox internal network.  
**Testing workstation:** Kali Linux, 192.168.100.20.

## Executive summary

Initial network, web authentication and SSH security testing identified three security concerns. Findings are based on observed screenshots and user-reported test results. No exploitation of SSH or external network exposure was established. Severity ratings are provisional, not CVSS scores.

## Test results

| Test | Observation |
| --- | --- |
| Network connectivity | Target reachable from Kali |
| HTTPS certificate | Port 443 open; matching issuer and subject; RSA 2048; SHA-256 signature |
| Invalid web credentials | HTTP 403, response body reports invalid credentials |
| Valid web credentials | HTTP 302, session cookie issued; dashboard accessible in browser |
| Cookie flags | Secure and HttpOnly present; SameSite not explicitly set |
| Redirect in Repeater | HTTP 403; request lacked session cookie |
| Basic logout | Login page returned; server-side invalidation not independently verified |
| Password policy | User confirmed acceptance of three-character numeric password |
| SSH service | TCP 22 open; Dropbear SSH protocol 2.0 |
| SSH login | Negotiation failed before authentication; server offered ssh-rsa |

## F001: Weak administrator password policy

**Severity:** Medium (provisional). **Classification:** CWE-521.

**Evidence:** The administration interface accepted the test password `123`, as reported by the tester. The tester reported restoring the password afterward.

**Risk:** Short, predictable passwords increase the risk of unauthorized administrative access.

**Recommendation:** Enforce a suitable minimum length, block common passwords and implement rate limiting. Retest rejection of weak passwords.

## F002: Insecure SSH configuration

**Severity:** Medium (provisional).

**Evidence:** Administration settings showed password authentication and root password login enabled, interface unspecified (all interfaces), port 22. Nmap confirmed `22/tcp open ssh Dropbear sshd (protocol 2.0)` from Kali.

**Risk:** A reachable service allowing root password authentication increases the impact of compromised credentials.

**Recommendation:** Restrict SSH to trusted management networks, use key authentication, disable root password login where feasible and verify recovery access before changes.

**Limitation:** Successful SSH password authentication and exposure outside the lab were not verified.

## F003: Deprecated SSH cryptographic algorithms

**Severity:** Medium (provisional).

**Evidence:** SSH negotiation failed because the server offered only `ssh-rsa` for host key signatures. Algorithm enumeration showed `diffie-hellman-group1-sha1`, `diffie-hellman-group14-sha1`, `ssh-rsa` and `hmac-sha1`. Modern alternatives included `curve25519-sha256@libssh.org` and `hmac-sha2-256`; AES CTR ciphers were also offered. HMAC-SHA1 is legacy, not equivalent to a broken SHA-1 signature.

**Risk:** Legacy algorithms reduce cryptographic assurance and interoperability with modern clients.

**Recommendation:** Update Dropbear, remove obsolete key exchange and signature algorithms, and support modern host key signatures. Retest with a current SSH client.

## Additional observations

The web response included X-Frame-Options SAMEORIGIN, X-Content-Type-Options nosniff, legacy X-XSS-Protection and Cache-Control no-cache. These are observations, not proof of comprehensive protection. TLS cipher enumeration completion was not verified.

## Remaining work

- Firmware extraction and filesystem analysis.
- Software component inventory and vulnerability review.
- Review of embedded secrets and configurations.
- TLS protocol and cipher verification if not already documented.
- Server-side logout invalidation and rate limiting verification.
- Remediation implementation and retesting.

## Evidence handling

Add sanitized screenshots to an evidence directory. Remove session tokens, passwords and unrelated personal information before publishing.
