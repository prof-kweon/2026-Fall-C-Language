# Git Usage Guide 

### 1. Installing Git
- Download and install Git from [git-scm.com](https://git-scm.com/).
- Open the VS Code terminal and check: 
  ```sh
  git --version
  ```

### 2. Configuring Git
- Set your user name and email:
  ```sh
  git config --global user.name "Your Name"
  git config --global user.email "your.email@example.com"
  ```
- Check your configuration:
  ```sh
  git config --list
  ```

### 3. Initializing a Repository
- Create a new repository:
  ```sh
  git init
  ```
- Clone an existing repository:
  ```sh
  git clone <repository_url>
  ```

### 4. Basic Commands
- Check the repository status:
  ```sh
  git status
  ```
- Add files to staging:
  ```sh
  git add <file>
  ```
- Commit changes:
  ```sh
  git commit -m "Your commit message"
  ```
- View commit history:
  ```sh
  git log
  ```
- Push changes to a remote repository:
  ```sh
  git push origin main
  ```
- Pull changes from a remote repository:
  ```sh
  git pull origin main
  ```

### 5. Branching and Merging
- Create a new branch:
  ```sh
  git branch <branch_name>
  ```
- Switch to a branch:
  ```sh
  git checkout <branch_name>
  ```
- Merge branches:
  ```sh
  git merge <branch_name>
  ```
- Delete a branch:
  ```sh
  git branch -d <branch_name>
  ```

### 6. Undoing Changes
- Undo the last commit:
  ```sh
  git reset --soft HEAD~1
  ```
- Discard uncommitted changes:
  ```sh
  git checkout -- <file>
  ```
- Revert a commit:
  ```sh
  git revert <commit_hash>
  ```

---
