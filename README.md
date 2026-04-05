# 慧盈财富 · Wise Win Financial — Newsletter Payloads

This repository contains newsletter HTML payload files. Pushing a new `.html` file to a newsletter folder automatically triggers delivery to all active subscribers via GitHub Actions.

🌐 **Subscription Site:** https://verceltest-umber.vercel.app  
⚙️ **App Repo:** https://github.com/yangfa1/verceltest

---

## 📁 Folder Structure

```
wisewin-newsletters/
├── weekly-financial-report/     ← Weekly Financial Report payloads
│   └── YYYY-MM-DD.html
├── market-forecast/             ← Market Forecast payloads
│   └── YYYY-MM-DD.html
└── README.md
```

Each folder name corresponds to a `folder_name` in the `newsletter_types` database table. To add a new newsletter type, create a new folder here **and** add the matching row in the admin panel at `/admin/newsletter-types`.

---

## 🚀 How to Send a Newsletter

1. Write your newsletter as an HTML file (see [Email Template Guide](#-email-template-guide) below)
2. Name it with the send date: `YYYY-MM-DD.html` (e.g. `2026-04-07.html`)
3. Drop it into the correct newsletter folder
4. Commit and push to `main`:

```bash
git add weekly-financial-report/2026-04-07.html
git commit -m "Weekly report Apr 7, 2026"
git push origin main
```

GitHub Actions will automatically:
- Detect the new `.html` file
- Auto-generate the subject line from the filename + newsletter type
- POST to the Vercel `/api/send-newsletter` endpoint
- Send the email to all active subscribers of that newsletter type
- Log the result (sent count, failures) in the Actions tab

---

## 📧 Subject Line Auto-Generation

Subject lines are derived from the filename and newsletter type:

| Folder | File | Subject Generated |
|---|---|---|
| `weekly-financial-report` | `2026-04-07.html` | `Weekly Financial Report — Apr 7, 2026` |
| `market-forecast` | `2026-04-04.html` | `Market Forecast — Apr 4, 2026` |

---

## 📄 Email Template Guide

### Required: Unsubscribe Link

Every newsletter **must** include an unsubscribe link. Use the placeholder `{{UNSUBSCRIBE_URL}}` in your HTML footer — it's automatically replaced with each subscriber's unique link:

```html
<a href="{{UNSUBSCRIBE_URL}}">Unsubscribe</a>
```

### Recommended HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Newsletter Title</title>
</head>
<body style="margin:0;padding:0;font-family:Inter,Arial,sans-serif;">
  <!-- Use inline styles — email clients don't support external CSS -->

  <!-- Header with Wise Win branding -->
  <!-- Body content -->
  <!-- Footer with unsubscribe link -->
    <a href="{{UNSUBSCRIBE_URL}}">Unsubscribe</a>
  <!-- End footer -->
</body>
</html>
```

> ⚠️ **Use inline CSS only.** Email clients strip external stylesheets and `<style>` tags. See the mock payloads for reference.

### Reference Files

The following files in this repo serve as templates:

| File | Description |
|---|---|
| `weekly-financial-report/2026-04-07.html` | Full weekly report template |
| `market-forecast/2026-04-04.html` | Full market forecast template |
| `market-forecast/2026-04-05.html` | Simple test payload example |

---

## 📋 Current Newsletter Types

| Folder | Friendly Name | Cadence |
|---|---|---|
| `weekly-financial-report` | Weekly Financial Report | Every Monday |
| `market-forecast` | Market Forecast | Every Friday |

To add a new newsletter type:
1. Log in to [admin panel](https://verceltest-umber.vercel.app/admin)
2. Go to **Newsletter Types** → **+ Add Newsletter**
3. Create the matching folder in this repo
4. Start dropping payloads!

---

## ⚠️ Important Rules

- Files **must** be named `YYYY-MM-DD.html` — other formats are silently ignored
- Only `.html` files in newsletter subfolders trigger sends
- Push only when ready — every push with a new `.html` file triggers a **real send to real subscribers**
- Check [GitHub Actions](https://github.com/yangfa1/wisewin-newsletters/actions) after each push to confirm delivery

---

## 🔍 Monitoring Sends

After pushing a new file:

1. Go to **github.com/yangfa1/wisewin-newsletters → Actions**
2. Click the latest **Send Newsletter** run
3. Expand the **Send newsletters** step
4. Check for `✅ Newsletter sent successfully` and the `{ sent, failed }` counts

If the action fails, the logs will show the API response with error details.
