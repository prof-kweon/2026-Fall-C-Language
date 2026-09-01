# Git Usage Guide 

### 1. Installing Git
- Download and install Git from [git-scm.com](https://git-scm.com/).
- Open the VS Code terminal and check: 
  ```sh
  git --version
  ```

### 2. Sign in to GitHub from VS Code
- Set your user name and email:
- Open VS Code.
- Click the Accounts icon in the bottom-left corner.
- Select Sign in with GitHub and complete the authentication process.

### 3. Clone the GitHub Repository
- On GitHub, open the repository you want to use.
- Click Code and copy the HTTPS repository URL.
- In VS Code, press:
  - Windows/Linux: Ctrl + Shift + P
  - macOS: Cmd + Shift + P
- Search for Git: Clone.
- Paste the repository URL.
- Select a local folder where you want to save the project.
- Click Open after cloning is complete.

### 4. Make Changes and Push Them to GitHub
- Edit or create files in VS Code.
- Open Source Control from the left sidebar.
- Stage your changes by clicking the + button.
- Enter a commit message and click Commit.
- Click Sync Changes or Push to upload your changes to GitHub.


You can also do it from the terminal:
  ```sh
  git clone https://github.com/USERNAME/REPOSITORY.git
  cd REPOSITORY

  # After making changes
  git add .
  git commit -m "Update project"
  git push
  ```

---
