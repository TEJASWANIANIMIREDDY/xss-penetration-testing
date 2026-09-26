# HOW TO FIND XSS MANUALLY - Complete Guide

## Overview
Cross-Site Scripting (XSS) happens when user input is reflected back or stored without sanitization.
This guide shows you HOW TO FIND IT systematically.

---

## STEP 1: IDENTIFY INPUT FIELDS

**Look for any place where users can enter data:**

### Common Injection Points
- [ ] URL parameters (e.g., `?search=`, `?id=`, `?name=`)
- [ ] Form fields (login, search, comments)
- [ ] Search boxes
- [ ] Comment sections
- [ ] User profiles
- [ ] Chat messages
- [ ] File upload descriptions
- [ ] Email fields
- [ ] Any text input

### Example from DVWA:

Found at: http://dvwa.local/vulnerabilities/xss_r/

Input field: "name" parameter in URL

Type: Query string parameter


---

## STEP 2: TEST WITH HARMLESS INPUT FIRST

**Always start with normal input to understand how the app works:**

### What to Do:
1. Type: `hello` in search box
2. Submit form
3. Observe: WHERE does it appear on the page?

### Questions to Answer:
- [ ] Does the input appear on the page?
- [ ] Is it inside HTML tags, attributes, or JavaScript?
- [ ] Is it encoded (e.g., `&lt;` instead of `<`)?
- [ ] Does the page reload or use AJAX?

### DVWA Example:

Input: hello

Output on page: "Hello hello"

Location: Inside html tags

Encoding: None (raw output)


---

## STEP 3: TEST WITH SPECIAL CHARACTERS

**Try breaking out of the current context:**

### Test These:

< > " ' { } ( )

### What Happens:
- Do angle brackets disappear?
- Do quotes get escaped?
- Does page break or show error?

### DVWA Reflected XSS Test:

Input:

Result: Angle bracket appears in output

Conclusion: No filtering - likely vulnerable

---

## STEP 4: IDENTIFY THE CONTEXT

**Understanding WHERE your input goes is critical:**

### Context Types:

#### A) HTML Body Context
```html
<p>Your input: USER_INPUT_HERE</p>
```
**Test:** `<b>test</b>` - does text appear bold?

#### B) HTML Attribute Context
```html
<input value="USER_INPUT_HERE">
```
**Test:** `" onclick="alert('XSS')` - does attribute close?

