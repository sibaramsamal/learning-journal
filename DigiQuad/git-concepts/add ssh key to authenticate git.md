# GitHub Command Line Authentication Using Personal Access Tokens

Earlier, GitHub allowed users to authenticate from the command line using their GitHub **username and password**. However, **password-based authentication has been disabled** by GitHub for security reasons.

If you try to authenticate using a username and password—even if they are correct—GitHub will **not allow access**.

To authenticate successfully, you must use a **Personal Access Token (PAT)** instead of your password.

---

## Why Personal Access Tokens?

Personal Access Tokens are more secure and provide better control over permissions compared to passwords. GitHub now requires PATs for all HTTPS-based Git operations.

---

## Steps to Generate a Personal Access Token

1. Log in to **GitHub**
2. Click on your **Profile Icon** (top-right corner)
3. Select **Settings**
4. From the left-side menu, click **Developer settings**
5. Go to **Personal access tokens**
6. Click on **Tokens (classic)**
7. Select **Generate new token**
8. Choose the required **scopes/permissions**
9. Generate and **copy the token** (you won’t be able to see it again)

---

## How to Use the Token

- When Git asks for your **username**, enter your GitHub username
- When Git asks for your **password**, paste the **Personal Access Token** instead

Example:
```bash
git clone https://github.com/username/repository.git
