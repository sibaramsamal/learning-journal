## March 25, 2025
### Activity: Installation of Development Tools on MacBook Air (macOS)

---

### 1. **JDK Installation using Terminal**

To set up Java Development Kit (JDK) on macOS, follow these steps:

1. **Check if Homebrew is installed**:
   - Homebrew is a package manager for macOS that makes installing software easier.
   - If Homebrew is not installed, install it using the following command:
     ```bash
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
     ```

2. **Install JDK**:
   - Use Homebrew to install the latest version of **OpenJDK**:
     ```bash
     brew install openjdk
     ```
     for specific version,
     ```bash
     brew install openjdk@11
     ```

3. **Set up the JDK environment variables**:
   - After installation, add the following to your `.zshrc` file (or `.bash_profile` if you're using Bash):
     ```bash
     export PATH="/usr/local/opt/openjdk/bin:$PATH"
     export JAVA_HOME=$(/usr/libexec/java_home)
     ```
   - Then, refresh your terminal:
     ```bash
     source ~/.zshrc
     ```

4. **Verify Installation**:
   - Check the installed Java version to ensure it's installed correctly:
     ```bash
     java -version
     ```
   - You should see the installed JDK version.

---

### 2. **IntelliJ IDEA Installation**

IntelliJ IDEA is a powerful IDE for Java and other languages. Here's how to install it on macOS:

1. **Install IntelliJ IDEA using Homebrew**:
   - If you have **Homebrew Cask** installed, you can easily install IntelliJ IDEA with the following command:
     ```bash
     brew install --cask intellij-idea-ce
     ```

2. **Manual Installation (if you prefer)**:
   - Download **IntelliJ IDEA** from the [official website](https://www.jetbrains.com/idea/download/).
   - Choose either the **Community** (free) or **Ultimate** (paid) edition.
   - After downloading, open the `.dmg` file and drag **IntelliJ IDEA** to the **Applications** folder.

3. **Launch IntelliJ IDEA**:
   - After installation, you can launch it from the **Applications** folder or by searching via **Spotlight**.

4. **Set up the IDE**:
   - When first launched, IntelliJ IDEA will ask you to import settings. Choose "Do not import settings" if this is your first time.
   - After that, you'll be ready to create or import projects.
  
---

### 3. **MySQL Database Installation**

To set up MySQL on macOS, you can use **Homebrew** for an easy installation:

1. **Install MySQL**:
   - Open Terminal and use **Homebrew** to install MySQL:
     ```bash
     brew install mysql
     ```

2. **Start MySQL**:
   - Once installed, start the MySQL service:
     ```bash
     brew services start mysql
     ```

3. **Secure MySQL Installation**:
   - Run the following command to secure the installation and set a root password:
     ```bash
     mysql_secure_installation
     ```

4. **Verify MySQL Installation**:
   - After securing it, check if MySQL is running by logging into MySQL as the root user:
     ```bash
     mysql -u root -p
     ```
   - Enter the password you set during the secure installation process.

5. **Stop MySQL** (if needed):
   - To stop the MySQL service:
     ```bash
     brew services stop mysql
     ```

---

### Reflections:
- **JDK Installation**: Successfully installed OpenJDK and set up the necessary environment variables. Java development environment is ready.
- **IntelliJ IDEA**: Installed IntelliJ IDEA for Java development. It looks very user-friendly and ready for use.
- **MySQL Installation**: MySQL is successfully installed and secured. I will explore creating databases and tables tomorrow.

---