#### C) JavaScript Context
```html
<script>
  var search = "USER_INPUT_HERE";
</script>
```
**Test:** `"; alert('XSS'); //' - can you break out of string?

#### D) URL Context
```html
<a href="USER_INPUT_HERE">Click</a>
```
**Test:** `javascript:alert('XSS')` - does it execute?

### How to Identify Context:

1. **Open Developer Tools** (F12)
2. **Find your input in the HTML**
3. **Determine the context** (body, attribute, script, URL)
4. **Choose appropriate payload** (see PAYLOAD_REFERENCE.md)

### DVWA Example:

Input: hello
HTML Output: <pre>Hello hello</pre>
Context: HTML body (inside `<pre>` tag)
Payload Strategy: Use <img> or <script> tags

---

## STEP 5: CRAFT PAYLOAD FOR THE CONTEXT

**Different contexts need different payloads:**

For HTML Body:
<img src=x onerror="alert('XSS')"> ````

For HTML Attribute:
" onmouseover="alert('XSS')" x="

For JavaScript:
"; alert('XSS'); //

For URL:
javascript:alert('XSS')

---

STEP 6: TEST PAYLOAD

Submit your payload and watch what happens:

Using Browser Directly:
Type payload in search box
Submit
Does alert pop up? → VULNERABLE ✓
Using Burp Suite Repeater:
Capture request in Proxy
Send to Repeater
Modify parameter with payload
Check response for unencoded payload
If unencoded → VULNERABLE ✓

DVWA Testing:

Payload: <script>alert('XSS')</script>
Method: URL parameter
Result: Alert box appeared
Conclusion: VULNERABLE

---

STEP 7: DETERMINE PAYLOAD TYPE

Understand what type of XSS you found:

Reflected XSS

1.Payload in URL
2.Server echoes it back
3.Triggers immediately

Test: Change URL parameter, alert changes


Stored XSS

1.Payload submitted via form
2.Stored in database
3.Triggers for all users

Test: Refresh page, payload still triggers


DOM-based XSS

1.Payload in URL
2.JavaScript processes it
3.No server response needed

Test: Check Network tab - no server reflection

DVWA Examples:

Reflected:

URL: ?name=<img src=x onerror=alert('XSS')>
Result: Alert on first load
Payload in server response: YES
Type: REFLECTED ✓

Stored:

Form: Name + Message
Action: Submit comment with payload
Result: Alert on page reload for all users
Payload in database: YES
Type: STORED ✓

DOM:

URL: ?default=<img src=x onerror=alert('XSS')>
Result: Alert triggered by JavaScript
Payload in server response: NO
Type: DOM ✓

---

STEP 8: CONFIRM THE EXPLOIT

Make sure it's really XSS, not something else:

Checklist:
 Alert box (or console output) confirmed
 Payload appears in page source (Reflected/Stored)
 Payload NOT HTML-encoded (e.g., &lt; is safe, < is vulnerable)
 No Content-Security-Policy blocking
 Multiple payloads work, not just one

DVWA Confirmation:
✓ Alert appeared
✓ <script> tag in HTML source (not &lt;script&gt;)
✓ No CSP headers
✓ Multiple payloads triggered alerts
Conclusion: Confirmed Vulnerable

---

STEP 9: DOCUMENT YOUR FINDINGS

Record exactly what you found for the report:

Template:
Vulnerability: XSS (Type: REFLECTED/STORED/DOM)
URL: [exact vulnerable URL]
Parameter: [which field is vulnerable]
Payload: [exact payload used]
Context: [HTML body/attribute/JavaScript/URL]
Severity: [HIGH/MEDIUM/LOW]
Screenshot: [attach proof]

DVWA Documentation:
Vulnerability: Reflected XSS
URL: http://dvwa.local/vulnerabilities/xss_r/?name=%3Cscript%3Ealert('XSS')%3C/script%3E
Parameter: name
Payload: <script>alert('XSS')</script>
Context: HTML body (inside <pre> tag)
Severity: MEDIUM (requires user to click link)
Screenshot: alert_box.png, burp_response.png

---

STEP 10: LOOK FOR DEFENSES (Don't Stop After First Find)

Real apps often have multiple layers:

Test for Encoding:
Input: 
Output: &lt;
Conclusion: HTML encoding active (safer)

Test for Filtering:
Input: <script>alert('XSS')</script>
Output: alert('XSS')  [<script> removed]
Conclusion: Tag filtering active
Try Bypass: <img src=x onerror=alert('XSS')>

Test for WAF (Web Application Firewall):
Input: <iframe src="javascript:alert('XSS')">
Output: Error page or blocked
Conclusion: WAF present
Action: Try URL encoding, case variations, etc.

COMMON MISTAKES TO AVOID

❌ Don't: Assume payload failed if alert doesn't pop
✅ Do: Check browser console (F12 → Console tab) for errors

❌ Don't: Give up after one payload
✅ Do: Try multiple payloads (see PAYLOAD_REFERENCE.md)

❌ Don't: Miss DOM XSS because you only test forms
✅ Do: Test URL parameters even if no form visible

❌ Don't: Stop at finding one XSS
✅ Do: Test ALL input fields systematically

REAL DVWA TESTING WALKTHROUGH
Finding Reflected XSS:
Step 1: Found URL parameter "name" in /xss_r/
Step 2: Tested with "hello" → appeared on page
Step 3: Tested with "<" → appeared in output
Step 4: Context: HTML body (inside <pre>)
Step 5: Payload: <script>alert('XSS')</script>
Step 6: Submitted → Alert popped
Step 7: Determined: REFLECTED (URL echoed by server)
Step 8: Confirmed: Unencoded, multiple payloads work
Step 9: Documented: Full URL, payload, screenshot
Result: ✓ VULNERABLE

Finding Stored XSS:
Step 1: Found form with "name" and "message" fields
Step 2: Tested with normal input "hello" → stored in DB
Step 3: Tested with "<" → appeared in output
Step 4: Context: HTML body (inside comment display)
Step 5: Payload: <img src=x onerror="alert('Stored XSS')">
Step 6: Submitted form → Alert popped immediately
Step 7: Determined: STORED (refreshing page re-triggers alert)
Step 8: Confirmed: Alert triggers for all users viewing page
Step 9: Documented: Form location, payload, persistent behavior
Result: ✓ VULNERABLE

Finding DOM XSS:
Step 1: Found XSS (DOM) page with dropdown
Step 2: Tested form with normal input → nothing special
Step 3: Tested URL parameter: ?default=hello
Step 4: Context: JavaScript processes URL parameter
Step 5: Payload: <img src=x onerror="alert('DOM XSS')">
Step 6: Injected in URL → Alert popped
Step 7: Determined: DOM (no form submission, URL only)
Step 8: Confirmed: JavaScript processes parameter directly
Step 9: Documented: URL injection point, no form involved
Result: ✓ VULNERABLE

---

SUMMARY: THE XSS HUNTING PROCESS
Find input fields → Identify ALL places users can input data
Test normal input → Understand how app processes data
Test special chars → See if anything breaks
Identify context → Where does input appear? (body/attr/JS)
Choose payload → Match payload to context
Test payload → Does it execute?
Determine type → Reflected/Stored/DOM?
Confirm vulnerable → Multiple proofs, not just luck
Document it → Full details for report
Look for defenses → What stops you? Can you bypass?

Apply this process to ANY web app and you'll find XSS!(if present) ✓
