### Documentation: Installing and Configuring PostgreSQL 15 on Ubuntu for Odoo

This guide provides step-by-step instructions to install PostgreSQL 15 on an Ubuntu virtual machine (VM) and configure it to allow connections from another VM hosting an Odoo installation.

---

### **Step 1: Update the System**
Before installing PostgreSQL, ensure your Ubuntu system is up to date.

```bash
sudo apt update && sudo apt upgrade -y
```

---

### **Step 2: Install PostgreSQL 15**

1. **Add the PostgreSQL APT Repository**  
   PostgreSQL 15 is not available in the default Ubuntu repositories. Add the PostgreSQL official repository:

   ```bash
   sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
   ```

2. **Import the Repository Signing Key**  
   Download and add the PostgreSQL signing key:

   ```bash
   wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
   ```

3. **Update the Package List**  
   Refresh the package list to include the new repository:

   ```bash
   sudo apt update
   ```

4. **Install PostgreSQL 15**  
   Install PostgreSQL 15 and its contrib packages:

   ```bash
   sudo apt install -y postgresql-15 postgresql-contrib
   ```

5. **Verify the Installation**  
   Check the status of the PostgreSQL service to ensure it is running:

   ```bash
   sudo systemctl status postgresql
   ```

---

### **Step 3: Configure PostgreSQL for Remote Access**

1. **Edit `pg_hba.conf`**  
   Allow connections from the Odoo VM by modifying the PostgreSQL client authentication configuration file:

   ```bash
   sudo nano /etc/postgresql/15/main/pg_hba.conf
   ```

   Add the following line to allow connections from the Odoo VM (replace `<odoo_vm_ip>` with the IP address of the Odoo VM):

   ```
   host    all             all             <odoo_vm_ip>/32            md5
   ```

2. **Edit `postgresql.conf`**  
   Configure PostgreSQL to listen on all network interfaces:

   ```bash
   sudo nano /etc/postgresql/15/main/postgresql.conf
   ```

   Locate the `listen_addresses` line and modify it as follows:

   ```
   listen_addresses = '*'
   ```

3. **Restart PostgreSQL**  
   Restart the PostgreSQL service to apply the changes:

   ```bash
   sudo systemctl restart postgresql
   ```

---

### **Step 4: Configure Firewall**

Allow PostgreSQL traffic through the firewall (if `ufw` is enabled):

```bash
sudo ufw allow 5432/tcp
sudo ufw reload
```

---

### **Step 5: Create a PostgreSQL User and Database for Odoo**

1. **Switch to the `postgres` User**  
   Switch to the `postgres` user to manage the database:

   ```bash
   sudo -i -u postgres
   ```

2. **Create a Database User**  
   Create a new user for Odoo (replace `<odoo_user>` and `<password>` with your desired username and password):

   ```bash
   createuser --createdb --username postgres --no-createrole --no-superuser --pwprompt <odoo_user>
   ```

3. **Create a Database**  
   Create a database for Odoo (replace `<odoo_db>` with your desired database name):

   ```bash
   createdb --owner=<odoo_user> <odoo_db>
   ```

4. **Exit the `postgres` User**  
   Exit the `postgres` user session:

   ```bash
   exit
   ```

---

### **Step 6: Verify Remote Connection**

From the Odoo VM, test the connection to the PostgreSQL server:

```bash
psql -h <postgres_vm_ip> -U <odoo_user> -d <odoo_db>
```

Replace `<postgres_vm_ip>`, `<odoo_user>`, and `<odoo_db>` with the appropriate values.

---

### **Step 7: Odoo Configuration Example**

On the Odoo VM, configure the Odoo configuration file (`odoo.conf`) to connect to the PostgreSQL database:

```ini
[options]
; Database settings
db_host = <postgres_vm_ip>
db_port = 5432
db_user = <odoo_user>
db_password = <password>
db_name = <odoo_db>

; Other Odoo settings
admin_passwd = <admin_password>
addons_path = /path/to/odoo/addons
```

Replace `<postgres_vm_ip>`, `<odoo_user>`, `<password>`, `<odoo_db>`, `<admin_password>`, and `/path/to/odoo/addons` with the appropriate values.

---

### **Conclusion**

PostgreSQL 15 is now installed and configured on your Ubuntu VM, ready to accept connections from the Odoo VM. You can proceed with setting up and running Odoo with the provided configuration.
