# 慧盈财富 · Wise Win Financial Newsletters

This repository contains newsletter payload files. Dropping a new `.html` file into a newsletter folder automatically triggers an email send to all active subscribers.

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

---

## 🚀 How to Send a Newsletter

1. Create your newsletter as an HTML file
2. Name it with the date: `YYYY-MM-DD.html` (e.g. `2026-04-07.html`)
3. Drop it into the correct newsletter folder
4. Commit and push to `main`

```bash
git add weekly-financial-report/2026-04-07.html
git commit -m "Weekly report Apr 7 2026"
git push origin main
```

GitHub Actions will automatically:
- Detect the new file
- Derive the subject line from the filename and newsletter type
- Send the email to all active subscribers of that newsletter
- Log the result (sent count, failures)

---

## 📧 Subject Line Format

Subject lines are auto-generated from the filename:

| File | Newsletter Type | Subject |
|---|---|---|
| `2026-04-07.html` | Weekly Financial Report | `Weekly Financial Report — Apr 7, 2026` |
| `2026-04-04.html` | Market Forecast | `Market Forecast — Apr 4, 2026` |

---

## 📋 Newsletter Types

| Folder | Friendly Name | Cadence |
|---|---|---|
| `weekly-financial-report` | Weekly Financial Report | Every Monday |
| `market-forecast` | Market Forecast | Every Friday |

To add a new newsletter type, insert a row in the `newsletter_types` database table and create the corresponding folder here.

---

## ⚠️ Rules

- Files **must** be named `YYYY-MM-DD.html` — other formats are ignored
- Only `.html` files trigger sends
- Only push when ready — every push to `main` with a new file triggers a real send
- Test with a small subscriber list first
