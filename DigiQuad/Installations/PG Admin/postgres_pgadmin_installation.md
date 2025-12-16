This guide outlines the steps to install PostgreSQL and pgAdmin on macOS using Homebrew, as well as troubleshooting common issues with logging into pgAdmin. If Homebrew is not installed on your macOS, run the following command in the terminal:

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

Once Homebrew is installed, run the following command to install PostgreSQL:

`brew install postgresql`

After installation, start the PostgreSQL service with:

`brew services start postgresql`

To verify that PostgreSQL is installed and running correctly, check the version:

`psql --version`

You should see the version of PostgreSQL printed in the terminal. To create a new database, run:

`createdb mydb`

Replace `mydb` with your desired database name. After installing PostgreSQL via Homebrew, PostgreSQL is typically set up with your macOS username (the current user) as the default PostgreSQL role. This can lead to login issues when accessing pgAdmin if you're not aware of this setup. To log into `psql`, run:

`psql postgres`

To see which roles (users) are available in your PostgreSQL installation, use the following command inside the `psql` shell:

`\du`

If the `postgres` user does not exist, you can create it manually by running the following command:

`CREATE ROLE postgres WITH LOGIN SUPERUSER PASSWORD 'admin';`

Replace `'admin'` with your desired password. After creating the `postgres` user, verify that it was successfully created by running:

`\du`

Once the necessary changes are made, exit the `psql` shell by typing:

`\q`

Now that the `postgres` user has been created, log in using the following command:

`psql -U postgres -d postgres`

This command will log you in as the `postgres` superuser, allowing you to administer the database. If you want to change the password for the `postgres` user, you can do so with the following command in `psql`:

`ALTER USER postgres WITH PASSWORD 'newpassword';`

Replace `'newpassword'` with your desired password. To install pgAdmin, run the following command:

`brew install --cask pgadmin4`

Once installed, open **pgAdmin** from the **Applications** folder or via Spotlight. To add a server in **pgAdmin**, right-click on **Servers** in the left panel and select **Create** > **Server**. You’ll need to provide the following details:

- **Name**: A name for your server (e.g., `Local PostgreSQL`)
- **Host**: `localhost`
- **Port**: `5432` (default PostgreSQL port)
- **Username**: `postgres`
- **Password**: The password you set for the `postgres` user

Click **Save** and you should be able to connect to your PostgreSQL server via **pgAdmin**. This guide provides a clear set of steps for installing PostgreSQL, creating users, troubleshooting issues, and accessing your databases through pgAdmin.


# Database Backup Guide (With Create & Insert Scripts)

This guide explains how to take a database backup that includes both **CREATE** and **INSERT** statements.

## Steps

### **Step 1:**  
Select the database, and from the options, choose **Backup**.

### **Step 2:**  
In the backup type, choose **Plain**.

### **Step 3:**  
Fill in the backup file name and set the file type to **.sql**.

### **Step 4:**  
In the **Data** options (2nd column), check the **Data** radio button, **pre-data** and **post-data** all must checked.

### **Step 5:**  
In the 3rd column, under **Query Options**, enable:  
- **Include CREATE DATABASE statement**  
- **Use INSERT commands**

### **Step 6:**  
In the last column, choose all the tables, including those from the **public** schema.

---
## Screenshots

<p align="center">
  <img src="https://github.com/user-attachments/assets/98b94896-e53d-4a44-8831-b24348515957" width="450" alt="Screenshot 1" />
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/4d11a20e-62ea-41a6-bdbc-e36b16c320b7" width="450" alt="Screenshot 2" />
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/77b8256d-23a2-40ae-ad23-e0d398d7eb72" width="450" alt="Screenshot 3" />
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/ceb09f37-a5bf-4cfd-ad3b-7f460ae39e1b" width="450" alt="Screenshot 4" />
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/ac049649-4907-4e19-adcd-b6aa8f141a35" width="450" alt="Screenshot 5" />
</p>

---