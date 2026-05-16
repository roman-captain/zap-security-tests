# zap-security-tests

DAST security scanning with OWASP ZAP against OWASP Juice Shop.

## What is DAST?

Dynamic Application Security Testing - ZAP crawls the target and automatically probes for vulnerabilities: XSS, SQL injection, broken headers, insecure cookies, open redirects, and more.

No test scripts needed - ZAP finds issues automatically and produces a report.

## Target

[OWASP Juice Shop](https://juice-shop.herokuapp.com) - an intentionally vulnerable web application created by OWASP for security testing practice.

## Stack

- **OWASP ZAP** - security scanner (runs via Docker)
- **GitHub Actions** - CI/CD

## CI behavior

- Scan runs on every push, PR, and nightly at 06:00 UTC
- ZAP runs directly via `docker run` - no GitHub Action wrapper
- `|| true` keeps pipeline green; Juice Shop is intentionally vulnerable
- Full HTML report uploaded as a CI artifact after every run
- Download the report from the Actions tab to see all findings (Low / Medium / High)

## Run locally

```bash
docker pull ghcr.io/zaproxy/zaproxy:stable

docker run --rm \
  -v $(pwd)/reports:/zap/wrk \
  ghcr.io/zaproxy/zaproxy:stable \
  zap-baseline.py \
  -t https://juice-shop.herokuapp.com \
  -r zap-report.html \
  -a
```

Report will be saved to `reports/zap-report.html`.

## Report findings

| Risk level | Examples |
|------------|---------|
| High | SQL Injection, XSS, Broken Auth |
| Medium | Missing security headers, CSRF |
| Low | Cookie flags, verbose errors |
| Informational | Server info disclosure |
