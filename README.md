# 🚀 n8n Content Automation System

This project is an automated content creation and posting system built using **n8n**, **Google Sheets**, and social media integrations (e.g., LinkedIn, Facebook).

It allows you to:
- Generate content
- Approve or reject posts
- Post manually OR automatically (scheduled)
- Track posting status
- Maintain a clean workflow pipeline

---

# 🧠 System Overview

The system is divided into **two main branches**:

## 1. ✍️ Create Content Branch
- Triggered via **Webhook**
- Generates content (LinkedIn, Facebook, etc.)
- Saves content into Google Sheets
- Marks status as `draft`

## 2. 📤 Post Content Branch
- Triggered either:
  - Manually (via webhook / button)
  - Automatically (via schedule trigger)
- Filters approved posts
- Posts to selected platforms
- Updates status to `posted`

---

# 🔄 Workflow Architecture
[Webhook Trigger]
↓
[Switch Node]
/
Create Post
Branch Branch

Post Branch:
[Schedule Trigger / Manual Trigger]
↓
[Google Sheets - Get Rows]
↓
[IF Node (Filter Conditions)]
↓
[Post to Platform]
↓
[Update Sheet]


---

# 📊 Google Sheets Structure

Your sheet must include the following columns:

| Column Name     | Description |
|----------------|------------|
| `id`           | Unique post ID |
| `topic`        | Content topic |
| `linkedin_post`| Generated LinkedIn content |
| `facebook_post`| Generated Facebook content |
| `status`       | `draft` / `posted` |
| `approved`     | `yes` / `no` |
| `schedule_time`| Time for scheduled posting |
| `posted_at`    | Timestamp after posting |
| `image_url`    | Image link (optional) |

---

# ⚙️ Posting Logic

## ✅ A post will ONLY be published if:

- `approved = yes`
- `status != posted`
- AND (for scheduled posts):
  - `schedule_time <= current time`

---

# 🧩 Key Features

- ✅ Manual posting (select specific post ID)
- ✅ Scheduled automatic posting
- ✅ Approval system (prevents accidental posting)
- ✅ Multi-platform support
- ✅ Google Sheets as CMS
- ✅ Scalable workflow design

---

# ⚠️ Known Issues & Fixes

## Issue 1: Unapproved posts getting posted
**Fix:** Ensure IF node includes:
```

approved = yes

```

---

## Issue 2: Multiple posts being triggered
**Fix:** Add filter in Google Sheets node:
```

id = {{$json.id}}

```

---

## Issue 3: Schedule trigger runs but no posts
**Fix:**
- Check IF node conditions
- Ensure `schedule_time` is not empty
- Ensure time format is valid

---

# 🔐 Security Notes

- Do NOT expose API keys in workflows
- Remove credentials before pushing to GitHub
- Use environment variables where possible

---

# 📦 Installation & Setup

## 1. Import Workflow
- Open n8n
- Click **Import**
- Upload `.json` workflow file

## 2. Connect Services
- Google Sheets account
- Social media APIs (LinkedIn, etc.)
- Google Drive (for images)

## 3. Configure Sheet
- Create sheet with required columns
- Ensure correct data types

## 4. Activate Workflow
- Click **Publish** in n8n

---

# 🔁 Automation

## Schedule Trigger
- Runs every X minutes
- Checks for approved + scheduled posts

## Manual Trigger
- Uses webhook
- Posts a specific ID only

---

# 🚀 Future Improvements

- Queue system (1 post at a time)
- Retry logic for failed posts
- Analytics tracking
- AI content optimization
- Real-time GitHub backup

---

# 👨‍💻 Author

Built using:
- n8n
- Google Sheets
- Automation best practices

---

# 📄 License

MIT License — feel free to use and modify.

---

# ⭐ Final Notes

This system is designed to be:
- Simple to manage
- Flexible to scale
- Safe from accidental posting

If something breaks, always check:
1. IF node conditions
2. Google Sheets data
3. Trigger configuration

