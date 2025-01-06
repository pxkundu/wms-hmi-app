# wms-hmi-app
Warehouse Management System (WMS/WCS) application with HMI and Conveyer line control functionality using Ignition project

### **Setting Up a Warehouse Management System (WMS) with HMI and Conveyor Line Control on Your MacBook Pro**

Let’s break down the steps for creating your WMS project with HMI and conveyor control functionality using **Ignition** on your **MacBook Pro**. This guide starts from installation to setting up the foundation for your project.

---

### **Step 1: Install and Configure Ignition on macOS**
1. **Download Ignition**:
   - Visit the [Inductive Automation Download Page](https://inductiveautomation.com/downloads) and download the **Ignition Installer** for macOS.

2. **Install Ignition**:
   - Run the `.dmg` file and follow the installation steps.
   - Open the **Ignition Gateway Control Utility (GCU)** to start the Gateway.

3. **Access the Gateway**:
   - Open your browser and go to `http://localhost:8088`.
   - Complete the setup by creating an admin username and password.

---

### **Step 2: Set Up a SQL Database**
To manage inventory, conveyor status, and logs, you'll need a SQL database.

1. **Install MySQL** (or PostgreSQL if you prefer):
   - Download MySQL for macOS from [MySQL Community Downloads](https://dev.mysql.com/downloads/).
   - Install MySQL and start the server via `System Preferences`.
   - Follow this for configuration [/installmysql.md]

2. **Set Up the Database**:
   - Use a GUI tool like **MySQL Workbench** or connect via the terminal:
     ```bash
     mysql -u root -p
     ```
   - Create a database for your project:
     ```sql
     CREATE DATABASE wms_project;
     ```

3. **Connect Ignition to the Database**:
   - In the Ignition Gateway, go to **Config > Databases > Connections**.
   - Add a new connection, choose **MySQL**, and enter your database credentials.

---

### **Step 3: Configure Tags for the WMS**
Tags represent the variables in your project, such as inventory levels or conveyor statuses.

1. **Create Tags for Warehouse Data**:
   - Open the Ignition Designer and create a new project.
   - Go to the **Tag Browser** and create folders like `Inventory`, `Conveyors`, and `Alarms`.
   - Add memory tags:
     - `Inventory/ItemCount` (integer): Represents the number of items in stock.
     - `Conveyors/Conveyor1Status` (boolean): Indicates if the conveyor is running.
     - `Alarms/Overload` (boolean): Triggered when conveyor load exceeds capacity.

2. **Simulate Tags for Testing**:
   - Right-click a tag, go to **Edit**, and set a simulation mode (e.g., ramp or sine wave).

---

### **Step 4: Create an HMI Interface**
The HMI (Human-Machine Interface) will display the status of the warehouse and conveyor systems.

1. **Design the Interface**:
   - Use the **Perspective Module** (responsive and mobile-friendly) or the **Vision Module** (traditional HMI).
   - Add elements like:
     - Numeric Displays for inventory levels.
     - Toggle Switches for conveyor control.
     - Charts for visualizing trends (e.g., conveyor load over time).

2. **Bind Tags to Components**:
   - Drag and drop tags onto components (e.g., a label for `Inventory/ItemCount`).

---

### **Step 5: Add Conveyor Line Control Functionality**
1. **Write Tag Change Scripts**:
   - Automate conveyor operations based on inventory levels.
   ```python
   # Tag Change Script for Inventory/ItemCount
   conveyor_tag = "[default]Conveyors/Conveyor1Status"
   if currentValue.value > 100:
       system.tag.writeBlocking([conveyor_tag], [False])  # Stop conveyor if overload
   else:
       system.tag.writeBlocking([conveyor_tag], [True])   # Keep conveyor running
   ```

2. **Simulate Conveyor Behavior**:
   - Create a gateway timer script to simulate conveyor movement:
   ```python
   import random
   current_items = system.tag.readBlocking(["[default]Inventory/ItemCount"])[0].value
   system.tag.writeBlocking(["[default]Inventory/ItemCount"], [max(0, current_items - random.randint(0, 5))])
   ```

---

### **Step 6: Store and Retrieve Data from the Database**
1. **Store Inventory Data**:
   - Use database queries to log inventory levels.
   ```python
   query = "INSERT INTO inventory_log (timestamp, item_count) VALUES (?, ?)"
   item_count = system.tag.readBlocking(["[default]Inventory/ItemCount"])[0].value
   system.db.runPrepUpdate(query, [system.date.now(), item_count], "wms_project")
   ```

2. **Retrieve Historical Data**:
   - Create a query to fetch data for charts or reports.
   ```python
   query = "SELECT * FROM inventory_log WHERE timestamp > ?"
   results = system.db.runPrepQuery(query, [system.date.now().addDays(-7)], "wms_project")
   ```

---

### **Step 7: Add Alarm Management**
1. **Create Alarms**:
   - Define alarms for critical conditions like conveyor overloads.
   ```python
   # Tag Change Script for Alarms/Overload
   if currentValue.value == True:
       system.alarm.createSimpleAlarm("Overload Alarm", "Conveyor load is too high!")
   ```

2. **Acknowledge Alarms**:
   - Add functionality to acknowledge alarms from the HMI.
   ```python
   system.alarm.acknowledge(["Overload Alarm"])
   ```

---

### **Step 8: Test and Debug Your Project**
1. **Use the Script Console**:
   - Test your scripts interactively to ensure correctness.
2. **Monitor Logs**:
   - Check the **Gateway Status Logs** for errors or warnings.
3. **Simulate Edge Cases**:
   - Test scenarios like inventory overflow or conveyor failures.

---

### **Step 9: Deploy and Demonstrate**
1. **Deploy Locally**:
   - Run the project on your local Ignition Gateway.
2. **Access from Multiple Devices**:
   - Use Perspective to access the HMI from a browser or mobile device.
3. **Document the Project**:
   - Include clear instructions and screenshots for end-users.

---

### **Example Database Schema**
Here’s a schema to use for logging data:

#### **inventory_log**
| Column Name   | Data Type | Description                        |
|---------------|-----------|------------------------------------|
| `id`          | INT       | Primary key                       |
| `timestamp`   | DATETIME  | Time of inventory log             |
| `item_count`  | INT       | Number of items in the inventory  |

---

This setup will give you a fully functional demo of a Warehouse Management System with HMI and conveyor line control. Let me know if you'd like detailed examples of specific functionality! 😊