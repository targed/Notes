# Logseq Git Backup & Multi-Device Sync Guide

This guide documents the architecture, rationale, decisions, and exact setup instructions for your Logseq Git synchronization system across **Windows**, **Linux**, and **Android**.

---

## 1. Context & Motivation

### The Original Setup
* **Windows**: Notes stored in `C:\Users\ltkli\My Drive\DriveSyncFiles\My Graph` syncing via Google Drive for Desktop.
* **Android**: Synced using the third-party **DriveSync** app against Google Drive.
* **Linux**: Synced using **rclone** against Google Drive.

### The Pain Points
* **rclone on Linux** was sluggish, lacked real-time sync, suffered from Google Drive API latency/rate limits, and had no automated conflict handling.
* **DriveSync on Android** caused battery drain, sync delays, and frequent duplicate conflict files (`Note (1).md`) due to collisions with Logseq's internal file indexing.
* **No Version History**: Google Drive only syncs raw files. Accidental block deletions, graph corruptions, or bad edits could not be reverted via atomic history or diffs.

---

## 2. Why Git & Key Architectural Decisions

### 2.1 Why Not Put `.git` Inside Google Drive?
Hosting a `.git` repository inside an active cloud synchronization folder (Google Drive, OneDrive, Dropbox, or iCloud) is an **anti-pattern**:
* **File-Lock Collisions**: Git operations create and destroy lockfiles rapidly (`.git/index.lock`, `refs/heads/*.lock`). Google Drive actively scans and locks these files mid-operation, crashing Git with `fatal: Unable to create '.../.git/index.lock': File exists`.
* **Database Corruption**: Git stores objects in a local content-addressed database (`.git/objects`). If Google Drive syncs files out of order or creates duplicate files (e.g., `index (1)`), the repository becomes corrupted and unrecoverable.
* **Decision**: Decouple Git completely from Google Drive. Keep the Git working copy in a dedicated local folder (`Documents/GitHub/Logseq` on Windows, `~/Documents/Logseq` on Linux).

### 2.2 Why Native Git Hooks Over Plugins or External Daemons?
* Logseq has a built-in auto-commit feature (**Settings > Version control**), but it **only commits locally**—it does not push to GitHub.
* Third-party plugins (like `logseq-plugin-git`) add UI buttons, but are external dependencies that can break during Logseq major upgrades.
* External sync daemons (`logseq/git-auto`) add background OS processes to manage.
* **The Solution**: A native Git `post-commit` hook (`.git/hooks/post-commit`). When Logseq triggers an auto-commit, Git automatically fires the hook to run `git push origin HEAD` silently in the background. **Zero plugins, zero background services.**

