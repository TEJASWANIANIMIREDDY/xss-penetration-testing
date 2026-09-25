# XSS Penetration Testing - Complete Learning Guide

Comprehensive guide to finding, exploiting, and documenting XSS vulnerabilities.

## Contents

- `HOW_TO_FIND_XSS.md` - Manual XSS testing methodology
- `PAYLOAD_REFERENCE.md` - XSS payload reference with examples
- `XSS_FINDINGS.txt` - Real vulnerabilities found on DVWA
- `/screenshots/` - Proof of exploitation

## Learning Path Completed

### PortSwigger Web Security Academy - 9 Labs

#### APPRENTICE Level (Foundation)
- ✅ Reflected XSS into HTML context with nothing encoded
- ✅ Stored XSS into HTML context with nothing encoded
- ✅ DOM XSS in document.write sink using source location.search
- ✅ DOM XSS in innerHTML sink using source location.search
- ✅ DOM XSS in jQuery anchor href attribute sink using location.search source

#### PRACTITIONER Level (Intermediate)
- ✅ DOM XSS in document.write sink using source location.search inside a select element
- ✅ DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded
- ✅ Reflected DOM XSS
- ✅ Stored DOM XSS

**→ [Detailed Lab Breakdown](PORTSWIGGER_LABS.md)**

### DVWA (Damn Vulnerable Web Application)
- ✓ Reflected XSS - [See screenshots]
- ✓ Stored XSS - [See screenshots]
- ✓ DOM-based XSS - [See screenshots]

## Vulnerabilities Found

### 1. Reflected XSS on DVWA
- **URL:** `/vulnerabilities/xss_r/?name=<script>alert('XSS')</script>`
- **Payload:** `<script>alert('XSS')</script>`
- **Impact:** Arbitrary JavaScript execution
- **Proof:** See `/screenshots/reflected-xss-alert.png`

### 2. Stored XSS on DVWA
- **Form:** Comment submission
- **Payload:** `<img src=x onerror="alert('Stored XSS')">`
- **Impact:** Persistent, affects all users
- **Proof:** See `/screenshots/stored-xss-alert.png`

### 3. DOM-based XSS on DVWA
- **URL:** `/vulnerabilities/xss_d/?default=<img src=x onerror="alert('DOM XSS')">`
- **Payload:** `<img src=x onerror="alert('DOM XSS')">`
- **Impact:** Client-side execution, no server involvement
- **Proof:** See `/screenshots/dom-xss-alert.png`

**→ [Complete DVWA Findings](XSS_FINDINGS.txt)**


## Tools Used

- Burp Suite Community Edition (Proxy, Repeater)
- DVWA (Docker)
- Firefox (with proxy configuration)
- PortSwigger Academy

---

## 📖 How to Use This Repository

### For Learning XSS Testing:
1. Start with **[HOW_TO_FIND_XSS.md](HOW_TO_FIND_XSS.md)** - Learn the methodology
2. Reference **[PAYLOAD_REFERENCE.md](PAYLOAD_REFERENCE.md)** - Choose appropriate payloads
3. Study **[PORTSWIGGER_LABS.md](PORTSWIGGER_LABS.md)** - Understand each lab concept
4. Review **[XSS_FINDINGS.txt](XSS_FINDINGS.txt)** - See real exploitation examples

### For Quick Payload Lookup:
- Jump directly to [PAYLOAD_REFERENCE.md](PAYLOAD_REFERENCE.md)
- Use the decision tree to select payload for your context

### For Testing Methodology:
- Follow [HOW_TO_FIND_XSS.md](HOW_TO_FIND_XSS.md) step-by-step
- Apply to your target application

---

## 🎯 Key Concepts Covered

### XSS Types
- **Reflected XSS** - Payload in URL, server echoes it back
- **Stored XSS** - Payload stored in database, persists for all users
- **DOM-based XSS** - Client-side JavaScript processes input unsafely

### Exploitation Contexts
- HTML body context
- HTML attribute context
- JavaScript string context
- URL context
- Framework-specific contexts (jQuery, AngularJS)

### Defensive Measures
- HTML encoding
- JavaScript encoding
- Input validation
- Content Security Policy (CSP)
- Output encoding

### Payload Techniques
- Basic payloads (`<script>`, `<img>`, `<svg>`)
- Event handlers (`onerror`, `onload`, `onmouseover`)
- Context escaping (breaking out of quotes/tags)
- Filter bypasses
- Real-world attack payloads

---

## 📊 Testing Summary

| Vulnerability | Type | Severity | Status |
|---|---|---|---|
| DVWA Reflected XSS | Reflected | MEDIUM | ✅ EXPLOITED |
| DVWA Stored XSS | Stored | HIGH | ✅ EXPLOITED |
| DVWA DOM XSS | DOM | MEDIUM-HIGH | ✅ EXPLOITED |
| PortSwigger Lab 1-5 | Multiple | - | ✅ COMPLETED |
| PortSwigger Lab 6-9 | Advanced | - | ✅ COMPLETED |

---

## 🔍 What You'll Learn

After working through this material, you'll understand:

✅ How XSS vulnerabilities occur in web applications  
✅ How to systematically find XSS injection points  
✅ How to craft payloads for different contexts  
✅ How to use Burp Suite to test for XSS  
✅ The difference between Reflected, Stored, and DOM XSS  
✅ How to document security findings professionally  
✅ Common defensive mechanisms and their limitations  

---

## 🚨 Disclaimer

**Educational purposes only.** All testing performed on:
- Intentionally vulnerable applications (DVWA)
- Authorized lab environments (PortSwigger Academy)
- Applications you have explicit permission to test

**Unauthorized access to computer systems is illegal.** Only test applications you own or have written permission to test.

---

---

## 🏆 Skills Demonstrated

By completing this project, I've demonstrated:

- **Security Testing** - Manual XSS vulnerability discovery
- **Web Application Exploitation** - Real-world attack scenarios
- **Technical Documentation** - Professional findings reports
- **Tools Proficiency** - Burp Suite, DVWA, Firefox proxy
- **Methodology** - Systematic testing approach
- **Problem Solving** - Bypassing filters and defenses
- **Communication** - Clear explanation of vulnerabilities

---

## 📖 Additional Resources

- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [DVWA GitHub](https://github.com/digininja/DVWA)
- [Burp Suite Community Edition](https://portswigger.net/burp/communitydownload)

---

## 👤 Author

Tejaswani Animireddy 
GitHub: [@tejaswanisingh](https://github.com/tejaswanisingh)

---

**Last Updated:** September 24, 2026  
**Status:** Complete ✅
