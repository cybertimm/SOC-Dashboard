# Meridian SOC — SIEM Dashboard

A fully self-contained, browser-based Security Operations Center (SOC) dashboard built with React, D3.js, and Babel. No server, no build step, no dependencies to install — open the HTML file and go.

Built around a realistic simulated incident: **Operation Credential Harvest** — a phishing-to-ransomware-precursor attack chain spanning 7 days across a fictional enterprise network.

---

## Features

| Area | What's included |
|---|---|
| **Overview** | Live UTC clock, event/alert/login stat cards with sparklines, login activity bar chart, alerts-by-severity donut, top failing IPs, critical alert feed |
| **Alerts** | 8 pre-loaded alerts (Critical/High), severity filter tabs, per-alert remediation steps, analyst notes, status workflow (New → Investigating → Escalated → Closed) |
| **Events** | Full SIEM-style log table, Splunk-style search bar, event-type filter, click-to-expand rows with full field detail, malicious-row highlighting |
| **Investigation** | Kill chain progress bar, attack timeline, SVG network topology diagram, IOC list with clipboard export, affected systems panel, interactive containment checklist with progress bar, threat actor profile, case notes |
| **ATT&CK** | 12 MITRE tactics, 44 techniques, Cards view + Heatmap/Navigator view, detection coverage meter, click-through technique detail panel (description, log evidence, detection methods, remediation) |

---

## Quick Start

```bash
# Clone the repo
git clone https://github.com/your-username/meridian-soc-dashboard.git

# Open in browser — no build step needed
start "SOC dashboard.html"        # Windows
open "SOC dashboard.html"         # macOS
xdg-open "SOC dashboard.html"    # Linux
```

> **Requirements:** A modern browser (Chrome, Edge, Firefox, Safari). No Node.js, no npm, no server required.

---

## Incident Scenario

The dashboard simulates **INC-2024-0304** — a real-world attack pattern observed against enterprise targets.

### Attack Chain

```
Mar 4  10:42  Phishing emails from merid1anfg.com → 3 employees     T1566.002
Mar 4  10:56  jsmith clicks link → credentials submitted to fake portal  T1056.003
Mar 5  02:14  RDP brute-force from Tor exit node → DC-01 (success)  T1110.001
Mar 6  08:47  Impossible travel: Chicago login + Moscow VPN 32m later T1078
Mar 7  01:15  Lateral movement: SMB C$ + ADMIN$ + SSH (01:15–01:20 AM) T1021.002
Mar 7  01:22  Encoded PowerShell: Invoke-WebRequest (base64)         T1059.001
Mar 7  01:25  C2 beacon via pastebin.com HTTP POST                   T1071.001
Mar 8  02:00  Dual persistence: fake WindowsUpdate task + HKCU Run key T1053.005
Mar 8–9 22:00 Data exfiltration: 45MB + 62MB to transfer.sh          T1567.002
Mar 10 03:00  VSS shadow deletion + Security/System log wipe          T1070.001
```

**Kill chain status:** 7/8 stages complete — Impact stage (ransomware) not yet executed.

**TTP pattern:** Matches Conti / BlackBasta pre-encryption playbook.

---

## MITRE ATT&CK Coverage

44 techniques mapped across 12 tactics. Each technique includes detection status, log evidence, detection methods, and remediation steps.

