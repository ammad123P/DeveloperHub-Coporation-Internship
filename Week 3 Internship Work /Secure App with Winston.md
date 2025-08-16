# Secure Node.js App with Winston Logging

This is a **secure Node.js application** built with Express.js, implementing security best practices and logging using **Winston**. The app demonstrates safe input handling, rate limiting, and centralized error logging for production-ready web applications.

---

## Features

- **Winston Logging**:  
  - Logs errors separately (`logs/error.log`)  
  - Logs all activities (`logs/combined.log`)  
  - Colorized console output during development

- **Security Best Practices**:  
  - Helmet for HTTP headers security  
  - Rate limiting to prevent brute-force attacks  
  - Input validation and sanitization using `express-validator`  
  - Centralized error handling to avoid leaking sensitive information  

- **Basic Routes**:  
  - GET `/` – Home route  
  - POST `/login` – Example login route with validation  

---

## Prerequisites

- Node.js >= 18  
- npm >= 9  

---

## Installation

1. Clone the repository:

```bash
git clone <repository-url>
cd secure-app
````

2. Install dependencies:

```bash
npm install
```

3. Create a `.env` file:

```env
NODE_ENV=development
PORT=3000
```

---

## Usage

Start the server:

```bash
node index.js
```

Server will run on `http://localhost:3000`.

* Access home route: `GET /`
* Example login: `POST /login` with JSON body:

```json
{
  "username": "user123",
  "password": "mypassword"
}
```

---

## Project Structure

```
secure-app/
├─ index.js         # Main Express app
├─ logger.js        # Winston logger configuration
├─ .env             # Environment variables
├─ logs/            # Directory for log files
├─ package.json
```

---

## SETTING UP 

# 1. `package.json`

```json
{
  "name": "secure-app",
  "version": "1.0.0",
  "description": "A secure Node.js app with Winston logging and best practices",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "helmet": "^7.0.0",
    "express-rate-limit": "^7.0.0",
    "winston": "^3.9.0",
    "express-validator": "^7.0.1",
    "dotenv": "^16.3.1"
  }
}
```

---

# 2. `.env`

```env
NODE_ENV=development
PORT=3000
```

---

# 3. `logger.js`

```js
const winston = require('winston');
const path = require('path');

const logDir = 'logs';

// Create logger
const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }),
    winston.format.printf(
      (info) => `${info.timestamp} [${info.level.toUpperCase()}]: ${info.message}`
    )
  ),
  transports: [
    new winston.transports.File({ filename: path.join(logDir, 'error.log'), level: 'error' }),
    new winston.transports.File({ filename: path.join(logDir, 'combined.log') }),
  ],
});

// Console logging for development
if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.combine(
      winston.format.colorize(),
      winston.format.simple()
    )
  }));
}

module.exports = logger;
```

---

# 4. `index.js`

```js
require('dotenv').config();
const express = require('express');
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
const { body, validationResult } = require('express-validator');
const logger = require('./logger');
const fs = require('fs');
const path = require('path');

const app = express();
const PORT = process.env.PORT || 3000;

// Create logs directory if not exists
if (!fs.existsSync('logs')) fs.mkdirSync('logs');

// Middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(helmet());

// Rate Limiting
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 min
  max: 100, // max 100 requests per window per IP
  message: "Too many requests from this IP, try again later",
});
app.use(limiter);

// Routes
app.get('/', (req, res) => {
  logger.info('Home route accessed');
  res.send('Welcome to Secure Node.js App!');
});

app.post('/login', 
  // Validation and sanitization
  body('username').trim().escape().notEmpty().withMessage('Username is required'),
  body('password').trim().escape().notEmpty().withMessage('Password is required'),
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      logger.warn(`Validation failed: ${JSON.stringify(errors.array())}`);
      return res.status(400).json({ errors: errors.array() });
    }

    const { username, password } = req.body;

    // Example authentication logic (DO NOT store plain passwords)
    if (username === 'admin' && password === 'password123') {
      logger.info(`User '${username}' logged in successfully`);
      return res.json({ message: 'Login successful' });
    } else {
      logger.warn(`Failed login attempt for username: '${username}'`);
      return res.status(401).json({ message: 'Invalid credentials' });
    }
  }
);

// Error Handling Middleware
app.use((err, req, res, next) => {
  logger.error(`Unhandled error: ${err.message}`);
  res.status(500).json({ message: 'Internal server error' });
});

// Start Server
app.listen(PORT, () => {
  logger.info(`Server running on http://localhost:${PORT}`);
});
```

---

# ✅ How to Run

1. Install dependencies:

```bash
npm install
```

2. Start the server:

```bash
node index.js
```

3. Access the home route: `http://localhost:3000`
4. Test login via POST `/login` with JSON body:

```json
{
  "username": "admin",
  "password": "password123"
}
```

---

This setup ensures:

* Input validation & sanitization
* Rate limiting
* Centralized logging (errors + activity)
* Secure HTTP headers via Helmet



---

## Security Notes

* Always validate and sanitize inputs before processing.
* Use HTTPS in production.
* Avoid logging sensitive information like passwords or tokens.
* Adjust rate-limiting rules according to your application's traffic.

---

## Logging

* **Error logs**: `logs/error.log`
* **Combined logs**: `logs/combined.log`
* **Console logs** (development only)
