# Meridian SOC — SIEM Simulation Dashboard

A fully interactive, browser-based SOC (Security Operations Center) simulation dashboard that mimics Splunk's interface. Built for cybersecurity training — no Splunk installation required.

## Features

- **Overview Dashboard** — Real-time stats, login activity charts, severity donut, top attacking IPs
- **Alert Queue** — 8 pre-populated alerts with triage workflow (Investigating / Escalated / Closed)
- **Event Log** — 65+ realistic log entries with Splunk-style search and filtering
- **Investigation Workspace** — Full attack timeline, IOCs, and affected systems map
- **MITRE ATT&CK Mapping** — Visual technique mapping across 8 tactics

## 🚀 Deploy to GitHub Pages (Step-by-Step)

### Step 1: Create a GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Name it something like `soc-dashboard`
3. Set it to **Public**
4. Check **"Add a README file"**
5. Click **Create repository**

### Step 2: Upload the File

1. In your new repo, click **"Add file"** → **"Upload files"**
2. Drag and drop the `index.html` file (downloaded from this conversation)
3. Write a commit message like `Add SOC dashboard`
4. Click **"Commit changes"**

### Step 3: Enable GitHub Pages

1. Go to your repo's **Settings** tab
2. In the left sidebar, click **Pages**
3. Under **"Source"**, select **"Deploy from a branch"**
4. Under **"Branch"**, select **main** and **/ (root)**
5. Click **Save**

### Step 4: Access Your Dashboard

After 1–2 minutes, your dashboard will be live at:

```
https://YOUR-USERNAME.github.io/soc-dashboard/
```

Replace `YOUR-USERNAME` with your actual GitHub username.

You can find the exact URL on the **Settings → Pages** screen after deployment completes.

---

## 🖥️ Alternative: Run Locally

Just open `index.html` in any modern browser. No server needed.

```bash
# Or use a simple server
python3 -m http.server 8080
# Then open http://localhost:8080
```

## 📋 What's Inside

The dashboard simulates a 7-day attack campaign ("Operation Credential Harvest") against a fictional company, Meridian Financial Group. The dataset includes:

| Attack Phase | Day | Technique |
|---|---|---|
| Phishing delivery | Day 1 | Typosquatted domain (merid1anfg.com) |
| Credential harvesting | Day 1 | Fake benefits portal |
| Brute-force RDP | Day 2 | From Tor exit node |
| Impossible travel | Day 3 | Chicago → Moscow in 32 min |
| Lateral movement | Day 4 | SMB admin shares + SSH |
| PowerShell execution | Day 4 | Encoded Invoke-WebRequest |
| Persistence | Day 5 | Fake scheduled task |
| Data exfiltration | Day 5–6 | transfer.sh uploads (45MB + 62MB) |
| Anti-forensics | Day 7 | Shadow copy deletion + log clearing |

## License

MIT — Free to use for training and education.
