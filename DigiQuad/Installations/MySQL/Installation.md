1. To install the Mysql package through home brew - 
```brew install mysql```
2. To start the DB - ```brew services start mysql```
3. To set the DB Password and other customizations like: Set root password, Remove anonymous users, Disable remote root login, Remove test database: ```mysql_secure_installation```

4. After all these, you are all set to use the DB. To login: ```mysql -u root -p```
It will ask for the password. After filling the correct password, we will be entering to the DB terminal and perform any actions.

🔐 Reset Root Password (If Forgot)
```
brew services stop mysql
mysql.server start --skip-grant-tables
mysql -u root

ALTER USER 'root'@'localhost' IDENTIFIED BY '<NewPassword>';
FLUSH PRIVILEGES;

brew services restart mysql
```

📦 Install MySQL Workbench (GUI Tool)
```
brew install --cask mysqlworkbench
```

Some Common Errors while trying to connect with the DB through MySQL
```
Command to start the server is not configured. Please set the command that must be used to start the server in the remote management section of this connections settings.
```
This is because of My SQL is installed properly, but mySQL workbench is unaware of the running processes.
For solving that,
- Check the alrady running services. 
```
ps aux | grep mysql

It will list down all running instances of mysql
1. 25171   0.0  0.0 410733616   1568 s004  S+   12:44AM   0:00.00 grep mysql
2. 24931   0.0  1.0 412501728 164832   ??  S    12:39AM   0:01.21 /opt/homebrew/opt/mysql/bin/mysqld --basedir=/opt/homebrew/opt/mysql --datadir=/opt/homebrew/var/mysql --plugin-dir=/opt/homebrew/opt/mysql/lib/plugin --log-error=Digiquads-MacBook-Air-3.local.err --pid-file=Digiquads-MacBook-Air-3.local.pid
3. 24850   0.0  0.0 410604896   1264   ??  S    12:39AM   0:00.02 /bin/sh /opt/homebrew/opt/mysql/bin/mysqld_safe --datadir=/opt/homebrew/var/mysql

Don't touch 25171, and kill other twos

sibaram@Digiquads-MacBook-Air-3 ~ % kill -9 24931
kill -9 24850
```

After that restart:  ```brew services start mysql```

Now we can connect 

