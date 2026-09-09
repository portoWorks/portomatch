# portoMatch — Compare · Curate · Archive

**portoMatch** is a desktop app that helps you find and remove duplicate files between two folders — without accidentally deleting anything that isn't already safely backed up.

---

## The problem it solves

You have files in two places — a working folder and a backup drive, a downloads folder and a photo library, a laptop and a NAS. Over time, thousands of files accumulate in the working folder that already exist in the backup. portoMatch finds them so you can safely delete only what you already have a copy of.

---

## How it works

### 1. Pick two folders

- **Target** — the folder you want to clean up (e.g. your downloads or staging folder)
- **Reference** — the folder that has the authoritative copies (e.g. your backup drive or photo library)

The app scans both. Nothing is ever deleted from the reference folder.

If you choose a different folder after a scan (Browse, the dropdown, Recent pairs, or Enter in the field), the app asks whether to rescan right away. Say No to keep the current results; the button then reads **Update Results** until you rescan.

Once the folders are set, the small ▴ button at the right of the card collapses it to a one-line summary so the results get the room; ▾ expands it again. The app remembers your choice.

### 2. Run the comparison

Click **Compare**. The app checks every file in the target against the reference using multiple signals:

- File size
- Modification time
- A fast content fingerprint (first and last 64 KB of each file)
- Pixel dimensions for photos and videos
- EXIF camera ID tags for images
- Optional: full byte-for-byte verification

Files that pass these checks are reported as **matched** — confirmed to already exist in the reference. Files that don't match anything are **unmatched** — the app will not touch them.

### 3. Review the results

Results appear in a list with thumbnail previews. For each matched file you can see:

- The target file and its confirmed reference copy, side by side
- A confidence level: **Exact**, **Strong**, or **Review**
- EXIF metadata (date taken, camera, keywords, etc.)

You can filter by filename, file type, size, or status, and switch between a list view and a thumbnail gallery.

### 4. Delete with confidence

Select files and click **Delete**. Three deletion modes are available:

- **Recycle Bin** — files go to the Recycle Bin and can be recovered
- **Custom trash folder** — files are moved to a folder you choose
- **Permanent** — files are deleted immediately (use with care)

Every deletion is recorded in a CSV audit log so you always have a record of what was removed and when.

### 5. Reject what you don't want

Rejecting is a verdict on the *file*, not on the match: "I don't want this one", whatever the comparison said.

- Press **X** (or right-click → **Reject**) on selected files in any view, including the target side of the Partial and Reconcile panes. They leave that view and appear under the **🚫 Rejected** button.
- The Rejected view has a **Match** column that says why each file was there (Exact, Visual 94%, Partial, Unmatched, …).
- From there: **Move to _Rejected** (a `_Rejected` folder under Target, subfolders kept), **Prefix names** (renames in place with `Rejected_`, right-click → Remove prefix to reverse), **Delete** (your usual deletion mode), or **Unreject** (X) to put a file back where it came from.
- Sidecar files (.xmp, .json) follow the photo; moves and renames can be undone for 10 seconds and are recorded in `folder_comparator_rejections.csv`. Re-Compare keeps your rejections. Nothing is ever deleted from the reference folder.
- Rejections are remembered. They are saved in scan snapshots (File → Save Snapshot) and in the match state the app offers to save when you quit, and re-applied the next time you scan the same two folders (a file that changed since is not rejected again).

---

## Image and video support

The app has deep support for photos and videos:

- **Photos:** JPEG, PNG, HEIC, GIF, BMP, WebP, TIFF, and all major RAW formats (Canon CR2/CR3, Nikon NEF, Sony ARW, Fujifilm RAF, Adobe DNG, and more)
- **Videos:** MP4, MOV, AVI, MKV, MTS/M2TS, and others
- **Perceptual matching:** Images that are visually identical but have been re-saved, re-exported, or lightly edited can be flagged as near-duplicates using perceptual hashing — even if their file size differs
- **Rotation-aware:** Photos that have been rotated are still recognized as duplicates

---

## Other features

**Alternate folder check** — if your archive spans multiple drives, you can search an additional reference location for files that didn't match the primary scan.

**Visual match view** — a separate view for near-duplicate images that are similar but not identical (useful for finding re-exports or lightly edited versions).

**Mirror Content view** — finds files with identical content but different modification timestamps, which can happen when copying across filesystems or cloud services.

**Smart auto-select** — automatically selects files for deletion based on rules, so you don't have to click through thousands of rows manually.

**Sweep empty folders** — after deleting matched files, removes any empty subfolders left behind in the target.

