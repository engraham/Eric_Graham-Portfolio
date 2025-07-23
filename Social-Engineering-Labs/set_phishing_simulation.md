# Lab 5: Social Engineering Toolkit (SET) — Phishing Simulation

---

## Objective

This lab demonstrates a basic phishing simulation using the **Social Engineering Toolkit (SET)** to:
- Clone a legitimate login page
- Craft a fake phishing email
- Capture credentials in a controlled lab environment

This shows how easily attackers exploit human weaknesses to gain an initial foothold.

---

## Lab Setup

- **Attacker:** Kali Linux VM with SET installed
- **Victim:** Simulated user with browser
- **Network:** Local NAT network only
- **Tools:** SET, Python web server, netcat, browser

---

## Steps

### 1️⃣ Launch SET

```bash
sudo setoolkit
```

![setoolkit](screenshots/setoolkit/sudo_setoolkit.png)


Choose 1) Social-Engineering Attacks

Then 2) Website Attack Vectors

Then 3) Credential Harvester Attack Method

Then 2) Site Cloner

2️⃣ Clone a Target Site
For demonstration, clone a simple login page (e.g., http://testphp.vulnweb.com/login.php).

Enter your local IP as the listener.

SET clones the site and starts a web server on port 80.

3️⃣ Craft a Fake Email
Write a realistic phishing email:

plaintext
Copy
Edit
Subject: Action Required — Security Update

Hi user,

Please verify your account to maintain access. Log in here:
http://<attacker-ip>

Thank you,
IT Support



4️⃣ Simulate Victim Click
Open a browser on your test victim VM.

Click the fake link → fake login page.

Enter test credentials: test / test.

5️⃣ Capture Credentials
SET console shows captured POST request:

less
Copy
Edit
[+] Username: test
[+] Password: test
Screenshot this evidence:

Result
✅ Credentials successfully captured via cloned phishing site.
✅ Demonstrates real-world risk of poor user awareness & missing email protections.

Mitigation & Recommendations
Deploy email filtering with phishing detection.

Enforce Multi-Factor Authentication (MFA).

Train users on phishing awareness with regular simulations.

Use strong domain & DMARC/SPF/DKIM records to prevent spoofing.

References
Social-Engineering Toolkit (SET)

