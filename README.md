# XSS Penetration Testing - Complete Learning Guide

Comprehensive guide to finding, exploiting, and documenting XSS vulnerabilities.

## Contents

- `HOW_TO_FIND_XSS.md` - Manual XSS testing methodology
- `PAYLOAD_REFERENCE.md` - XSS payload reference with examples
- `XSS_FINDINGS.txt` - Real vulnerabilities found on DVWA
- `/screenshots/` - Proof of exploitation

## Learning Path Completed

### PortSwigger Web Security Academy
- ✓ Lab 1: Reflected XSS in HTML body
- ✓ Lab 2: Reflected XSS in HTML attribute
- ✓ Lab 3: Reflected XSS in JavaScript
- ✓ Lab 4: XSS with filters and encoding
- ✓ Lab 5: Advanced XSS with strict filters

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

## Key Learnings

✓ Understood all 3 XSS types and how they differ  
✓ Learned to identify vulnerable injection points  
✓ Mastered payload crafting and encoding  
✓ Practiced with Burp Suite Repeater  
✓ Documented findings professionally  

## Tools Used

- Burp Suite Community Edition (Proxy, Repeater)
- DVWA (Docker)
- Firefox (with proxy configuration)
- Notepad/VS Code

## Disclaimer

Educational purposes only. All testing performed on intentionally vulnerable applications with authorization. For authorized security testing only.
