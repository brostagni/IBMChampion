# 🏆 IBM Champion Activity Reporter — Bob Agent

> Automatically fill in your [IBM Champions advocacy reporting form](https://airtable.com/appuwf3eOGdO6x1oS/pagF5IfVT7m6unCbG/form) in seconds using **Bob**, IBM's AI assistant.

Simply paste a link to your advocacy act (LinkedIn post, blog, talk, video…) and Bob generates a complete, copy-paste-ready report — correctly classified, within the 250-word limit, and aligned with the IBM Champions acceptance criteria. Bob can also **fill the Airtable form automatically** using Puppeteer — you just click Submit.

---

## Prerequisites

Before using the auto-fill feature, make sure the following are in place.

### 1. Node.js (v18+)

```bash
node --version   # must return v18.x or higher
```

If not installed → [nodejs.org](https://nodejs.org)

---

### 2. Chrome for Testing (required by Puppeteer)

```bash
npx puppeteer browsers install chrome
```

Verify the binary was installed:
```bash
ls ~/.cache/puppeteer/chrome/
# You should see a folder like: mac_arm-148.x.x.x  (or mac-148.x.x.x on Intel)
```

---

### 3. MCP `fetch` configured in Bob — pinned version + Chrome path

Open (or create) `~/.bob/settings/mcp.json` and add the `fetch` entry:

```json
{
  "mcpServers": {
    "fetch": {
      "command": "/opt/homebrew/bin/npx",
      "args": ["-y", "mcp-fetch@0.1.6"],
      "env": {
        "PUPPETEER_EXECUTABLE_PATH": "/Users/YOUR_USERNAME/.cache/puppeteer/chrome/mac_arm-148.0.7778.97/chrome-mac-arm64/Google Chrome for Testing.app/Contents/MacOS/Google Chrome for Testing"
      }
    }
  }
}
```

> ⚠️ **Two things are mandatory here:**
> - **Pinned version `mcp-fetch@0.1.6`** — without it, a newer version may be downloaded that expects a different Chrome binary.
> - **`PUPPETEER_EXECUTABLE_PATH`** — without it, Puppeteer looks for a Chrome it cannot find and fails silently.

To find your exact Chrome path after installation:
```bash
ls ~/.cache/puppeteer/chrome/
# Copy the exact folder name, e.g. mac_arm-148.0.7778.97
# Then build the full path:
# ~/.cache/puppeteer/chrome/<FOLDER>/chrome-mac-arm64/Google Chrome for Testing.app/Contents/MacOS/Google Chrome for Testing
```

On **Intel Mac**, replace `mac_arm-` with `mac-` and `chrome-mac-arm64` with `chrome-mac`.

On **Windows**, the path will be under `%USERPROFILE%\.cache\puppeteer\chrome\`.

---

### 4. Bob reloaded after any `mcp.json` change

Every time you modify `mcp.json`, you must reload Bob for the new MCP process to start with the correct environment variables.

In Bob: `Cmd+Shift+P` → **Reload Window** (macOS) or `Ctrl+Shift+P` → **Reload Window** (Windows/Linux).

---

### Quick checklist

| Prerequisite | Verification command |
|---|---|
| Node.js v18+ | `node --version` |
| Chrome for Testing installed | `ls ~/.cache/puppeteer/chrome/` |
| MCP fetch configured | read `~/.bob/settings/mcp.json` |
| `AGENTS.md` personalised | read `AGENTS.md` at workspace root |
| Bob reloaded | `Cmd+Shift+P` → Reload Window |

---

## How it works — 6 steps

### Step 1 — Get access to Bob

Bob is IBM's AI assistant, available at **[bob.ibm.com](https://bob.ibm.com)**.

- IBM employees: log in with your IBM SSO credentials.
- External IBM Champions: request access through your IBM Champion program contact or via the [IBM Champions portal](https://ibm.com/ibmchampion).

---

### Step 2 — Install Bob on your computer *(recommended)*

Installing the Bob desktop app lets you open local folders as workspaces, which is required for Steps 3–5.

1. Go to **[bob.ibm.com](https://bob.ibm.com)** and click **Download** (macOS, Windows, or Linux).
2. Install and launch the app.
3. Sign in with the same IBM credentials.

> You can also use Bob in the browser — but you will need to paste the `AGENTS.md` content manually.

---

### Step 3 — Create your IBMChampion folder

Create a dedicated folder inside Bob's playground directory:

```
~/Documents/Bob/Playground/IBMChampion/
```

> The exact path of your playground may differ. In Bob desktop, check **Settings → Workspace**.

**On macOS / Linux:**
```bash
mkdir -p ~/Documents/Bob/Playground/IBMChampion
```

**On Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Path "$HOME\Documents\Bob\Playground\IBMChampion" -Force
```

---

### Step 4 — Place the AGENTS.md file in your folder

1. Download [`AGENTS.md`](./AGENTS.md) from this repository.
2. Open the file in any text editor.
3. **Replace the placeholder values** in the `Identity` section at the top with your real information:

```markdown
- **First Name**: YOUR_FIRST_NAME          → your real first name
- **Last Name**: YOUR_LAST_NAME            → your real last name
- **Champion Program ID**: YOUR_CHAMPION_ID → e.g. C1234567
- **Primary Email**: your.email@company.com → your IBM Champion registered email
- **Alternate Email**: ...                  → optional second email
- **Champion Categories**: ...             → e.g. AI, Data, Security
- **Country**: YOUR_COUNTRY                → e.g. France
```

4. Save the file as `AGENTS.md` inside your `IBMChampion/` folder.

> ⚠️ Do **not** rename the file — Bob specifically reads `AGENTS.md` to load agent instructions automatically.

---

### Step 5 — Open the IBMChampion folder in Bob

1. Launch **Bob** (desktop app or browser).
2. Click **Open Folder** (or **File → Open Workspace**).
3. Select your `IBMChampion/` folder.

Bob will automatically detect the `AGENTS.md` file and load the IBM Champion Activity Reporter agent — no further configuration needed.

---

### Step 6 — Start a new conversation and submit your advocacy link

1. In Bob, click **New Conversation** (or **New Task**).
2. Paste your advocacy act link (or the full text of your post/article/talk), then add:

```
Here is my advocacy act. Please analyze it and fill out my IBM Champion activity report.
```

Bob will:
- Fetch the page and detect the publication date automatically
- Classify the activity type from the official IBM Champions list
- Write a 250-word English description aligned with acceptance criteria
- Output a complete, copy-paste-ready report
- **Offer to fill the Airtable form automatically** — reply "yes, fill the form" and Bob opens the browser, fills every field, and asks you to click Submit

---

## Example prompt

```
Here is my advocacy act. Please analyze it and fill out my IBM Champion activity report.

https://www.linkedin.com/pulse/how-i-use-ibm-watsonx-automate-my-workflows-jane-doe/
```

---

## What Bob generates

```
=== IBM CHAMPION ACTIVITY REPORT ===

Champion Program ID  : C1234567
First Name           : Jane
Last Name            : DOE
Primary Email        : jane.doe@company.com
Alternate Email      : jane@personal.com

Activity Type        : LinkedIn Post with carousel, video, or 250+ words
Product(s) Involved  : IBM watsonx
Description (EN)     : [250-word English description...]
Link / URL           : https://www.linkedin.com/pulse/...
IBM Can Amplify?     : Yes
Date of Activity     : 2025-06-15

--- REVIEWER NOTES (not submitted) ---
Activity tier        : Extended contribution
Strength signals     : Original technical insight, community value, expertise demonstrated
Watch out            : Ensure post is publicly accessible
Suggested pairing    : Submit a related blog on IBM community
Word count           : 198 / 250
```

Then Bob offers:
> 🤖 **Would you like me to fill in the form automatically?**

---

## Supported activity types

Bob classifies all official IBM Champions activity types, including:

- LinkedIn posts, articles, and reposts
- Blog posts (IBM property or external)
- YouTube and other videos
- Conference talks and webinars
- Podcast appearances (host or guest)
- Community forum contributions
- Product reviews (G2, Gartner Peer Insights…)
- Open source contributions
- Mentoring and coaching
- Case studies, books, Redbooks
- User group events and volunteering
- …and more (see [AGENTS.md](./AGENTS.md) for the full list)

---

## Repository structure

```
IBMChampion/
├── AGENTS.md     ← Bob agent instructions (personalise with your data)
└── README.md     ← This file
```

---

## Contributing

Found a bug or want to improve the agent prompt? Open an issue or submit a pull request — contributions are welcome.

---

## License

MIT — free to use, adapt, and share.
