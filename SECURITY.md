# Security Policy

## Supported versions

| Component | Version | Supported |
|-----------|---------|-----------|
| Python SDK (`tracelify-sdk`) | 1.0.x | ✅ |
| Other SDKs and backend | `main` branch | ✅ |
| Older releases | — | ❌ |

## Reporting a vulnerability

**Please do not open a public issue for security problems.**

Report vulnerabilities privately through GitHub:
**Security → Report a vulnerability** on this repository
([direct link](https://github.com/SurajsinghBayas/tracelify-trace-sdk/security/advisories/new)).

Please include:
- the affected component (SDK language, backend or dashboard) and version
- steps to reproduce, or a proof of concept
- the impact you expect

## What to expect

- Acknowledgement within **3 business days**
- An initial assessment within **7 days**
- A fix or mitigation plan for confirmed issues, with credit to you in the release notes unless you prefer to stay anonymous

## Scope

In scope: the SDKs in this repository, the FastAPI ingestion backend, DSN/key validation, and the dashboard.
Out of scope: issues in third-party dependencies (please report those upstream), and deployments you run with insecure settings such as default secrets.
