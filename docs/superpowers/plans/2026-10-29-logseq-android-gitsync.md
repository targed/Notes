# Logseq Android GitSync Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Establish fully automated two-way Git synchronization for Logseq on Android using the GitSync companion application, completing the unified sync pipeline across Windows, Linux, and Android.

**Architecture:** The open-source GitSync application maintains a local clone of `https://github.com/targed/Logseq.git` in Android shared storage (`Documents/Logseq`). When Logseq opens, GitSync pulls latest upstream commits; when Logseq closes, GitSync stages, commits, and pushes edits directly to GitHub.

**Tech Stack:** Android (10+), GitSync (ViscousPot), Logseq Android App v0.10+, GitHub.

## Global Constraints

- Do not modify or delete the legacy Google Drive folder until the new GitSync pipeline is fully tested and verified.
- Ensure GitSync has unrestricted background battery permissions to prevent Android Doze from suspending background pushes.
- Notes must reside in standard user-accessible storage (`/storage/emulated/0/Documents/Logseq`) so Logseq can open the graph directly.

---

### Task 1: Install GitSync & Configure Android Permissions

**Files / Devices:** Android Smartphone

- [ ] **Step 1: Install the GitSync application**
  * Open Google Play Store on your Android phone and search for **GitSync** (by *ViscousPotential*).
  * *Alternative*: Download the latest release APK from the official [GitHub Repository](https://github.com/ViscousPot/GitSync/releases).

- [ ] **Step 2: Grant Storage Permissions (All Files Access)**
  * Open GitSync.
  * When prompted, grant **All Files Access** (`MANAGE_EXTERNAL_STORAGE`).
  * If not prompted automatically: Navigate to **Android Settings > Apps > GitSync > Permissions > Storage / Files and media > Allow management of all files**.

- [ ] **Step 3: Exempt GitSync from Battery Optimization**
  * Open **Android Settings > Apps > GitSync > Battery**.
  * Select **Unrestricted** (prevents Android Doze from killing GitSync when syncing in the background).

---

### Task 2: Authenticate GitHub & Clone Graph Repository

**Files / Devices:** Android Smartphone, GitHub Account

- [ ] **Step 1: Set up GitHub Authentication in GitSync**
  * Open GitSync and tap the **Settings / Accounts** tab.
  * Tap **Add Account > GitHub**.
  * Choose **OAuth** (logs in via your web browser) OR generate a GitHub **Personal Access Token (Classic)** with the `repo` scope and paste it in.
  * Verify account shows as connected.

- [ ] **Step 2: Clone the Logseq Repository**
  * In GitSync, tap the **Repositories** tab and tap **+ (Clone Repository)**.
  * Set **Repository URL**: `https://github.com/targed/Logseq.git`
  * Set **Target Directory / Path**: `Documents/Logseq` (inside internal storage).
  * Branch: `master`
  * Tap **Clone** and allow the ~300 MB repository to download completely.

- [ ] **Step 3: Verify Cloned Files**
  * In GitSync or your Android file manager, inspect `Documents/Logseq`.
  * Confirm `pages/`, `journals/`, `whiteboards/`, and `assets/` are present.

---

### Task 3: Configure GitSync Automation Rules for Logseq

**Files / Devices:** GitSync App on Android

- [ ] **Step 1: Set Target Application**
  * Open GitSync and go to the repository settings for `Logseq`.
  * In the **Automation / App Triggers** section:
    * Select **Target Application** $\rightarrow$ choose **Logseq**.

- [ ] **Step 2: Configure Launch & Exit Actions**
  * Toggle **Sync on App Open**: Enabled (executes `git pull --rebase` before or upon opening Logseq).
  * Toggle **Sync on App Close**: Enabled (executes `git add .`, commit, and `git push` upon exiting/minimizing Logseq).
  * Set **Default Commit Message**: `mobile sync: $(datetime)` or `Auto saved from Android`.

- [ ] **Step 3: (Optional) Add Quick Settings Tile / Home Screen Widget**
  * Edit your Android Quick Settings panel (swipe down twice $\rightarrow$ pencil icon) and add the **GitSync** tile for instant 1-tap manual sync anytime.

---

### Task 4: Link Logseq Android App to Cloned Graph

**Files / Devices:** Logseq Android App

- [ ] **Step 1: Open Graph in Logseq Android**
  * Launch the **Logseq** app on your phone.
  * Tap the graph selector menu in the top-left corner.
  * Tap **Add a local graph** (or **Open local folder**).
  * Browse to and select `Documents/Logseq`.
  * Tap **Use this folder** and confirm storage permission when prompted by Android SAF.

- [ ] **Step 2: Perform Initial Re-Index**
  * Tap the left menu bar in Logseq.
  * Tap your graph name and select **Re-index**.
  * Wait 1–2 minutes for Logseq to parse the Markdown notes and compile its internal search database.
  * Verify your existing notes and journals display properly.

---

### Task 5: End-to-End Two-Way Verification Across Devices

**Files / Devices:** Android Smartphone, Windows/Linux Desktop, GitHub

- [ ] **Step 1: Test Android $\rightarrow$ GitHub $\rightarrow$ Desktop Sync**
  1. Open today's journal in Logseq on Android.
  2. Write a test block: `- Hello from Android GitSync test [timestamp]`.
  3. Close or swipe away Logseq.
  4. Check GitSync (or watch notification shade) to verify commit and push to GitHub succeed.
  5. On your Windows PC (or Linux box), run:
     ```powershell
     git -C "C:\Users\ltkli\Documents\GitHub\Logseq" pull --rebase
     ```
  6. Confirm the Android test block appears cleanly in your desktop Logseq notes.

- [ ] **Step 2: Test Desktop $\rightarrow$ GitHub $\rightarrow$ Android Sync**
  1. On your desktop, add a test note in Logseq: `- Hello from Desktop test [timestamp]`.
  2. Wait 60 seconds or close Logseq (confirm commit and push in GitHub).
  3. Unlock phone and open Logseq.
  4. Confirm GitSync pulls changes and the desktop note appears in Logseq Android.

---

### Task 6: Decommission Legacy Android DriveSync

**Files / Devices:** Android Smartphone

- [ ] **Step 1: Disable DriveSync Sync Pair**
  * Open the **DriveSync** app on your phone.
  * Navigate to **Synced Folders**.
  * Pause or delete the sync pair pointing to the old Google Drive graph.

- [ ] **Step 2: Remove or Archive Old Google Drive Cache**
  * Verify that `Documents/Logseq` is now your sole working graph on Android.
  * You can now safely uninstall DriveSync to reclaim battery and background resources.
