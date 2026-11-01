
---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
|---|---|
| 🖥️ Host | Windows (VMware Workstation) |
| 🐉 Guest OS | Kali Linux |
| 🎯 Target | OWASP Juice Shop (Docker, `bkimminich/juice-shop`) |
| 🌐 Target URL | http://localhost:3000 |
| 🧰 Tools | Nmap, Nikto, curl, Burp Suite Community, Firefox DevTools |

---

## 🔎 Findings Summary

| # | Finding | Severity |
|---|---|---|
| 1 | SQL Injection — Authentication Bypass | 🔴 High |
| 2 | DOM-Based Cross-Site Scripting (XSS) | 🟠 Medium |
| 3 | Sensitive Data Exposure (`/ftp` directory) | 🟠 Medium |
| 4 | Security Misconfiguration (missing headers, permissive CORS) | 🟡 Low |

Full details for each finding — affected component, risk, evidence, recommended mitigation — are in [report.md](report.md).

---

## 🪜 Methodology

**Step 1 — Reconnaissance**
Ran Nmap, `curl -I`, and Nikto against the target to identify the open port and inspect HTTP response headers.

**Step 2 — Authentication testing**
Tested the login form for SQL injection using the payload `' or 1=1--`, successfully bypassing authentication and logging in as the administrator account.

**Step 3 — Input validation testing**
Tested the search feature for XSS using `<iframe src="javascript:alert(\`xss\`)">`, confirming unsanitized input is rendered and executed in the DOM.

**Step 4 — Access control & file exposure**
Discovered a publicly accessible `/ftp` directory containing an internal document explicitly marked confidential.

**Step 5 — Configuration review**
Reviewed response headers for missing security controls (CSP, HSTS) and overly permissive CORS.

All request/response pairs were captured in Burp Suite; screenshots for each finding are in the [`evidence/`](evidence) folder.

---

## 💡 What I Learned

### 1. Authentication logic is fragile without input validation
A single unsanitized field in a login query can lead to a complete authentication bypass.

### 2. Client-side rendering needs sanitization too
XSS isn't just a server-side problem — unsanitized data rendered in the DOM is just as exploitable.

### 3. Access control has to be explicit
Files aren't private just because no one links to them — `/ftp` was reachable by anyone who guessed the path.

### 4. Headers matter
Missing security headers don't cause a breach on their own, but they remove layers of defense that make other attacks easier.

### 5. Evidence-first documentation
Capturing the exact request/response in Burp, rather than just describing a bug, makes a finding verifiable and professional.

---

## 🔐 Security & Ethical Use

This assessment was performed for **educational purposes only**, against an intentionally vulnerable training application (OWASP Juice Shop) running entirely inside a private, isolated lab that I own and control. No real or third-party systems were tested.

---

## 🔗 Tools & Resources

- **OWASP Juice Shop:** https://owasp-juice.shop
- **Burp Suite Community:** https://portswigger.net/burp
- **Nmap:** https://nmap.org
- **Nikto:** https://cirt.net/Nikto2

---

## 👤 Author

**Mubassir**
Security Intern, SWYNEX Technologies — Task 1

---

## 📌 Project Information

**Program:** SWYNEX Technologies Internship | **Task:** 1 — Security Fundamentals Assessment | **Repository:** GitHub
