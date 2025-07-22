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