**Export** — save results as an HTML report or CSV spreadsheet.

**Session save/restore** — save a comparison in progress and pick up where you left off later.

**Cold storage manifests** — generate SHA-256 integrity manifests for archive drives, so you can verify the contents of offline storage over time.

---

## What it will never do

- Delete, move or overwrite anything in the **Reference** folder (the only thing that can reach it is a copy you explicitly promote into it)
- Propose a deletion without first confirming a copy exists in the reference
- Treat a file marked **Unmatched** as safe to delete — those are files with no confirmed copy; deleting one is only possible after you reject it yourself and confirm the deletion

---

## Updates

portoMatch never checks for updates on its own. About once a month it shows a small
in-app notice — "Check for updates now?" with **Check** and **Not now** — and only
clicking **Check** fetches anything. Either answer resets the month. **Help → Check for
Updates…** always works for an on-demand check, any time.

- **What is fetched (only when you click Check):** the latest release information (version number, download page link, release notes) from the project's public GitHub page — nothing else.
- **What is sent:** nothing about you or your files. The request carries no personal data, just an app-name/version header so GitHub can see which app is asking.
- **How to turn it off:** uncheck **Ask me once a month whether to check for updates** in Settings → General. The Help menu entry still works for a manual, one-off check.
- **What it never does:** the app never downloads or installs anything by itself, and never connects to the network without you clicking Check. If a newer version exists, it only shows you a link to the download page — you decide whether to get it.

---

## Pro Features

Everything above is free. A Pro licence additionally unlocks:

- **Alternate layouts** — Gallery + Inspector, Dense List, and Compare-First views (switch anytime under View → Layout; Classic stays free).
- **Reconcile Mode** — a dual-pane backup audit for verifying an entire mirror/backup copy against its source.
- **Partial Match review** — a dedicated view for reviewing partial (not exact or visual) matches.
- **Signature cache** — remembers file signatures across sessions for much faster re-scans of large libraries, and powers **Find Rotated Copy**, which depends on it.
- **Cold Storage Pro** — multi-drive manifests, audits, spin-up reminders, and S.M.A.R.T. health checks that keep your cold-storage drives and backups verified, plus cold-storage cross-checks during a compare.
- **EXIF editor** and the **provenance row** — see (and edit) which software last touched a file.
- **Visual duplicates** in the single-folder Duplicate Finder.
- **Build Reference Index** — pre-index a large reference root for instant lookups on future scans.
- **Backup Sync Mode** — replays the deletions you made in a folder onto its backup copy, from the audit log, with a full preview before anything is removed.
- **Commercial use**, and no periodic reminder notice.

---

## Licence

portoMatch is free for personal use. A Pro licence removes the periodic notice and authorises commercial use. There are two ways to activate one, in Settings → License:

- **Offline key** (Pro tab) — a key derived from your institution name, entered once. 100% offline: no internet connection is ever needed, checked, or used.
- **Lemon Squeezy key** (second tab) — the 36-character key from your purchase email. Needs an internet connection once to activate; after that it works completely offline for up to 30 days between short, silent re-checks (once every 7 days) that confirm the key is still active.
  - **What is sent:** only the key itself and this machine's name (its hostname) — nothing about you, your files, or your usage.
  - **Moving it to another PC:** click **Deactivate this machine** in the License dialog, then activate the same key on the new PC. Each key allows 2 activations at a time, so you don't have to deactivate the old machine first if you're just trying the app on a second computer.
  - **If a re-check can't reach the server:** nothing changes — the licence keeps working offline. Only after 30 days with no successful check does it fall back to the free, personal tier, and only until the app can reach the server again.

---

## Running it

- **Windows:** double-click `run.bat`. It checks that Python and tkinter are present, lists which optional features are available, then opens the app without a console window.
  - `run.bat debug` keeps a console open so any error stays on screen.
  - `run.bat check` only runs the checks and reports what is missing.
- **Any platform:** `python fc.py` (or `python -m fc`) from the project folder.

---

## Requirements

- Windows, macOS, or Linux
- Python 3.9 or later
- Optional packages, all recommended — install in one go with `pip install -r requirements.txt`:
  - [Pillow](https://pillow.readthedocs.io/) — thumbnails, perceptual matching, EXIF reading
  - PyYAML — YAML config file (JSON is used without it)
  - send2trash — "Recycle Bin" deletion mode
  - PyMuPDF — PDF page previews
  - tkinterdnd2 — drag files out of the window

---

*portoMatch by portoWorks — version 1.3*
