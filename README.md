# 📬 Daily GitHub Commit Digest (n8n Workflow)

Automated daily summary of GitHub main branch commits, delivered to Slack every morning. 4 nodes. Zero manual effort.

## 🚀 What It Does

Runs at 11 AM daily and posts a clean commit digest to Slack showing exactly what went into the main branch yesterday — filtered, deduplicated, and noise-free.

**Sample Slack output:**
```
📊 Daily GitHub Update
🗓️ 2025-04-02 – main branch

- Jackie: feat: added wallet connect flow
- Arun: fix: token expiry edge case
- Priya: refactor: cleanup prediction API response
```

## ⚙️ Workflow Overview

```
Schedule Trigger (11 AM)
  → HTTP Request (GitHub REST API – main branch commits)
  → Code in JavaScript (filter + deduplicate + format)
  → Slack (post digest to channel)
```

## ✅ Features

- **Yesterday-only window** — UTC-safe time calculation, always accurate
- **Branch noise filtering** — skips merge commits from stg, qa, dev branches
- **Deduplication** — no commit appears twice
- **First-line only** — multi-line commit messages are trimmed cleanly
- **Fallback message** — posts "No meaningful updates" if nothing qualifies
- **Daily active** — used by a real engineering team in production

## 🧩 Node Details

| Node | Purpose |
|------|---------|
| Schedule Trigger | Fires at 11:00 AM daily |
| HTTP Request | GET /repos/{owner}/{repo}/commits?sha=main |
| Code in JavaScript | Time filter, branch filter, dedup, format |
| Send a message | Posts formatted digest to Slack DM or channel |

## 🛠️ Setup

### Requirements
- n8n instance
- GitHub Personal Access Token (repo read scope)
- Slack Bot Token with `chat:write` permission

### Configuration
Update these values in the workflow:

| Variable | Where |
|----------|-------|
| `GITHUB_PAT` | HTTP Request → Authorization header |
| `REPO_URL` | HTTP Request → URL (replace owner/repo) |
| `SLACK_CHANNEL_ID` | Slack node → Channel ID |
| `TRIGGER_HOUR` | Schedule Trigger → triggerAtHour |

### Filtering Customization
To change which merge branches are ignored, edit the `IGNORE_BRANCHES` array in the Code node:
```javascript
const IGNORE_BRANCHES = ['stg', 'qa', 'dev'];
```

## 📁 Files

```
n8n_dailyGithubDigest/
├── workflow.json       # Import directly into n8n
├── .env.example        # Required environment variables
└── README.md
```

## 🏁 Impact

- ✅ Team uses this daily in production
- 📉 Eliminated manual GitHub checking before standups
- 🧠 Surfaces yesterday's work automatically at the right time
- ⚡ 4 nodes — lightweight, fast, reliable
