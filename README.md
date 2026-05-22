# 📐 Auto-Resize Dashboard — Tableau Extension

A Tableau Dashboard Extension that **automatically extends the dashboard height** whenever a worksheet is added or manually resized — keeping every other element (filters, legends, text boxes, images, floating items) exactly where it is.

---

### How It Works

| Trigger | What Happens |
|---|---|
| New worksheet added (e.g. 100 px tall) | Dashboard height grows by **+100 px** |
| Existing worksheet resized (100 → 300 px) | Dashboard height grows by the **delta (+200 px)** |
| Worksheet shrinks (300 → 200 px) | Dashboard height shrinks by **−100 px** |

The extension uses a dual-detection strategy — it listens for Tableau's `DashboardLayoutChanged` event **and** polls every 1.5 seconds as a safety net — so no resize is ever missed.

---

### What's in This Repository

```
├── auto-resize.html          ← Extension UI + all logic (self-contained)
├── auto-resize.trex          ← Tableau manifest (⚠️ needs your GitHub URL)
├── README.md                 ← This file
└── SETUP-INSTRUCTIONS.md     ← Step-by-step GitHub upload & setup guide
```

---

### Prerequisites

| Requirement | Details |
|---|---|
| **Tableau Desktop or Server** | Version **2018.2** or later (Extensions API v1.7+) |
| **GitHub account** | Free account is sufficient |
| **GitHub Pages** | Enabled on your repository (free for public repos) |

---

### 🚀 Quick Setup (5 Minutes)

> **For detailed step-by-step instructions with screenshots, see [`SETUP-INSTRUCTIONS.md`](SETUP-INSTRUCTIONS.md)**

#### Step 1 — Create a GitHub Repository

