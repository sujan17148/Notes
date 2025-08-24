# 📘 Version Control System (VCS) & Git

## 🔹 What is Version Control System (VCS)?

A Version Control System (VCS) is a software tool that helps manage changes to source code and files over time.  
It records every modification made, allowing developers to track history, collaborate efficiently, and revert to earlier versions when necessary.

👉 Think of it as a time machine for your code.

### Example: Writing a book
- **Without VCS**: You save multiple files → `MyBook.docx`, `MyBook_v2.docx`, `MyBook_final.docx`, `MyBook_final_FINAL.docx` 📂
- **With VCS**: You keep one file, and the system automatically remembers every change (what changed, when, and who made it).

## 🔹 Why Do We Need VCS?

- **Track Changes** – See exactly what changed between versions.
- **Backup** – Your entire history is saved, reducing risk of data loss.
- **Collaboration** – Multiple developers can work together without overwriting each other’s code.
- **Branching** – Experiment with new features without breaking the main project.
- **Rollback** – Instantly return to any previous version if something breaks.

### Popular VCS:
- **Git** (most popular)
- **Apache Subversion**
- **Piper** (used by Google)

## 🔹 What is Git?

Git is the most widely used Version Control System (VCS) in modern software development.  
It is a free and open-source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

## 🔹 Key Features of Git:
- **Distributed** → Every developer has a full copy of the project’s history on their computer.
- **Fast** → Operations like commits, branching, and merging are optimized for speed.
- **Open Source** → Completely free and community-driven.
- **Local-first** → Works offline on your computer, internet only needed for syncing with remotes.

### Git Tracks:
- What changed in each file.
- When it changed.
- Who made the change.
- Why it changed (via commit messages).

### ✅ In short:
- **VCS** = the concept of tracking and managing changes.
- **Git** = the most popular tool to implement VCS.

## 📘 GitHub

### 🔹 What is GitHub?

GitHub is a cloud-based platform that hosts Git repositories, making it easy to collaborate, share, and back up projects online.  
👉 Think of it as Google Drive, but designed for Git projects.

### 🔹 What GitHub Provides:
- **Cloud Storage** – Your Git repositories live safely online.
- **Collaboration Platform** – Teams can contribute code, review changes, and track issues together.
- **Social Network for Developers** – Discover, share, and contribute to open-source projects.
- **Backup Service** – Your project history is securely stored in the cloud.

### 🔹 The Relationship: Git vs GitHub
- **Git (Local) ↔️ GitHub (Cloud)**  
  Your Computer ↔️ Online Storage

👉 **Git** = manages versions on your computer.  
👉 **GitHub** = stores and shares those versions online.

### 🔹 Key Analogy 🏠
- **VCS** = The concept of keeping blueprints and progress photos.  
- **Git** = Your personal filing system to organize every version of blueprints.  
- **GitHub** = The online vault where you store and share copies, so others can help you build and review.

---

# git configuration 

### Set your username (appears in commits)
```bash
git config --global user.name "Your Name"
```

### Set your email (must match GitHub/GitLab email for commits to link)
```bash
git config --global user.email "your_email@example.com"
```

### Check current config
```bash
git config --list
```

### Set default branch name (instead of 'master')
```bash
git config --global init.defaultBranch main
```

👉 `--global` = applies to all repos on your computer  
👉 Remove `--global` to set it per project only

---

# 📘 Basic Git Commands

### Initialize a new Git repository in current folder
```bash
git init
```

### Check status of changes (tracked/untracked/staged)
```bash
git status
```

### Add a specific file to staging area
```bash
git add filename
```

### Add all changes to staging area
```bash
git add .
```

### Commit staged changes with a message
```bash
git commit -m "your message"
```

### View commit history (detailed)
```bash
git log
```

### View commit history (short form)
```bash
git log --oneline
```

### Remove a file from repo + staging + working directory
```bash
git rm filepath
```

### Show details of a commit(default: last commit)
 👉changes occurred during that particular commit 
```bash
git show commit_id
```

### See changes not staged yet
```bash
git diff
```

### Show who changed each line of a file and when
```bash
git blame filepath
```

### Revert a commit by creating a new commit that undoes it
```bash
git revert commit_id
```

### Reset back to a specific commit (⚠️ destructive)
*commit flow: commit1 → commit2 → commit3 → … where the last commit is HEAD*  

We use a target `commit_id` to make it the new HEAD, meaning all the commits after that particular commit are lost.

```bash
git reset --hard commit_id
```