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
