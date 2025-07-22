# Lab Report: Broken JWT Authentication - API Privilege Escalation

---

## Objective
Demonstrate exploiting an insecure JWT implementation to escalate privileges in a web API.

---

## Lab Setup
- **Target:** Custom vulnerable API (or OWASP Juice Shop)
- **Attacker:** Kali Linux
- **Tools:** Burp Suite, JWT.io, curl, Postman

---

## Recon
- Log in as low-privilege user.
- Capture JWT in `Authorization: Bearer` header.
- Decode with [jwt.io](https://jwt.io).

---

## Exploitation

1️⃣ Check for `alg: none` or `HS256` with guessable secret.  
2️⃣ Forge new JWT with admin payload:
```json
{
  "user": "admin",
  "role": "admin"
}
````

* Sign with known or brute-forced secret.
* Replace original token in request.

3️⃣ Send forged token:

```bash
curl -H "Authorization: Bearer <forged-jwt>" http://<target>/api/admin
```

* Confirm admin access to restricted endpoint.

---

## Post-Exploitation

* Dump user list.
* Access protected resources.
* Prove privilege escalation.

---

## Mitigation

* Use secure algorithms (RS256 with proper key rotation)
* Validate signatures properly
* Implement short JWT lifetimes
* Use strong secrets

---

## Screenshots

![JWT decode](screenshots/jwt_decode.png)
![Forged token](screenshots/jwt_forged.png)
![Admin access](screenshots/jwt_admin_access.png)

---

## References

* [OWASP JWT Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_for_Java_Cheat_Sheet.html)
* [JWT.io](https://jwt.io/)
