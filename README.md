# CodeAlpha_Secure-Coding-Review

**CodeAlpha Cyber Security Internship — Task 3: Secure Coding Review**

A secure code audit of a sample Python/Flask web application, combining automated static analysis (Bandit) with manual line-by-line review against the OWASP Top 10 and CWE Top 25. Ten vulnerabilities were identified, documented, and fully remediated.

---

## 📋 Overview

| | |
|---|---|
| **Target application** | Sample Flask user portal (login, dashboard, profile, file download, admin panel) |
| **Review type** | Static Application Security Testing (SAST) + manual review |
| **Tools used** | [Bandit](https://bandit.readthedocs.io/) 1.9.4, manual inspection |
| **Findings** | 10 vulnerabilities (3 High, 4 Medium, 3 Low) |
| **Result after fix** | 0 High, 0 Medium — verified by re-scan |

---

## 📁 Repository Structure

```
CodeAlpha_SecureCodingReview/
├── README.md
├── Secure_Coding_Review_Report.docx     
├── vulnerable_app/
│   ├── app.py                         
│   └── requirements.txt
├── fixed_app/
│   └── app.py                         
└── analysis/
    ├── bandit_report.txt               
    └── bandit_report_fixed.txt          
```

---

## 🔍 Methodology

1. **Target selection** — a small, self-contained Flask application was built to reflect realistic production patterns (authentication, sessions, file access, admin functions).
2. **Static analysis** — ran Bandit against the source to flag known-insecure API usage.
3. **Manual review** — traced every untrusted input (`request.args`, `request.form`, cookies, request body) from entry point to sink, checked against OWASP Top 10 (2021) and MITRE CWE Top 25.
4. **Remediation** — rewrote the application, resolving each finding with the corresponding secure pattern.
5. **Verification** — re-ran the identical Bandit scan against the fixed code to confirm the High/Medium findings were eliminated with no regressions.

---

## 🚨 Findings Summary

| ID | Finding | Severity | CWE |
|---|---|---|---|
| 1 | Hardcoded secret key | Low | CWE-798 |
| 2 | SQL Injection (login + profile lookup) | Medium | CWE-89 |
| 3 | Weak password hashing (MD5) | High | CWE-327 |
| 4 | Insecure session cookie attributes | Medium | CWE-614 |
| 5 | Cross-Site Scripting via template injection | High | CWE-79 |
| 6 | OS command injection | High | CWE-78 |
| 7 | Path traversal | High | CWE-22 |
| 8 | Insecure deserialization (`pickle`) | High | CWE-502 |
| 9 | Broken access control | High | CWE-862 |
| 10 | Debug mode enabled in production | Medium | CWE-489 |

Full details — description, impact, vulnerable code, and the exact fix for each finding — are in [`Secure_Coding_Review_Report.docx`](./Secure_Coding_Review_Report.docx).

---

## ✅ Key Fixes Applied

- Parameterized SQL queries instead of string concatenation
- Salted, adaptive password hashing (`werkzeug.security`) instead of MD5
- Server-verified sessions instead of client-trusted cookies
- Allow-listed, `shell=False` subprocess calls instead of shell string building
- Path resolution + containment checks instead of raw path joins
- JSON instead of `pickle` for untrusted data
- Secrets loaded from environment variables, never hardcoded
- Debug mode disabled; secure cookie flags enabled

---

## 🧪 Reproducing the Scan

```bash
pip install bandit flask werkzeug markupsafe

# Before remediation
bandit -r vulnerable_app/app.py -f txt

# After remediation
bandit -r fixed_app/app.py -f txt
```

## 👤 Author

CodeAlpha Cyber Security Intern — July 2026