| Tactic | ID | Techniques | Detected |
|---|---|---|---|
| Initial Access | TA0001 | Spearphishing Link, Valid Accounts, Exploit Public-Facing App, Spearphishing Attachment | 2/4 |
| Execution | TA0002 | PowerShell, Windows Command Shell, Malicious File, Service Execution | 2/4 |
| Persistence | TA0003 | Scheduled Task, Registry Run Keys, Create Local Account | 2/3 |
| Privilege Escalation | TA0004 | Domain Accounts, Access Token Manipulation, Process Injection | 1/3 |
| Defense Evasion | TA0005 | Clear Event Logs, VSS Deletion, Obfuscated Files, Disable Security Tools, Match Legitimate Name | 4/5 |
| Credential Access | TA0006 | Password Guessing, Web Portal Capture, OS Credential Dumping, Password Store Access | 2/4 |
| Discovery | TA0007 | Remote System Discovery, File & Directory Discovery, System Network Config, Process Discovery | 1/4 |
| Lateral Movement | TA0008 | SMB Admin Shares, RDP, SSH, Pass the Hash | 3/4 |
| Collection | TA0009 | Data from Local System, Data from Network Share, Data Staged, Screen Capture | 3/4 |
| Command & Control | TA0011 | Web Protocols, Ingress Tool Transfer, Encrypted Channel, Remote Access Software | 1/4 |
| Exfiltration | TA0010 | Exfil via Cloud Storage, Exfil over Alt Protocol, Exfil over C2 Channel | 1/3 |
| Impact | TA0040 | Inhibit System Recovery, Data Destruction, Data Encrypted for Impact | 1/3 |

**Detection statuses:**
- 🟢 **Detected** — confirmed evidence in log data
- 🟡 **Suspected** — circumstantial evidence, requires investigation
- ⬛ **Not Seen** — not observed in this incident

---

## Dashboard Tabs

### Overview
- 4 stat cards (Total Events, Active Alerts, Failed Logins, Unique Users) each with a 7-day sparkline
- D3 stacked bar chart — login success vs. failure by day
- Donut chart — alert severity distribution + MITRE coverage %
- Top 5 failing source IPs with proportional bar chart
- Critical alert feed — click to jump to alert detail

### Alerts
- 8 alerts loaded: 5 Critical, 3 High
- Filter by severity: All / Critical / High / Medium
- Left-border color coding by severity
- Status badges: New / Investigating / Escalated / Closed
- Detail panel (click any alert):
  - Description in monospace block
  - Source IP, User, MITRE ID, Timestamp metadata grid
  - Numbered remediation steps
  - Analyst notes textarea (per-alert, persists during session)
  - Action buttons to update status

### Events
- 41 log entries spanning Mar 4–10, 2024
- Splunk-style search: `index=* | search user=jsmith` strips Splunk syntax automatically
- Event type filter dropdown (login, email, web_browse, process_create, smb_access, etc.)
- Malicious rows highlighted in dark red
- Click any row to expand — shows all fields as key-value pairs + attack type badge

### Investigation
- **Kill Chain Progress** — 8-stage Lockheed Martin model, color-coded by completion
- **Attack Timeline** — 10 events with MITRE IDs on a vertical timeline
- **Network Topology** — SVG diagram showing full attack path from Tor node through pivot point to exfil
- **Threat Actor Profile** — campaign name, actor type, infrastructure, TTP pattern match
- **IOC List** — 10 indicators with COPY IOCs button (copies to clipboard as `[Type] value`)
- **Affected Systems** — 5 hosts with severity status, role description, and action tags
- **Containment Checklist** — 14 items, interactive checkboxes with progress bar
- **Case Notes** — freeform textarea with `● saved` indicator

### ATT&CK
- **Cards view** — 12 tactic cards, each listing techniques with:
  - Colored status dot (green/amber/gray)
  - Technique ID + severity badge
  - Evidence preview from log data
  - Click to open detail panel
- **Heatmap view** — ATT&CK Navigator-style compact grid, scrollable, click any cell
- **Coverage bar** — shows detected % with green/amber split
- **Technique detail panel** (right side, slides in):
  - Status badge + severity + tactic context
  - Description
  - Log evidence (highlighted in amber)
  - Detection methods
  - Remediation steps (semicolon-delimited, rendered as checklist)

---

## Indicators of Compromise (Simulated)

