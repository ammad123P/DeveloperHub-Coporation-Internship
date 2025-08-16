## 🔓 Brute Force Attack on DVWA using Burp Suite

In DVWA, the **Brute Force vulnerability module** is designed to help security learners practice password cracking attacks.  
I used **Burp Suite Intruder** to perform this attack in a controlled environment.

---

### 🛠️ Step-by-Step Process

#### 1. Open DVWA and Set Security Level
- Logged into **DVWA**.
- Set the **security level to "Low"** from the DVWA security settings.
- Navigated to the **Brute Force** module.

#### 2. Intercept the Login Request
- Entered dummy credentials (`admin:test`) in the DVWA brute force login form.
- Opened Burp Suite and enabled **Proxy → Intercept**.
- Submitted the form and captured the HTTP POST request containing:
  ```http
  POST /vulnerabilities/brute/ HTTP/1.1
  Host: localhost
  username=admin&password=test&Login=Login

#### 3. Send Request to Intruder

* Right-clicked the captured request → **Send to Intruder**.
* In Intruder, selected the **Positions tab**.
* Highlighted the values of `username` and `password` and marked them with **§** symbols.

#### 4. Configure Payloads

* Went to the **Payloads tab**.
* Selected **Simple List** as payload type.
* For username → provided `admin`.
* For password → loaded a **wordlist** of common passwords (e.g., `1234`, `password`, `admin`, `12345`, etc.).

#### 5. Start the Attack

* Clicked **Start Attack** in Intruder.
* Burp Suite sent multiple requests with each username-password combination.

#### 6. Analyze the Results

* In the results window, compared the **Status** and **Length** of each response.
* All incorrect attempts had similar response length (e.g., 4920).
* The correct login response (`admin:password`) had a **different length** and redirected to the DVWA success page.

---

### 📊 Observation

* Brute Force on DVWA (Low Security) was successful because **no rate limiting or lockout mechanism** was implemented.
* With a small wordlist, Burp Suite quickly identified the valid credentials:

  ```
  Username: admin
  Password: password
  ```

---

### ⚠️ Ethical Reminder

This attack was performed **only on DVWA in a virtual lab environment**.
Never attempt brute force attacks on real websites, as it is **illegal and unethical**.

👉 Bro, do you also want me to make **Medium and High Security level explanation** (how brute force behaves differently there), so your README looks more advanced?

### Video Explanation
https://drive.google.com/file/d/1af2F6BPJ_3dsz9MYJtcQVZv6xiAlDNpK/view?usp=sharing


