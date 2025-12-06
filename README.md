# 🚀 The Collab-Hub Protocol: Master Git & GitHub by Simulating a Real Startup

**Meta Description:** Stop being a "tutorial developer." This comprehensive, 10-module guide forces you to learn advanced Git workflows, CI/CD with GitHub Actions, and disaster recovery by simulating a real-world software environment.

-----

## 📖 Table of Contents

1.  [Introduction](https://www.google.com/search?q=%23introduction)
2.  [Prerequisites & Setup](https://www.google.com/search?q=%23prerequisites--setup)
3.  [Module 1: The Git Mental Model & Configuration](https://www.google.com/search?q=%23module-1-the-git-mental-model--configuration)
4.  [Module 2: Advanced Branching Strategies](https://www.google.com/search?q=%23module-2-advanced-branching-strategies)
5.  [Module 3: Surviving Merge Conflict Hell](https://www.google.com/search?q=%23module-3-surviving-merge-conflict-hell)
6.  [Module 4: Cleaning History with Interactive Rebase](https://www.google.com/search?q=%23module-4-cleaning-history-with-interactive-rebase)
7.  [Module 5: Monorepos & Submodules](https://www.google.com/search?q=%23module-5-monorepos--submodules)
8.  [Module 6: Project Management with GitHub Issues](https://www.google.com/search?q=%23module-6-project-management-with-github-issues)
9.  [Module 7: CI/CD Pipelines with GitHub Actions](https://www.google.com/search?q=%23module-7-cicd-pipelines-with-github-actions)
10. [Module 8: Debugging with Bisect & Blame](https://www.google.com/search?q=%23module-8-debugging-with-bisect--blame)
11. [Module 9: Disaster Recovery Drills](https://www.google.com/search?q=%23module-9-disaster-recovery-drills)
12. [Module 10: The Final Boss Simulation](https://www.google.com/search?q=%23module-10-the-final-boss-simulation)

-----

## Introduction

Most developers learn `git add` and `git push`, but panic when they encounter a merge conflict or need to revert a production bug. **Collab-Hub** is not a passive tutorial; it is a **simulation**. You will act as a Junior DevOps Engineer at a fictional startup, working through scenarios that require professional-grade version control skills.

**By the end of this course, you will master:**

  * **Release Management:** Git Flow vs. Trunk-Based Development.
  * **Automation:** Writing YAML for GitHub Actions.
  * **History Manipulation:** Safely using `rebase` and `reflog`.
  * **Team Dynamics:** Code reviews, branch protection, and issue tracking.

-----

## Prerequisites & Setup

Before entering the simulation, ensure your environment is ready.

  * **Git Installed:** Run `git --version` (v2.30+ recommended).
  * **Code Editor:** VS Code (recommended for its visual Git lens) or Vim/Neovim.
  * **GitHub Account:** You will need this for the remote simulation.
  * **Terminal:** Bash, Zsh, or PowerShell.

-----

## Module 1: The Git Mental Model & Configuration

**Objective:** Understand the "Three Trees" architecture (Working Directory, Staging Area, Repository) and establish your digital identity.

### 1.1 The "Three Trees" Theory

Git is not just a save button. It is a transport mechanism moving data between three zones:

1.  **Working Directory:** Where you edit files.
2.  **Staging Area (Index):** The "Draft" zone where you prepare a snapshot (`git add`).
3.  **Repository (HEAD):** The permanent history (`git commit`).

### 1.2 Professional Configuration

Do not skip setting up aliases. Senior engineers type fast because they use shortcuts.

```bash
# Identity (Matches your GitHub email)
git config --global user.name "Your Actual Name"
git config --global user.email "your.email@example.com"

# Productivity Aliases
git config --global alias.st status        # Checks state of working directory
git config --global alias.co checkout      # Switches branches
git config --global alias.br branch        # Lists branches
git config --global alias.lg "log --graph --oneline --all --decorate" # Visual history
```

### 1.3 Repository Initialization

We will create a **Service-Oriented Architecture** (SOA) simulation.

1.  **Frontend Repo:** `mkdir collab-hub-frontend && cd collab-hub-frontend && git init`
2.  **Backend Repo:** `mkdir collab-hub-backend && cd collab-hub-backend && git init`
3.  **Infra Repo:** `mkdir collab-hub-infra && cd collab-hub-infra && git init`

> **Assignment:** Create a `docs/git-journal.md` in each repo. Log every command you use. This reinforces muscle memory.

-----

## Module 2: Advanced Branching Strategies

**Objective:** Move beyond `main`. Learn to manage Feature Branches and Integration Branches.

### 2.1 The Workflow

We will simulate **Git Flow**, a standard model for enterprise software.

  * `main`: Production-ready code only.
  * `develop`: Integration branch where features merge.
  * `feature/*`: Temporary branches for new work.

### 2.2 Execution

Perform this in your **Frontend** repo:

1.  **Setup Integration:**
    ```bash
    git checkout -b develop
    git push -u origin develop
    ```
2.  **Start a Feature:**
    ```bash
    git checkout -b feature/auth-system develop
    # Create login.html
    git commit -am "feat: scaffold login page"
    ```

### 2.3 Merging Strategies (Crucial Interview Topic)

You must understand the difference between the three merge types:

  * **Merge Commit:** Preserves all history. Used for merging `develop` to `main`.
  * **Squash Merge:** Combines all feature commits into one. Keeps the main history clean.
    ```bash
    git checkout develop
    git merge --squash feature/auth-system
    git commit -m "feat: complete auth system"
    ```
  * **Rebase:** Moves your branch to the tip of another.
    ```bash
    git checkout feature/new-ui
    git rebase develop
    # Your commits are now applied ON TOP of develop
    ```

-----

## Module 3: Surviving Merge Conflict Hell

**Objective:** Intentionally create a conflict to demystify the resolution process.

### 3.1 The Scenario

Two developers modify the same line of code in the **Backend** repo.

1.  **Base State:** Create `config.json` on `develop` containing `{"port": 8080}`.
2.  **Dev A (Branch `feat/port-3000`):** Changes port to `3000`.
3.  **Dev B (Branch `feat/port-5000`):** Changes port to `5000`.

### 3.2 The Collision

Merge Dev A first. Then try to merge Dev B.

```bash
git checkout develop
git merge feat/port-3000 # Success
git merge feat/port-5000 # CONFLICT! Automatic merge failed.
```

### 3.3 Resolution Guide

Open the file. You will see markers:

```text
<<<<<<< HEAD
{"port": 3000}
=======
{"port": 5000}
>>>>>>> feat/port-5000
```

  * **HEAD** is what is currently on `develop`.
  * **Bottom** is what is coming in.
  * **Task:** Delete the markers. Choose a port (e.g., 8000). Save. Run `git add .` and `git commit`.

-----

## Module 4: Cleaning History with Interactive Rebase

**Objective:** Present a professional commit history. "WIP" (Work In Progress) commits should never reach the `main` branch.

### 4.1 The Dirty History

Create a branch `feature/messy`. Make 4 commits:

1.  "update readme"
2.  "oops typo"
3.  "fixing typo again"
4.  "final fix"

### 4.2 The Magic Command

```bash
git rebase -i HEAD~4
```

This opens your text editor with a list of commits.

  * **Pick:** Use this commit.
  * **Squash (s):** Meld this commit into the previous one.
  * **Fixup (f):** Meld into previous one, but discard the log message.

**Action:** Change the last 3 commits to `fixup`. Save and exit. You now have one single, clean commit.

> **Warning:** Never rebase a public branch that others are working on. Rebase is for *your* local cleanup.

-----

## Module 5: Monorepos & Submodules

**Objective:** Simulate a complex corporate environment where multiple projects live in one "Monorepo."

### 5.1 Why Submodules?

Submodules allow you to keep a Git repository as a subdirectory of another Git repository.

### 5.2 Implementation

1.  Create a root folder `collab-hub-monorepo`.
2.  Link your previous repos:
    ```bash
    git submodule add <url-to-frontend> packages/client
    git submodule add <url-to-backend> packages/api
    ```
3.  **The "Gotcha":** When you clone a repo with submodules, the folders are empty by default.
      * **Fix:** `git submodule update --init --recursive`

-----

## Module 6: Project Management with GitHub Issues

**Objective:** Link code to business logic.

1.  **Kanban Boards:** Go to GitHub Projects. Create "Todo", "Doing", "Done".
2.  **Smart Commits:**
      * Create an Issue: "Fix API Crash" (Issue \#5).
      * Write code.
      * Commit: `git commit -m "fix: prevent null pointer exception. Closes #5"`
      * **Result:** When you merge this, GitHub automatically closes the issue and moves the card on the board.

-----

## Module 7: CI/CD Pipelines with GitHub Actions

**Objective:** If it isn't automated, it doesn't exist. Block bad code from merging.

### 7.1 The YAML Workflow

In `collab-hub-frontend`, create `.github/workflows/ci.yml`.

```yaml
name: Production Guard
on: [push, pull_request]

jobs:
  test-suite:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install Node
        uses: actions/setup-node@v3
        with:
          node-version: '16'
      - name: Run Tests
        run: echo "Running tests..." # Replace with 'npm test'
```

### 7.2 Branch Protection Rules

1.  Go to GitHub Repo Settings -\> Branches.
2.  Add rule for `main`.
3.  Check **"Require status checks to pass before merging"**.
4.  **Task:** Try to push code that fails the test. GitHub will physically prevent the merge button from working.

-----

## Module 8: Debugging with Bisect & Blame

**Objective:** Use algorithmic debugging to find bugs in massive codebases.

### 8.1 Git Blame

Who wrote this broken line of code?

```bash
git blame -L 10,20 app.js
```

This shows the author and commit hash for lines 10-20.

### 8.2 Git Bisect (Binary Search)

When you have 100 commits and don't know which one caused a crash:

1.  `git bisect start`
2.  `git bisect bad` (Current version is broken).
3.  `git bisect good <commit-hash-from-last-week>`
4.  Git will automatically checkout the middle commit. You test it.
      * If broken: `git bisect bad`
      * If working: `git bisect good`
5.  Repeat until Git says: **"Commit xyz is the first bad commit."**

-----

## Module 9: Disaster Recovery Drills

**Objective:** Fix "Career-Ending" mistakes.

### Drill 1: The Accidental Reset

You ran `git reset --hard` and deleted work that wasn't pushed.

  * **Solution:** `git reflog`. This is the "Safety Net" that records every movement of HEAD. Find the hash before the reset and checkout to it.

### Drill 2: Reverting a Merge

You merged a bug into `main` and pushed it.

  * **Solution:** `git revert -m 1 <merge-commit-hash>`. This creates a new commit that mathematically negates the changes of the merge.

-----

## Module 10: The Final Boss Simulation

**Time Limit:** 60 Minutes.
**Role:** Lead Engineer.

**The Mission:**

1.  Setup the `collab-hub-monorepo` fresh.
2.  Create a `release/v1.0.0` branch.
3.  **Task:** Implement a feature that spans both Frontend and Backend.
4.  **Constraint:** You must squash your local commits.
5.  **Constraint:** You must open a Pull Request and get a green CI check.
6.  **Constraint:** You must tag the release (`git tag v1.0.0`) and push tags.
7.  **Chaos:** Introduce a bug, use `bisect` to find it, and `cherry-pick` a fix from another branch.

-----

### Conclusion

If you complete this simulation, you are no longer a beginner. You possess the version control skills required for Senior DevOps and Software Engineering roles.

**Next Steps:**

  * Add this project to your resume under "DevOps Simulations."
  * Star this guide if it helped you master the Git CLI.