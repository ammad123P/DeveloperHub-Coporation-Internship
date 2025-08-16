# Secure Web Application 🛡️

This project is a simple **Node.js + Express** web application built for learning **basic security hardening techniques**.
It demonstrates:

* ✅ Input validation & sanitization (`validator`)
* ✅ Password hashing (`bcrypt`)
* ✅ Token-based authentication (`jsonwebtoken`)
* ✅ Securing HTTP headers (`helmet`)

---

## 📂 Project Structure

```
secure-app/
│── package.json              # Dependencies
│── server.js                 # Main entry point
│── routes/
│     └── auth.js             # Auth routes (register/login)
│── middleware/
│     └── authMiddleware.js   # JWT middleware
```

---

## ⚙️ Setup Instructions

### 1️⃣ Clone & Install

```bash
git clone <your-repo-url>
cd secure-app
npm install
```

### 2️⃣ Run the Server

```bash
npm start
```

Server will run on 👉 `http://localhost:3000`

---

## 🔑 Features

### 1. Input Validation

Uses `validator` to validate and sanitize inputs.
Example:

```js
if (!validator.isEmail(email)) {
  return res.status(400).json({ error: "Invalid email format" });
}
```

---

### 2. Password Hashing

Uses `bcrypt` to hash user passwords before storing them.
Example:

```js
const hashedPassword = await bcrypt.hash(password, 10);
```

---

### 3. JWT Authentication

Generates a secure JWT on login, which must be included in the `Authorization` header.
Example:

```js
const token = jwt.sign({ email: user.email }, "your-secret-key", { expiresIn: "1h" });
```

Usage in requests:

```
Authorization: Bearer <token>
```

---

### 4. Secure Headers

Uses `helmet` to add common security headers automatically:

```js
app.use(helmet());
```

---

## 📌 API Endpoints

### Register

```http
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "mypassword"
}
```

### Login

```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "mypassword"
}
```

Response:

```json
{
  "message": "Login successful",
  "token": "your-jwt-token"
}
```

### Protected Example (requires JWT)

```http
GET /profile
Authorization: Bearer <your-token>
```

---

## 🔒 Security Notes

* Use environment variables for secrets (`dotenv`) instead of hardcoding keys.
* Add rate limiting (e.g., `express-rate-limit`) for brute-force protection.
* Always use HTTPS in production.

---

Would you like me to also **add the `/profile` protected route** inside `auth.js` so it matches the README (demo for JWT middleware)?
