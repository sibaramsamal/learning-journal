# Initialize a Git Repository and Push to GitHub

Follow the steps below to create a new Git repository, commit your code, and push it to GitHub:

## Steps

1. Open your terminal and navigate to your project folder.

2. Initialize a new Git repository:
   ```bash
   git init
   ```
   This will display something like:
   ```
   Initialized empty Git repository in your project folder
   ```

3. Add all files to the staging area:
   ```bash
   git add .
   ```

4. Commit the added files:
   ```bash
   git commit -m "Initial commit"
   ```
   This will commit all the added files.

5. Add your GitHub repository as the remote:
   ```bash
   git remote add origin https://github.com/your-username/your-repo-name.git
   ```

6. Push the changes to the `main` branch:
   ```bash
   git push -u origin main

