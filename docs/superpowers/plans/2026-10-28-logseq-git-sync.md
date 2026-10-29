# Logseq Git Auto-Commit and Push Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Migrate the user's Logseq graph from Google Drive to `C:\Users\ltkli\Documents\GitHub\Logseq`, configure a native Git `post-commit` hook for automated remote pushes, and enable Logseq's built-in Git auto-commit.

**Architecture:** Use Logseq's native version control settings (`Enable Git auto commit` and `Git commit on window close`) combined with a Git `post-commit` hook (`.git/hooks/post-commit`) in `C:\Users\ltkli\Documents\GitHub\Logseq`. The hook intercepts every local commit made by Logseq and immediately pushes changes to GitHub (`https://github.com/targed/Logseq.git`). The Google Drive directory is detached to prevent file locking and database corruption.

**Tech Stack:** Git for Windows, PowerShell, Logseq Desktop v0.10+, GitHub.

## Global Constraints

- Never initialize or sync `.git` inside Google Drive (`C:\Users\ltkli\My Drive\...`).
- Do not modify or delete files in the original Google Drive directory (`C:\Users\ltkli\My Drive\DriveSyncFiles\My Graph`) during migration; keep it intact as a safety fallback.
- `.git/hooks/post-commit` must be saved with Unix LF line endings (`\n`), not CRLF.
- No third-party Logseq plugins required (pure native configuration).

---

### Task 1: Migrate Graph Contents and Configure `.gitignore`

**Files:**
- Source: `C:\Users\ltkli\My Drive\DriveSyncFiles\My Graph`
- Destination: `C:\Users\ltkli\Documents\GitHub\Logseq`
- Create: `C:\Users\ltkli\Documents\GitHub\Logseq\.gitignore`

**Interfaces:**
- Consumes: Existing graph files (`pages/`, `journals/`, `logseq/`, `assets/`)
- Produces: Populated clean workspace in `C:\Users\ltkli\Documents\GitHub\Logseq` with excluded cache files

- [ ] **Step 1: Create tailored `.gitignore` in `C:\Users\ltkli\Documents\GitHub\Logseq\.gitignore`**

```gitignore
# Logseq local caches and transient backups
logseq/.recycle/
logseq/bak/
.DS_Store
Thumbs.db
desktop.ini
*.tmp
```

- [ ] **Step 2: Copy graph directories and files from Google Drive to local repository**

Run in PowerShell:
```powershell
$source = "C:\Users\ltkli\My Drive\DriveSyncFiles\My Graph"
$dest = "C:\Users\ltkli\Documents\GitHub\Logseq"

Copy-Item -Path "$source\*" -Destination $dest -Recurse -Force
```

- [ ] **Step 3: Verify destination file structure**

Run:
```powershell
Get-ChildItem -Path "C:\Users\ltkli\Documents\GitHub\Logseq"
```
Expected: Listing includes `pages`, `journals`, `logseq`, `.gitignore`, and any custom folders/assets.

---

### Task 2: Baseline Git Repository Setup & Initial Remote Push

**Files:**
- Directory: `C:\Users\ltkli\Documents\GitHub\Logseq`
- Config: `C:\Users\ltkli\Documents\GitHub\Logseq\.git\config`

**Interfaces:**
- Consumes: Populated files in `C:\Users\ltkli\Documents\GitHub\Logseq`
- Produces: Committed and pushed baseline on GitHub remote (`https://github.com/targed/Logseq.git`)

- [ ] **Step 1: Check git status in repository**

Run:
```powershell
git -C "C:\Users\ltkli\Documents\GitHub\Logseq" status
```
Expected: Untracked files showing `pages/`, `journals/`, `logseq/`, `.gitignore`.

- [ ] **Step 2: Stage all files and create baseline commit**

Run:
```powershell
git -C "C:\Users\ltkli\Documents\GitHub\Logseq" add .
git -C "C:\Users\ltkli\Documents\GitHub\Logseq" commit -m "feat: initial import of Logseq graph notes"
```
Expected: Output showing created files committed.

- [ ] **Step 3: Push baseline commit to GitHub**

Run:
```powershell
git -C "C:\Users\ltkli\Documents\GitHub\Logseq" push -u origin master
```
*(If branch is `main`, use `main`)*
Expected: Output showing `master -> master` pushed successfully.

---

### Task 3: Implement Automated Git `post-commit` Push Hook

**Files:**
- Create: `C:\Users\ltkli\Documents\GitHub\Logseq\.git\hooks\post-commit`

**Interfaces:**
- Consumes: Any local Git commit triggered by Logseq or CLI
- Produces: Automatic background `git push origin HEAD` and logs to `.git/post-commit.log`

- [ ] **Step 1: Write `post-commit` hook script with LF line endings**

Write to `C:\Users\ltkli\Documents\GitHub\Logseq\.git\hooks\post-commit`:
```sh
#!/bin/sh
export GIT_TERMINAL_PROMPT=0
git push origin HEAD > "$GIT_DIR/post-commit.log" 2>&1
```

- [ ] **Step 2: Test hook execution with a dummy commit**

Run:
```powershell
git -C "C:\Users\ltkli\Documents\GitHub\Logseq" commit --allow-empty -m "chore: test post-commit hook"
```
Expected: Commit completes, and hook runs.

- [ ] **Step 3: Verify push was performed and check hook log**

Run:
```powershell
Get-Content "C:\Users\ltkli\Documents\GitHub\Logseq\.git\post-commit.log"
git -C "C:\Users\ltkli\Documents\GitHub\Logseq" status
```
Expected: Output indicates `Your branch is up to date with 'origin/master'` and no errors in `post-commit.log`.

---

### Task 4: Configure Logseq Application & End-to-End Validation

**Files:**
- Logseq App Settings
- Test page in Logseq

**Interfaces:**
- Consumes: Configured Git repo and hook
- Produces: Fully automated commit + push upon note editing or window close

- [ ] **Step 1: Open graph in Logseq**
  1. Open Logseq.
  2. In the top-left corner, click your current graph name and select **Open graph**.
  3. Browse to and select `C:\Users\ltkli\Documents\GitHub\Logseq`.
  4. Wait for Logseq to re-index the notes.

- [ ] **Step 2: Configure Version Control in Logseq**
  1. Open **Settings** (gear icon or press `,` then `s`).
  2. Click **Version control** in the left menu.
  3. Toggle **Enable Git auto commit** to ON.
  4. Toggle **Git commit on window close** to ON.
  5. Set **Git auto commit seconds** to `60` (or your preferred interval).

- [ ] **Step 3: Test live automatic sync**
  1. Open today's journal or any page in Logseq.
  2. Add a test bullet: `- Test git auto-backup at [timestamp]`.
  3. Wait 60-70 seconds without closing Logseq, or close the window.
  4. Run in terminal:
  ```powershell
  git -C "C:\Users\ltkli\Documents\GitHub\Logseq" log -n 2 --oneline
  git -C "C:\Users\ltkli\Documents\GitHub\Logseq" status
  ```
  Expected: A new commit authored by Logseq (e.g., `git auto commit ...`) appears in `git log`, and `git status` confirms `Your branch is up to date with 'origin/master'`.
