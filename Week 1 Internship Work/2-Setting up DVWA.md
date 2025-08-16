## ⚙️ Setting Up DVWA

Follow the steps below to set up **Damn Vulnerable Web Application (DVWA)** on your system.  

---

### 📥 Step 1: Download DVWA

1. **Go to the web server folder:**  
   On Linux, this is usually the `/var/www/html/` folder.  

   ```bash
   cd /var/www/html/

2. **Download DVWA using Git:**
   This will clone the DVWA repository to your server.

   ```bash
   sudo git clone https://github.com/digininja/DVWA.git /var/www/html/dvwa

3. **Check folder contents:**

   ```bash
   ls /var/www/html/dvwa/
---
### 🔑 Step 2: Set Permissions

Change permissions so DVWA files can be read, written, and executed properly:

```bash
sudo chmod -R 755 /var/www/html/dvwa
```

> ✅ `755` is more secure than `777` because it gives the owner full control, while others only get read & execute permissions.

---

### 🛠️ Step 3: Configure DVWA

1. **Go to the DVWA config folder:**

   ```bash
   cd /var/www/html/dvwa/config/
   ```

2. **Check folder contents:**

   ```bash
   ls
   ```

3. **Rename the sample config file:**

   ```bash
   sudo mv config.inc.php.dist config.inc.php
   ```

4. **Edit the configuration file:**

   ```bash
   sudo nano /var/www/html/dvwa/config/config.inc.php
   ```

   Ensure the following database settings are correct:

   ```php
   <?php
   $DBMS = 'MySQL'; // Only MySQL supported

   $_DVWA = array();
   $_DVWA['db_server']   = '127.0.0.1';      // MySQL server address
   $_DVWA['db_database'] = 'dvwa';           // Database name
   $_DVWA['db_user']     = 'dvwa';           // Username
   $_DVWA['db_password'] = 'p@ssw0rd';       // Password
   $_DVWA['db_port']     = '3306';           // Default MySQL port

   // reCAPTCHA (optional)
   $_DVWA['recaptcha_public_key']  = '';
   $_DVWA['recaptcha_private_key'] = '';

   // Default Security Level
   $_DVWA['default_security_level'] = 'low';
   ?>
   ```

---

### 🗄️ Step 4: Set Up MySQL Database

1. **Start MySQL service:**

   ```bash
   sudo service mysql start
   ```

2. **Login to MySQL as root:**

   ```bash
   sudo mysql -u root -p
   ```

3. **Create the DVWA database:**

   ```sql
   CREATE DATABASE dvwa;
   ```

   Check databases:

   ```sql
   SHOW DATABASES;
   ```

4. **View existing users:**

   ```sql
   SELECT User, Host FROM mysql.user;
   ```

5. **Create DVWA user & grant permissions:**

   ```sql
   CREATE USER 'dvwa'@'127.0.0.1' IDENTIFIED BY 'p@ssw0rd';
   GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'127.0.0.1';
   ```

   Exit MySQL:

   ```sql
   exit;
   ```

---

### 🌐 Step 5: Configure Apache & PHP

1. **Navigate to PHP config directory:**

   ```bash
   cd /etc/php/8.2/apache2/
   ```

2. **Open the php.ini file:**

   ```bash
   sudo nano /etc/php/8.2/apache2/php.ini
   ```

3. **Adjust important settings:**

   ```ini
   allow_url_include = On
   memory_limit = 128M
   max_execution_time = 30
   ```

4. **Save & exit nano:**

   * `CTRL + O` → Save
   * `Enter` → Confirm
   * `CTRL + X` → Exit

5. **Restart Apache:**

   ```bash
   sudo service apache2 start
   ```

✅ **Optional:** Check PHP version loaded with Apache

```bash
sudo nano /var/www/html/phpinfo.php
```

Insert:

```php
<?php
phpinfo();
?>
```

Now visit:
👉 [http://127.0.0.1/phpinfo.php](http://127.0.0.1/phpinfo.php)

---

### 🧪 Step 6: Test with CSRF Attack

1. Open DVWA setup in browser:

   👉 [http://127.0.0.1/dvwa/setup.php](http://127.0.0.1/dvwa/setup.php)

2. Click on **"Create/Reset Database"**.

3. Login with default credentials:

   ```
   Username: admin
   Password: password
   ```

4. Navigate to **CSRF module** in the DVWA menu and test the vulnerability.

---

✅ You now have DVWA fully installed and ready for testing vulnerabilities.
