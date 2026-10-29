# Logseq Git Backup & Multi-Device Sync Guide (Windows, Linux, Android)

This guide documents the architecture, setup instructions, troubleshooting history, and daily workflow for synchronizing a Logseq graph across **Windows**, **Arch Linux**, and **Android** using GitHub.

---

## 1. Context & Motivation

### The Original Setup
* **Windows**: Notes stored in `C:\Users\ltkli\My Drive\DriveSyncFiles\My Graph` syncing via Google Drive for Desktop.
* **Android**: Synced using the third-party **DriveSync** app against Google Drive.
* **Linux**: Synced using **rclone** against Google Drive.

### The Pain Points
* **rclone on Linux** was sluggish, lacked real-time sync, hit Google Drive API rate limits, and had no automated conflict handling.
* **DriveSync on Android** caused battery drain, sync delays, and frequent duplicate conflict files (`Note (1).md`) due to collisions with Logseq's internal file indexing.
* **No Version History**: Google Drive only syncs raw files. Accidental block deletions, graph corruptions, or bad edits could not be reverted via atomic history or diffs.

---

## 2. Why Git & Key Architectural Decisions

### 2.1 Why Not Put `.git` Inside Google Drive?
Hosting a `.git` repository inside an active cloud synchronization folder (Google Drive, OneDrive, Dropbox, or iCloud) is an **anti-pattern**:
* **File-Lock Collisions**: Git operations create and destroy lockfiles rapidly (`.git/index.lock`, `refs/heads/*.lock`). Google Drive actively scans and locks these files mid-operation, crashing Git with `fatal: Unable to create '.../.git/index.lock': File exists`.
* **Database Corruption**: Git stores objects in a local content-addressed database (`.git/objects`). If Google Drive syncs files out of order or creates duplicate files (e.g., `index (1)`), the repository becomes corrupted and unrecoverable.
* **Decision**: Decouple Git completely from Google Drive. Keep the Git working copy in a dedicated local folder (`Documents/GitHub/Logseq` on Windows, `~/Documents/Logseq` on Linux, and `Documents/Logseq` on Android).

### 2.2 Why Native Git Hooks Over Plugins or External Daemons?
* Logseq has a built-in auto-commit feature (**Settings > Version control**), but it **only commits locally**—it does not push to GitHub.
* Third-party plugins (like `logseq-plugin-git`) add UI buttons, but are external dependencies that can break during Logseq major upgrades.
* External sync daemons (`logseq/git-auto`) add background OS processes to manage.
* **The Solution**: A native Git `post-commit` hook (`.git/hooks/post-commit`). When Logseq triggers an auto-commit, Git automatically fires the hook to run `git push origin HEAD` silently in the background. **Zero plugins, zero background services.**

---

## 3. Windows Configuration

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

## 4. Arch Linux Setup & Troubleshooting History

### Problem 1: Silent failure of Logseq auto-commits on Arch Linux
* **Cause**: Logseq's bundled `dugite` package expected Git at an unpacked directory (`app.asar.unpacked/node_modules/dugite/git/bin/git`). The Arch AUR package archives everything into `app.asar` without unpacking, causing Git calls to fail silently.
* **Solution**: Point `dugite` to the system Git binaries via environment variables:
  ```bash
  mkdir -p ~/.config/environment.d
  cat << 'EOF' > ~/.config/environment.d/logseq.conf
  LOCAL_GIT_DIRECTORY=/usr
  GIT_EXEC_PATH=/usr/lib/git-core
  EOF
  ```
  And override the launcher in `~/.local/share/applications/logseq-desktop.desktop` to ensure the environment variables are active when launched from desktop menus.

### Problem 2: Expired GitHub CLI credentials blocking background pushes
* **Cause**: The global git config used `gh auth git-credential`, whose token had expired. Because the hook sets `export GIT_TERMINAL_PROMPT=0`, pushes failed silently into `.git/post-commit.log`.
* **Solution**: Re-authenticating with `gh auth login -h github.com` restored non-interactive background pushes.

### Automatic Pull on Launcher Startup
To avoid uncommitted merge conflicts while ensuring notes written on other devices are pulled before editing:
```ini
[Desktop Entry]
Type=Application
Name=Logseq
Exec=bash -c "git -C /home/targed/Documents/Logseq pull --rebase --autostash; env LOCAL_GIT_DIRECTORY=/usr GIT_EXEC_PATH=/usr/lib/git-core logseq %u"
Icon=logseq
Terminal=false
StartupNotify=true
Categories=Office;
```

---

## 5. Android Setup (via GitSync)

Because Logseq Android has no built-in Git binaries and mobile terminal scripts require manual button-tapping, we use **GitSync** ([`ViscousPot/GitSync`](https://github.com/ViscousPot/GitSync)) to provide automatic pull on open and commit/push on close.

### Step 1: Install GitSync & Grant Permissions
1. Install **GitSync** from the **Google Play Store** (by *ViscousPotential*) or via [GitHub Releases](https://github.com/ViscousPot/GitSync/releases).
2. Open GitSync and grant **All Files Access** (`MANAGE_EXTERNAL_STORAGE`).
3. In **Android Settings > Apps > GitSync > Battery**, set battery usage to **Unrestricted** (prevents Android Doze from suspending background syncs).

### Step 2: Connect GitHub & Clone Graph
1. In GitSync, go to **Settings / Accounts > Add Account > GitHub**.
2. Sign in via OAuth (or generate a Personal Access Token with `repo` scope).
3. In the **Repositories** tab, tap **+ (Clone)**:
   * **URL**: `https://github.com/targed/Logseq.git`
   * **Path**: `Documents/Logseq`
   * **Branch**: `master`
4. Tap **Clone** and let the notes download completely.

### Step 3: Configure Automation Triggers
1. Open repository settings for `Logseq` inside GitSync.
2. In **Automation / App Triggers**:
   * Set **Target Application**: `Logseq`
   * Enable **Sync on App Open** (pulls upstream changes before you start editing).
   * Enable **Sync on App Close** (stages, commits, and pushes notes when Logseq is closed/minimized).
   * (Optional) Add the **GitSync tile** to your Android Quick Settings notification shade for 1-tap manual sync anytime.

### Step 4: Link Logseq Android to the Cloned Graph
1. Open the **Logseq** app on Android.
2. Click the graph switcher in top-left $\rightarrow$ **Add a local graph**.
3. Browse to and select `Documents/Logseq`. Confirm storage access.
4. Open the left menu $\rightarrow$ tap your graph name $\rightarrow$ select **Re-index** to build the mobile cache.

---

## 6. Daily Multi-Device Workflow

```
[Desktop (Win/Linux)] ---> Launch app pulls upstream ---> Edit notes auto-commit (60s) ---> Close app pushes to GitHub
                                                                    ^
                                                                    |
                                                            GitHub: targed/Logseq
                                                                    |
                                                                    v
[Mobile (Android)]    ---> Open app: GitSync pulls    ---> Edit notes in Logseq       ---> Close app: GitSync auto-pushes
```

* **No manual terminal commands needed in daily use.**
* Every device pulls when Logseq starts, and pushes when Logseq closes.

---

## 7. Retiring Legacy Google Drive / DriveSync

Once you verify notes flow smoothly across Windows, Linux, and Android:
1. On Android: Open **DriveSync** $\rightarrow$ delete/pause the sync pair pointing to Google Drive.
2. On Linux: Disable any remaining `rclone` background timers or cron jobs.
3. On Google Drive: The old folder (`My Graph`) can be left as a frozen historical backup or archived.