### 2.3 The Parallel Pilot Strategy
Instead of immediately deleting or disconnecting the Google Drive folder, we established a **risk-free parallel pilot**:
* The original Google Drive folder remains 100% untouched as a safety fallback.
* All 1,757 files (~300 MB) were copied into the Git repository at `C:\Users\ltkli\Documents\GitHub\Logseq` and pushed to [`targed/Logseq`](https://github.com/targed/Logseq).
* We validate the Git workflow on Windows and Linux first before transitioning Android and retiring Google Drive.

---

## 3. What Was Done on Windows

1. **Configured `.gitignore`**:
   Filtered out transient backups and OS junk that bloat Git repositories:
   ```gitignore
   logseq/.recycle/
   logseq/bak/
   .DS_Store
   Thumbs.db
   desktop.ini
   *.tmp
   ```
2. **Enabled Long File Paths**:
   Configured `git config core.longpaths true` to prevent Windows 260-character path limit errors on long course note filenames.
3. **Imported & Pushed Graph**:
   Copied all `pages/`, `journals/`, `whiteboards/`, `assets/`, and `logseq/` directories and pushed the baseline to `origin master`.
4. **Installed Automated `post-commit` Hook**:
   Created `C:\Users\ltkli\Documents\GitHub\Logseq\.git\hooks\post-commit` with Unix LF line endings:
   ```sh
   #!/bin/sh
   export GIT_TERMINAL_PROMPT=0
   GIT_DIR="$(git rev-parse --git-dir 2>/dev/null || echo .git)"
   git push origin HEAD > "$GIT_DIR/post-commit.log" 2>&1
   ```
5. **Configured Logseq**:
   * Pointed Logseq to `C:\Users\ltkli\Documents\GitHub\Logseq`.
   * Enabled **Enable Git auto commit** (60s interval).
   * Enabled **Git commit on window close**.

---

## 4. Linux Setup Instructions (Run When You Reboot)

When you boot into Linux, setting up the exact same automated sync takes under 3 minutes.

### Step 1: Ensure Git & GitHub Authentication Are Ready
Make sure Git is installed and you are authenticated to GitHub (via SSH key or GitHub CLI / Personal Access Token):
```bash
# Verify git is installed
git --version

# Verify GitHub SSH connection (if using SSH)
ssh -T git@github.com

# Or verify GitHub CLI authentication (if using HTTPS)
gh auth status
```

### Step 2: Clone the Repository
Clone your Logseq notes into your preferred directory (e.g., `~/Documents/Logseq`):
```bash
# If using SSH (recommended on Linux):
git clone git@github.com:targed/Logseq.git ~/Documents/Logseq

# OR if using HTTPS:
git clone https://github.com/targed/Logseq.git ~/Documents/Logseq
```

### Step 3: Install the Automated `post-commit` Push Hook
Create the executable hook inside the local repository:
```bash
cat << 'EOF' > ~/Documents/Logseq/.git/hooks/post-commit
#!/bin/sh
export GIT_TERMINAL_PROMPT=0
GIT_DIR="$(git rev-parse --git-dir 2>/dev/null || echo .git)"
git push origin HEAD > "$GIT_DIR/post-commit.log" 2>&1
EOF

# Grant execution permissions
chmod +x ~/Documents/Logseq/.git/hooks/post-commit
```

### Step 4: Test the Hook on Linux
Verify that Git triggers an automatic push when a commit happens:
```bash
cd ~/Documents/Logseq
git commit --allow-empty -m "chore: test linux post-commit hook"

# Check the log file to confirm it pushed to GitHub
cat .git/post-commit.log
```
*Expected output*: `To github.com:targed/Logseq.git ... HEAD -> master`.

### Step 5: Configure Logseq on Linux
1. Open Logseq on Linux.
2. Click the graph switcher in the top-left corner $\rightarrow$ **Open graph**.
3. Select `~/Documents/Logseq`.
4. Open **Settings** (press `,` then `s`) $\rightarrow$ **Version control**:
   * Toggle **Enable Git auto commit** $\rightarrow$ **ON**
   * Toggle **Git commit on window close** $\rightarrow$ **ON**
   * Set **Git auto commit seconds** $\rightarrow$ `60`

---

## 5. Daily Multi-Device Workflow (Windows $\leftrightarrow$ Linux)

Because Git is decentralized, you must ensure you have the latest notes before starting work on a different machine:

### Starting Your Work Session
Before opening Logseq on a machine, pull the latest changes made on the other machine:
```bash
# On Linux:
cd ~/Documents/Logseq && git pull

# On Windows (PowerShell or Git Bash):
cd "C:\Users\ltkli\Documents\GitHub\Logseq"; git pull
```

> [!TIP]
> **Convenient Linux Launch Shortcut**:
> You can create a one-line launcher script or shell alias in `~/.bashrc` or `~/.zshrc`:
> ```bash
> alias logseq-sync="cd ~/Documents/Logseq && git pull && logseq &"
> ```
> This automatically pulls down any notes you wrote on Windows right before launching Logseq on Linux.

### Ending Your Work Session
You don't need to do anything manually! When you close Logseq (or wait 60 seconds after your last keystroke), Logseq auto-commits and the `post-commit` hook pushes the changes straight to GitHub.

---

## 6. What Remains to Be Done (Next Steps)

1. **Verify Windows $\leftrightarrow$ Linux Two-Way Sync**:
   * Edit a note on Linux, close Logseq, check GitHub.
   * Boot into Windows, run `git pull`, and verify the note appears.
2. **Determine the Android Strategy**:
   Once desktop sync is validated, choose one of these two mobile paths:
   * **Option A: Termux + Git (Direct Git)**
     Install Termux and the Termux:Widget app on Android. Place a home screen widget button that runs `git pull && git push` against your notes folder.
   * **Option B: Syncthing (Phone $\leftrightarrow$ PC) + Git (PC $\rightarrow$ GitHub)**
     Use Syncthing to silently mirror your notes folder peer-to-peer between your Android phone and your PC/Linux machine when on the same Wi-Fi. The PC continues auto-pushing version history to GitHub.
3. **Retire Google Drive / DriveSync / rclone**:
   Once you are completely comfortable with Git on all devices, archive the old Google Drive folder and remove DriveSync/rclone background tasks.
