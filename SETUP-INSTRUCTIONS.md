# 🛠️ Setup Instructions — Upload to GitHub & Enable GitHub Pages

This guide walks you through every step to get the Tableau Auto-Resize Extension hosted on GitHub Pages and working in Tableau Desktop.

**Estimated time: 5–10 minutes.**

---

### Before You Begin

Make sure you have:

- [ ] A **GitHub account** ([sign up free](https://github.com/signup))
- [ ] **Tableau Desktop 2018.2** or later installed
- [ ] The extension files from this folder:
  - `auto-resize.html`
  - `auto-resize.trex`

---

### Step 1 — Create a New GitHub Repository

1. Go to **[github.com/new](https://github.com/new)**
2. Fill in the form:

   | Field | Value |
   |---|---|
   | **Repository name** | Choose a name, e.g. `tableau-auto-resize` |
   | **Description** | *(optional)* "Tableau extension that auto-resizes dashboard height" |
   | **Visibility** | **Public** ⚠️ *(Required for free GitHub Pages)* |
   | **Initialize with README** | Leave **unchecked** (we'll upload our own) |

3. Click **Create repository**

4. 📝 **Write down your details — you'll need them in Step 4:**

   ```
   My GitHub username:   ___________________________
   My repository name:   ___________________________
   ```

---

### Step 2 — Upload the Extension Files

#### Option A: Upload via GitHub Web Interface (Easiest)

1. On your new repository's page, click **"uploading an existing file"** (or go to `https://github.com/YOUR-USERNAME/YOUR-REPO-NAME/upload/main`).
2. **Drag and drop** all four files from this folder:
   - `auto-resize.html`
   - `auto-resize.trex`
   - `README.md`
   - `SETUP-INSTRUCTIONS.md`
3. In the commit message box, type: `Add Tableau auto-resize extension files`
4. Click **Commit changes**

#### Option B: Upload via Git Command Line

```bash
# Clone your new empty repository
git clone https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
cd YOUR-REPO-NAME

# Copy all extension files into this directory
# (adjust the source path to where you downloaded the files)
cp /path/to/tableau-github-upload/* .

# Commit and push
git add -A
git commit -m "Add Tableau auto-resize extension files"
git push origin main
```

> 💡 If your default branch is `master` instead of `main`, use `master` in the commands above and in Step 3.

---

### Step 3 — Enable GitHub Pages

1. Go to your repository on GitHub.
2. Click the **Settings** tab (gear icon, top right of the repo).
3. In the left sidebar, click **Pages**.
4. Under **"Build and deployment"**:

   | Setting | Value |
   |---|---|
   | **Source** | **Deploy from a branch** |
   | **Branch** | **`main`** (or `master`) |
   | **Folder** | **`/ (root)`** |

5. Click **Save**.
6. ⏳ **Wait 1–2 minutes** for GitHub to build and deploy your site.
7. Refresh the page — you should see a green banner:

   > ✅ **Your site is live at `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/`**

8. 📝 **Write down your GitHub Pages URL:**

   ```
   My GitHub Pages URL:  https://_____________________.github.io/____________________/
   ```

---

### Step 4 — ⚠️ Update the `.trex` File with Your URL

**This is the critical step.** The `.trex` file currently contains a placeholder URL that **will not work** until you replace it.

1. Open `auto-resize.trex` in any text editor (Notepad, VS Code, TextEdit, etc.).

2. Find this line (around line 18):

   ```xml
   <url>https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/auto-resize.html</url>
   ```

3. **Replace `YOUR-USERNAME`** with your actual GitHub username.

4. **Replace `YOUR-REPO-NAME`** with your actual repository name.

5. **Example — before and after:**

   **BEFORE (placeholder — won't work):**
   ```xml
   <url>https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/auto-resize.html</url>
   ```

   **AFTER (with real values):**
   ```xml
   <url>https://jsmith.github.io/tableau-auto-resize/auto-resize.html</url>
   ```

6. **Save the file.**

7. *(Optional but recommended)* Push the updated `.trex` back to GitHub so colleagues can download the correct version:

   ```bash
   git add auto-resize.trex
   git commit -m "Update .trex with actual GitHub Pages URL"
   git push
   ```

   Or edit the file directly on GitHub: click the file → pencil icon → edit → **Commit changes**.

---

### Step 5 — Verify Everything Works

Before opening Tableau, confirm the extension is accessible:

1. Open a web browser.
2. Go to: `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/auto-resize.html`
3. ✅ **You should see** the extension UI with:
   - Header: "📐 Auto-Resize Dashboard"
   - A status badge (it will say "Error" because it's not inside Tableau — that's normal)
   - Summary cards and an event log table

4. ❌ **If you see a 404 error:**
   - Wait another minute and try again (GitHub Pages can take up to 2 minutes).
   - Double-check that `auto-resize.html` is in the **root** of your repository (not in a subfolder).
   - Verify GitHub Pages is enabled and pointing to the correct branch.

---

### Step 6 — Load the Extension in Tableau

1. Open **Tableau Desktop** and open your workbook.
2. Navigate to a **Dashboard** tab.
3. In the left pane under **Objects**, find **Extension** and **drag it** onto the dashboard.
4. In the "Add an Extension" dialog:
   - Click **"My Extensions"**
   - Browse to where you saved `auto-resize.trex` on your computer
   - Select it and click **Open**
5. Tableau will show a security prompt — click **Allow**.
6. ✅ The extension panel should appear with a green **"Monitoring"** badge.

---

### Step 7 — Test It

1. **Add a new worksheet** to your dashboard → watch the event log record it and the dashboard height increase.
2. **Drag-resize a worksheet** to be taller → the dashboard height grows by the same amount.
3. Try the controls:
   - Toggle **Auto-extend enabled** off → resize a worksheet → the event is logged but the height doesn't change.
   - Click **Reset Baseline** → cumulative delta goes back to zero.

🎉 **You're all set!**

---

### Quick Reference — What to Customize

| Item | Where | What to Change |
|---|---|---|
| `YOUR-USERNAME` | `auto-resize.trex` → `<url>` tag | Replace with your GitHub username |
| `YOUR-REPO-NAME` | `auto-resize.trex` → `<url>` tag | Replace with your repository name |
| Poll interval | `auto-resize.html` → `POLL_INTERVAL` | Change `1500` (ms) to adjust responsiveness |
| Author info | `auto-resize.trex` → `<author>` tag | *(Optional)* Replace with your name/org |

---

### Sharing with Your Team

Once everything is working:

1. **Share the `.trex` file** (with the updated URL) with your colleagues.
2. They just need to drag an **Extension** object onto their dashboard and load the `.trex` file.
3. **No server setup required** — GitHub Pages serves the HTML for everyone.
4. If you update `auto-resize.html` and push to GitHub, everyone gets the latest version automatically.

---

### Folder Structure After Setup

```
Your GitHub Repository
├── auto-resize.html          ← Served by GitHub Pages (extension logic)
├── auto-resize.trex          ← Manifest with YOUR GitHub Pages URL
├── README.md                 ← Documentation
└── SETUP-INSTRUCTIONS.md     ← This file
```

---

### Need Help?

- **GitHub Pages docs**: [docs.github.com/en/pages](https://docs.github.com/en/pages)
- **Tableau Extensions API**: [tableau.github.io/extensions-api](https://tableau.github.io/extensions-api/)
- **Troubleshooting**: See the Troubleshooting section in [`README.md`](README.md)