| Type | Value | Context |
|---|---|---|
| Domain | `merid1anfg.com` | Typosquatted phishing domain |
| Domain | `merid1anfg-benefits.com` | Credential harvesting portal |
| IP | `185.220.101.42` | Tor exit node — RDP brute force |
| IP | `91.234.56.78` | Moscow, RU — impossible travel VPN |
| IP | `198.51.100.50` | Exfiltration destination |
| Email | `hr-updates@merid1anfg.com` | Phishing sender |
| File | `C:\temp\update.ps1` | Malicious PowerShell persistence script |
| Task | `WindowsUpdate` (fake) | Scheduled task persistence |
| RegKey | `HKCU\...\Run: update` | Registry Run key persistence |
| Account | `jsmith` | Compromised domain user |

---

## Technical Stack

| Component | Library | Source |
|---|---|---|
| UI framework | React 18.2 | CDN (cdnjs) |
| JSX transform | Babel Standalone 7.23.9 | CDN (cdnjs) |
| Charts | D3.js 7.8.5 | CDN (cdnjs) |
| Fonts | JetBrains Mono, DM Sans | Google Fonts |

No bundler, no package manager, no backend. All state is in-memory (React hooks). The file is fully self-contained — save it and open it anywhere.

---

## File Structure

```
.
└── SOC dashboard.html    # Single-file application (~1,200 lines)
    ├── <style>           # CSS — dark theme, animations, scrollbars
    └── <script type="text/babel">
        ├── Constants     # SEV colors, STCOL detection colors
        ├── LOGS[]        # 41 simulated log entries
        ├── ALERTS[]      # 8 alerts with remediation steps
        ├── MITRE_MATRIX[]# 12 tactics × 44 techniques
        ├── CONTAINMENT[] # 14 containment action items
        ├── I{}           # SVG icon library (inline)
        ├── TimeChart     # D3 stacked bar chart
        ├── MiniBar       # D3 sparkline bars
        ├── DonutChart    # D3 donut chart
        ├── AttackTimeline# Vertical event timeline
        ├── TechDetailPanel # MITRE technique slide-in panel
        ├── MitreHeatmapView # ATT&CK Navigator-style grid
        └── App           # Main application component
```

---

## Customisation

**Add your own log entries** — extend the `LOGS` array at the top of the script:
```javascript
{
  t:"2024-03-11 09:00:00",
  src:"10.0.0.200",
  dst:"10.0.0.10",
  user:"newuser",
  host:"WS-NEW-001",
  type:"login",
  status:"success",
  detail:"Interactive logon",
  mal:false
}
```

**Add an alert** — extend `ALERTS[]` with a `fix` array of remediation strings:
```javascript
{
  id:"ALT-009",
  sev:"high",
  title:"Your Alert Title",
  time:"2024-03-11 09:05:00",
  src:"10.0.0.200",
  user:"newuser",
  status:"new",
  mitre:"T1078",
  desc:"Description of the alert...",
  fix:["Step 1 remediation","Step 2 remediation"]
}
```

**Add a MITRE technique** — find the relevant tactic in `MITRE_MATRIX` and append to its `techniques` array:
```javascript
{
  id:"T1XXX",
  name:"Technique Name",
  status:"detected",   // "detected" | "suspected" | "not_observed"
  sev:"high",          // "critical" | "high" | "medium" | "low"
  desc:"What happened...",
  evidence:"Log entry evidence or null",
  detect:"How to detect this technique",
  fix:"Step 1; Step 2; Step 3"   // semicolon-separated
}
```

---

## Use Cases

- **SOC analyst training** — walk through a realistic multi-stage incident
- **MITRE ATT&CK workshops** — demonstrate tactic/technique mapping to log evidence
- **IR tabletop exercises** — use the containment checklist and case notes during drills
- **Interview portfolio** — demonstrate SOC/blue team tooling knowledge
- **Security awareness** — show non-technical stakeholders what an attack looks like end-to-end

---

## Limitations

- **Data is static** — no live log ingestion or real-time alerts
- **State is not persisted** — analyst notes, checklist items, and alert statuses reset on page reload
- **Single incident** — the dashboard is scoped to INC-2024-0304; extending it requires editing the JS data arrays
- **No authentication** — this is a demo/training tool, not a production deployment

---

## License

MIT — free to use, modify, and redistribute. Attribution appreciated but not required.
