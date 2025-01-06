### **Running MySQL in the Terminal**

Here's a step-by-step guide to running MySQL in the terminal on your **MacBook Pro**:

---

### **1. Check if MySQL is Installed**
- Open your terminal and type:
  ```bash
  mysql --version
  ```
- If MySQL is installed, this command will return the version number. If not, download it from [MySQL Community Downloads](https://dev.mysql.com/downloads/).

---

### **2. Start the MySQL Server**
- If you’ve already installed MySQL, ensure the MySQL server is running:
  1. Open **System Preferences** and go to **MySQL**.
  2. Click **Start MySQL Server**.
- Alternatively, use Homebrew (if installed):
  ```bash
  brew services start mysql
  ```

---

### **3. Log In to MySQL**
1. Open your terminal.
2. Enter the following command:
   ```bash
   mysql -u root -p
   ```
   - Replace `root` with your username (if different).
   - You’ll be prompted to enter the MySQL root password (set during installation).

---

### **4. Basic MySQL Commands**
Once logged in, you can start interacting with your database.

#### **Check Existing Databases**:
```sql
SHOW DATABASES;
```

#### **Create a New Database**:
```sql
CREATE DATABASE my_database;
```

#### **Use a Database**:
```sql
USE my_database;
```

#### **Create a Table**:
```sql
CREATE TABLE inventory (
    id INT AUTO_INCREMENT PRIMARY KEY,
    item_name VARCHAR(50),
    quantity INT,
    last_updated DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

#### **Insert Data**:
```sql
INSERT INTO inventory (item_name, quantity) VALUES ('Widgets', 100);
```

#### **View Data**:
```sql
SELECT * FROM inventory;
```

#### **Exit MySQL**:
To leave the MySQL prompt, type:
```bash
exit;
```

---

### **5. Troubleshooting Tips**
- **Error: `Command not found`**: Add MySQL to your PATH by updating your `.zshrc` or `.bash_profile` file:
  ```bash
  export PATH="/usr/local/mysql/bin:$PATH"
  ```
  Then reload the file:
  ```bash
  source ~/.zshrc
  ```
- **Forgot Root Password**: Reset it using the MySQL server's configuration.

---

### **6. Manage MySQL Server with Homebrew**
If you installed MySQL with Homebrew, you can manage the server easily:
- **Start MySQL**:
  ```bash
  brew services start mysql
  ```
- **Stop MySQL**:
  ```bash
  brew services stop mysql
  ```
- **Restart MySQL**:
  ```bash
  brew services restart mysql
  ```

---

By following these steps, you can run MySQL directly in your terminal and interact with your databases. Let me know if you encounter any issues! 😊