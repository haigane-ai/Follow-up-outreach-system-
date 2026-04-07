# 🤖 n8n AI-Powered Cold Outreach System

> End-to-end automated outreach system that sends personalized cold emails, follows up automatically over 10 days, detects replies, and updates a live CRM — zero manual input after setup.

Built for Canadian solar installers. Deployable on Railway.

---

## 📌 What This Does

This system automates the entire cold outreach process from first contact to follow-up close:

1. **Picks up a new lead** from Google Sheets
2. **Waits a random 8–20 minutes** to simulate human sending behaviour
3. **Sends a personalized first email** via Gmail
4. **Updates the CRM** (Google Sheets) with thread ID, message ID, and sent date
5. **Follows up automatically** at Day 3, Day 7, and Day 10 in the same thread
6. **Detects replies** — classifies as real reply, no reply, or auto-reply
7. **Sends a notification** to your Gmail when a real reply is detected

---

## 🏗️ Architecture

```
FINAL SOLAR SHEET (Google Sheets)
        ↓
Outreach System (n8n)
├── Schedule Trigger (every 30 min, Mon–Fri 8am–5pm)
├── Get row(s) in sheet → filter: done=false, next_pitch=empty
├── Code node → random wait (8–20 min)
├── Wait node
├── Send email via Gmail
└── Update sheet: done=true, next_pitch=Sent, sent_date=now

Follow-up Workflow (n8n)
├── Schedule Trigger (every 30 min, Mon–Fri 8am–5pm)
├── Branch 1 → Follow-up 1 (Day 3)
├── Branch 2 → Follow-up 2 (Day 7)
└── Branch 3 → Follow-up 3 (Day 10)

Notification Automation (n8n)
├── Schedule Trigger (every 2 hours)
├── Loop through sent leads
├── Get Gmail thread
├── Classify reply (real / no reply / auto-reply)
└── Send notification email if real reply detected
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **n8n** | Workflow automation engine |
| **Google Sheets** | CRM and lead database |
| **Gmail API** | Send emails and detect replies |
| **OpenAI GPT-4** | Reply classification |
| **Railway** | Cloud hosting for n8n |

---

## 📁 Repository Structure

```
n8n-solar-outreach-system/
├── README.md
├── workflows/
│   ├── outreach-system.json         # Main outreach workflow
│   ├── followup-workflow.json       # Follow-up sequences
│   └── notification-automation.json # Reply detection
└── docs/
    └── email-templates.md           # Cold email copy
```

---

## 📧 Email Sequence

| Day | Email | Subject |
|---|---|---|
| Day 1 | First contact | `Your Quote Form` |
| Day 3 | Follow-up 1 | Same thread |
| Day 7 | Follow-up 2 | Same thread |
| Day 10 | Follow-up 3 | Same thread (binary close) |

---

## ⚙️ Google Sheet Columns

| Column | Description |
|---|---|
| `First Name` | Lead first name |
| `Email` | Lead email address |
| `Company` | Company name |
| `website` | Company website |
| `done` | TRUE after first email sent |
| `next_pitch` | Set to "Sent" after first email |
| `sent_date` | Timestamp of first email |
| `thread_ID` | Gmail thread ID for reply tracking |
| `message_ID` | Gmail message ID |
| `follow_up_1` | Set to "Sent" after Day 3 follow-up |
| `follow_up_2` | Set to "Sent" after Day 7 follow-up |
| `follow_up_3` | Set to "Sent" after Day 10 follow-up |
| `issues` | Bounced / Not interested flags |
| `Checked reply` | TRUE after reply check |

---

## 🚀 How to Import and Configure

### 1. Import Workflows
- Open your n8n instance
- Go to **Workflows** → **Import from file**
- Import each JSON file from the `workflows/` folder

### 2. Set Up Credentials
- **Google Sheets** → OAuth2 with your Google account
- **Gmail** → OAuth2 with your sending Gmail account

### 3. Configure Your Sheet
- Create a Google Sheet with the columns listed above
- Add your leads with `done = FALSE` and `next_pitch` empty

### 4. Update Node References
Replace these placeholders in each workflow:
```
YOUR_SHEET_ID     → Your Google Sheet ID
YOUR_GMAIL        → Your Gmail address
```

### 5. Activate Workflows
- Activate **Outreach System** first
- Then activate **Follow-up Workflow**
- Then activate **Notification Automation**

---

## ⚠️ Important Notes

- **Anti-spam**: One email per lead per day maximum
- **Business hours only**: Cron set to Mon–Fri 8am–5pm
- **Random delay**: 8–20 minute wait between sends to avoid spam filters
- **Remove credentials**: Never commit API keys or OAuth tokens to GitHub

---

## 📊 Results

- 60+ Canadian solar installers contacted
- Multi-niche expansion: Dental, HVAC, Medical, Physio
- Deployed on Railway — runs 24/7 without manual intervention

---

## 👤 Author

**Yassine Haigane** — AI Automation Specialist  
🌐 [yassine-automations.com](https://yassine-automations.com)  
💼 [github.com/haigane-ai](https://github.com/haigane-ai)

---

## 📄 License

MIT License — free to use and modify with attribution.