1. Go to [github.com/new](https://github.com/new).
2. Name your repository (e.g. `tableau-auto-resize`).
3. Set it to **Public** (required for free GitHub Pages).
4. Click **Create repository**.

#### Step 2 — Upload the Files

Upload **all files** from this folder to the root of your new repository:

- `auto-resize.html`
- `auto-resize.trex`
- `README.md`
- `SETUP-INSTRUCTIONS.md`

You can drag & drop them on the GitHub upload page, or use git:

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
cd YOUR-REPO-NAME
# Copy the files into this directory, then:
git add -A
git commit -m "Add Tableau auto-resize extension"
git push
```

#### Step 3 — Enable GitHub Pages

1. In your repository, go to **Settings** → **Pages** (left sidebar).
2. Under **Source**, select **Deploy from a branch**.
3. Choose the **`main`** branch and **`/ (root)`** folder.
4. Click **Save**.
5. Wait 1–2 minutes for GitHub to deploy. Your site will be live at:

```
https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/
```

#### Step 4 — ⚠️ Update the `.trex` File (REQUIRED)

**This is the most important step.** The `.trex` file tells Tableau where to find the extension. You **must** replace the placeholder URL with your actual GitHub Pages URL.

Open `auto-resize.trex` and find this line:

```xml
<url>https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/auto-resize.html</url>
```

**Replace it** with your actual values. For example, if:
- Your GitHub username is **`jsmith`**
- Your repository name is **`tableau-auto-resize`**

Then the line becomes:

```xml
<url>https://jsmith.github.io/tableau-auto-resize/auto-resize.html</url>
```

> 💡 **How to find your URL:** After enabling GitHub Pages (Step 3), go to **Settings → Pages**. GitHub shows your live site URL at the top. Just append `/auto-resize.html` to it.

**Save the file** and commit the change if you edited it on GitHub.

#### Step 5 — Verify GitHub Pages Is Working

Before loading into Tableau, confirm the HTML file is accessible:

1. Open your browser.
2. Navigate to `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/auto-resize.html`
3. You should see the extension UI with the header "📐 Auto-Resize Dashboard" and a status badge.

If you see a 404 error, wait another minute — GitHub Pages can take up to 2 minutes to deploy.

#### Step 6 — Load the Extension in Tableau

1. Open your workbook in **Tableau Desktop**.
2. Go to a **Dashboard** tab.
3. In the left pane under **Objects**, drag **Extension** onto the dashboard.
4. Click **"My Extensions"** → browse to your local copy of `auto-resize.trex` → **Open**.
5. Tableau will prompt you to **Allow** the extension — click **Allow**.
6. The extension panel appears inside your dashboard with a green **"Monitoring"** badge.

> 📌 **Note:** The `.trex` file is only used during initial loading — Tableau reads the URL from it and loads the HTML from GitHub Pages. You only need the `.trex` file locally on the machine where you open Tableau.

---

### Using the Extension

Once loaded, the extension immediately begins monitoring your dashboard:

#### Automatic Mode (default)

- **Add a worksheet** → dashboard height increases by the worksheet's height
- **Resize a worksheet** → dashboard height adjusts by the exact height delta
- **Remove a worksheet** → logged (dashboard height doesn't auto-shrink on removal)

#### Control Panel

| Control | What It Does |
|---|---|
| **Auto-extend enabled** (checkbox) | Toggle auto-resizing on/off. When off, events are still logged but the dashboard height is unchanged. |
| **↻ Refresh Now** | Manually trigger a size check |
| **Clear Log** | Wipe the event log |
| **Reset Baseline** | Re-capture the current dashboard height as the new baseline and zero out the cumulative delta |

#### Dashboard Info Cards

| Card | Shows |
|---|---|
| **Dashboard Height** | Current total height in pixels |
| **Worksheets** | Number of worksheets being tracked |
| **Total Added** | Cumulative pixels added/removed |
| **Events** | Total number of resize events detected |

---

### Configuration Options

#### Poll Interval

Inside `auto-resize.html`, the constant `POLL_INTERVAL` controls how often the extension checks for changes (default: 1500 ms):

```js
const POLL_INTERVAL = 1500;   // milliseconds
```

Lower values = more responsive, but slightly more CPU usage.

#### Dashboard Size Mode

For best results, set your Tableau dashboard size to **Automatic**:
- **Dashboard** menu → **Size** → **Automatic**

Fixed-size dashboards may not respond to programmatic height changes.

---

### Troubleshooting

#### ❌ Tableau says "Could not connect to the extension"

| Check | Fix |
|---|---|
| Is GitHub Pages enabled? | **Settings → Pages** — verify a URL is shown |
| Is the URL correct in `.trex`? | Open the URL in a browser — you should see the extension UI |
| Did you replace the placeholders? | The URL must NOT contain `YOUR-USERNAME` or `YOUR-REPO-NAME` |
| Is the repo public? | GitHub Pages requires a public repo on free plans |

#### ❌ Extension loads but shows "Error — see console"

1. Right-click the extension zone in Tableau → **Inspect**.
2. Check the **Console** tab for red errors.
3. Common cause: the Tableau Extensions API CDN is blocked by a firewall. Contact your IT team to allow `extensions.tableausoftware.com`.

#### ❌ Extension panel is blank / never loads

1. Visit the GitHub Pages URL directly in a browser — does the page load?
2. Check that `auto-resize.html` is in the **root** of the repository (not inside a subfolder).
3. Make sure the branch set in GitHub Pages matches the branch where files are uploaded.

#### ❌ Dashboard height doesn't visually change

- **Fixed-size dashboards** may ignore programmatic height changes. Switch to **Automatic** size mode.
- **Range-based sizing**: the extension's delta is constrained to the range minimum/maximum.

#### ❌ Events are not detected

- `DashboardLayoutChanged` fires only after a layout operation completes (e.g., when you release the mouse after a drag). The 1.5 s polling interval catches anything else.
- After adding many worksheets at once, click **↻ Refresh Now** to force a check.

#### ❌ 404 error on GitHub Pages URL

- GitHub Pages can take **1–2 minutes** to deploy after you push changes.
- Make sure **Settings → Pages** shows "Your site is live at …".
- Confirm the file name is exactly `auto-resize.html` (case-sensitive).

#### ❌ `.trex` file won't load in Tableau

- Open the `.trex` in a text editor and verify it's valid XML (no stray characters).
- Confirm the `<url>` value starts with `https://` (not `http://`).
- GitHub Pages always serves over HTTPS — Tableau requires the protocol to match.

---

### Sharing with Colleagues

Once your GitHub Pages site is live, anyone can use your extension:

1. **Share the `.trex` file** — colleagues download it and load it in Tableau via **Extension → My Extensions**.
2. **No server required** — the HTML is served from GitHub Pages, so no one needs to run a local server.
3. **Always up to date** — if you push changes to the HTML, everyone gets the latest version next time they open the dashboard.

---

### Technical Details

- **Tableau Extensions API v1.12** — loaded from the official CDN
- **Dual detection** — `DashboardLayoutChanged` event listener + polling fallback
- **State persistence** — cumulative delta stored in `tableau.extensions.settings` (survives page reloads)
- **Zero dependencies** — pure vanilla HTML + CSS + JS; no build step, no npm, no frameworks
- **Mixed layout support** — tracks both tiled and floating worksheets via `dashboard.objects`

---

### License

MIT — use freely in personal and commercial projects.
