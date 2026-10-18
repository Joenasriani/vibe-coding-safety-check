# Pass 2: Security and Network Instruction Verification

Date: 2026-10-18
Source: Vibe Coding Safety Check v1.6 Editorial RC (Audits 4-9)
Status: REVIEW COMPLETE / TEXT CORRECTIONS REQUIRED
Scope: Technical accuracy of handbook descriptions and reusable prompts only. No network scans or security tests were executed.

## Results

| Audit | Verdict | Evidence and next correction |
| --- | --- | --- |
| 4 Semgrep / CodeQL | FIX | Source accurately states that coverage depends on rules and languages. Explicitly distinguish Semgrep findings from CodeQL findings, prerequisites, and unverified alerts. Avoid claims that a finding alone proves exploitation. |
| 5 Gitleaks / GitHub secret scanning | FIX | Explicitly separate scans of tracked Git history, current working files, ignored files, and GitHub scanning coverage. A .gitignore file does not remove secrets already committed. Never print suspected secret contents; rotate/revoke exposed credentials promptly. |
| 6 curl / HTTPie | FIX | A GET or HEAD is usually suitable for approved public endpoints but is not guaranteed side-effect-free. Check endpoint behavior and authorization first; do not follow untrusted redirects to internal endpoints. CORS cannot be pronounced correct from a generic response alone; use relevant Origin/preflight context and verify app policy. A single response time is not a load test. |
| 7 dig / nslookup | PASS | Queries of A/AAAA/CNAME/MX/TXT are appropriate where applicable; resolver caching and authoritative delegation need context. No inference that records are globally propagated from one query. |
| 8 OpenSSL | FIX | Using s_client with SNI is necessary but output alone does not necessarily imply certificate hostname validation. Instruct explicit verification of chain trust and expected hostname, and report expiry as of test time. Distinguish TLS/certificate checks from full application security. |
| 9 Nmap | PASS | Existing wording properly requires explicit authorization for target and scope, and distinguishes configuration review from active scanning. Clarify that scanning shared hosting/CDN infrastructure could affect third parties; not authorized by owning a domain alone. |

## Proposed amended copy-ready prompts (do not apply automatically to manuscript)

### Audit 4
Review configured Semgrep rules and GitHub CodeQL language coverage, latest completed scan results, and whether the scans actually ran. Run new static scans only if tools and permissions are available and explicitly authorized. List findings by tool, file, location, severity, and evidence. Separate confirmed issues, suspected issues, and false positives. Report any unscanned code as UNKNOWN. Do not edit the project.

### Audit 5
Review GitHub secret-scanning configuration, .gitignore, and available scan results. With explicit access, check current files and Git history separately using an approved secret scanner. Never disclose secret values in the report or commit scan outputs containing secrets. Give affected locations, exposure window if verifiable, and revocation/rotation recommendations. Do not rewrite history or alter credentials.

### Audit 6
Review approved public routes and identify which can safely be queried. For approved endpoints, use curl or HTTPie to inspect HTTP status, redirects, timing, and relevant headers. Do not assume GET and HEAD have no side effects; do not exercise payment or state-changing routes. Evaluate CORS only against the application's actual cross-origin requirements. Report any untested endpoint as NOT RECHECKED.

### Audit 7
For the authorized domain, query applicable A, AAAA, CNAME, MX, and TXT DNS records using dig or nslookup, and compare with the intended hosting/email configuration. Identify the resolver queried and distinguish an observed answer from propagation or authoritative configuration. Do not change DNS.

### Audit 8
On the authorized HTTPS hostname, inspect certificate dates, issuer, presented chain, and expected hostname using an OpenSSL or curl command that performs certificate validation with the hostname. Report verification failures and evidence. Certificate validity does not by itself establish complete TLS or application security. Do not modify configuration.

### Audit 9
First inspect available firewall, Docker, and hosting exposure configuration. List possible publicly exposed services based on those files. Do not scan IP addresses or hosts unless the owner has explicitly approved the precise target and scope, including any shared/CDN infrastructure restrictions. Separate inferred exposure from actual scanned ports and mark untested exposure UNKNOWN.

## Documentation consulted
- Semgrep findings: https://semgrep.dev/docs/for-developers/resolve-findings-through-app
- CodeQL overview: https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning-with-codeql
- GitHub secret patterns: https://docs.github.com/en/code-security/reference/secret-security/supported-secret-scanning-patterns
- Gitleaks current repository: https://github.com/gitleaks/gitleaks
- curl manual: https://curl.se/docs/manpage.html
- dig manpage: https://manpages.debian.org/dnsutils/dig.1.en.html
- OpenSSL s_client: https://docs.openssl.org/master/man1/openssl-s_client/
- Nmap official port scanning overview: https://nmap.org/book/port-scanning.html

## Boundaries
- No scanners run
- No user systems, websites, ports, or APIs probed
- No handbook/PDF modification
- No publication or payments
- Current manuscript is still a release candidate

Next: Apply confirmed editorial corrections to Audits 4, 5, 6 and 8 in a controlled revision, then continue with remaining audits.
