# Command Injection Lab

## Overview
This lab demonstrates a **Command Injection** vulnerability in a web application at **low security level**. Command Injection occurs when an application allows users to pass **operating system commands** via input fields without proper validation or sanitization.  

Attackers can exploit this to execute arbitrary commands on the server, potentially accessing sensitive information or taking control of the system.

---

## Objective
- Understand and exploit **command injection** vulnerabilities.  
- Execute arbitrary OS commands through vulnerable input fields.  
- Learn about **operators and techniques** to chain multiple commands.

---

## Steps to Perform the Attack

### 1. Setup
1. Open the lab environment (DVWA or local web application).  
2. Set **Security Level → Low**.  
3. Navigate to the **Command Injection module**.  

---

### 2. Understanding the Input
- The vulnerable input typically asks for information like **IP address, username, or PIN**.  
- Example input form:

```html
<form method="GET" action="vulnerable.php">
  Enter IP or Command: <input type="text" name="target">
  <input type="submit" value="Submit">
</form>
````

* The server executes the input directly using OS commands.

---

### 3. Basic Command Injection

1. Enter a normal input (e.g., `127.0.0.1`) to verify functionality.
2. Inject an OS command using operators like:

* **`;`** (semicolon) → separate multiple commands
* **`&&`** (AND) → execute second command if first succeeds

Example:

```text
127.0.0.1; ls
```

* This will list files on the server.

---

### 4. Advanced Techniques

* Chain multiple commands to extract sensitive data:

```text
127.0.0.1; cat /etc/passwd
```

* Use operators to **schedule commands** or perform **reverse connections**.
* Hidden or non-standard characters can also be used to bypass basic filters.

---

### 5. Observing Results

* Successful command injection allows viewing or modifying files, retrieving system info, or creating reverse shells.
* Output appears directly in the web interface if the server returns command results.

---

## Notes

* Low security levels **do not sanitize user input**, making attacks trivial.
* Always verify lab safety: perform these experiments only in controlled environments.
* Higher security levels include **input validation, command filtering, or escaping**, which prevent straightforward command injection.

---

## Mitigation

1. **Input validation**: Allow only expected characters or values.
2. **Escaping commands**: Prevent direct OS command execution.
3. **Use safe APIs**: Avoid shell execution functions; use language-native functions.
4. **Principle of least privilege**: Limit web application permissions on the server.

---

## References

* [OWASP Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
* DVWA Documentation

```

---

If you want, I can also **make a ready-to-use HTML page with test input examples** for this command injection lab so you can directly submit it in your lab environment.  

Do you want me to do that?
```
