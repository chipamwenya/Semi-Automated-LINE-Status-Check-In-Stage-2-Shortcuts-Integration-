# 📲 Semi-Automated LINE Status Check-In  
### Stage 2: iOS Shortcuts Logic for Dynamic Status Updates

This repository documents the iOS Shortcuts setup that powers a semi-automated status update system via LINE and logs/upgrades user data in Google Sheets through Make.com.

---

## 🚀 Overview

This stage introduces logic that enables two-way interaction:
- `I'm out` creates a new log entry
- `I'm back` updates the existing row based on sender match

This solution runs entirely on iOS using Shortcuts, requiring no external apps except LINE and Make.com webhooks.

---

## 🧠 Core iOS Shortcut Logic

### ✅ Global Variable Initialization (Top of Shortcut)

- `Current Date` → formatted as `yyyy-MM-dd HH:mm`
- `Current Time` → formatted with date set to "None", time format set to "Short"
- `Battery Level` → % retrieved from device
- `Location` → map coordinates + Apple Maps URL

All variables are initialized before menu logic to ensure global availability.

---

## 🎛 Menu-Based Conditional Logic (No `If` Statements!)

### `"Choose from Menu"` Prompt
- Label: *"What’s your status?"*
- Options: `"I'm out"` and `"I'm back"`

Each option acts like a conditional branch:

### 🟢 `"I'm out"` Block

- Pre-filled message includes timestamp, location, and battery level
- Message encoded using `URL Encode [Text]` and opened in LINE using `line://msg/text/`
- `Get Contents of URL` posts a structured JSON to Make.com:
```json
{
  "timestamp": "{{formatted date}}",
  "sender": "Chipa",
  "battery": "{{battery status}}",
  "location": "{{Maps URL}}",
  "message": "{{Text}}",
  "status": "I'm out"
}

---

# 📲 Semi-Automated LINE Status Check-In  
### Stage 2: iOS Shortcuts Logic for Dynamic Status Updates

This repository documents the iOS Shortcuts setup that powers a semi-automated status update system via LINE and logs/upgrades user data in Google Sheets through Make.com.

---

## 🚀 Overview

This stage introduces logic that enables two-way interaction:
- `I'm out` creates a new log entry
- `I'm back` updates the existing row based on sender match

This solution runs entirely on iOS using Shortcuts, requiring no external apps except LINE and Make.com webhooks.

---

## 🧠 Core iOS Shortcut Logic

### ✅ Global Variable Initialization (Top of Shortcut)

- `Current Date` → formatted as `yyyy-MM-dd HH:mm`
- `Current Time` → formatted with date set to "None", time format set to "Short"
- `Battery Level` → % retrieved from device
- `Location` → map coordinates + Apple Maps URL

All variables are initialized before menu logic to ensure global availability.

---

## 🎛 Menu-Based Conditional Logic (No `If` Statements!)

### `"Choose from Menu"` Prompt
- Label: *"What’s your status?"*
- Options: `"I'm out"` and `"I'm back"`

Each option acts like a conditional branch:

### 🟢 `"I'm out"` Block

- Pre-filled message includes timestamp, location, and battery level
- Message encoded using `URL Encode [Text]` and opened in LINE using `line://msg/text/`
- `Get Contents of URL` posts a structured JSON to Make.com:
```json
{
  "timestamp": "{{formatted date}}",
  "sender": "Chipa",
  "battery": "{{battery status}}",
  "location": "{{Maps URL}}",
  "message": "{{Text}}",
  "status": "I'm out"
}

---

### 🔄 Routing Logic in Make.com

```markdown
## 🔄 Routing Logic in Make.com

A single webhook is used for both `"I'm out"` and `"I'm back"`.

- **Route 1** → `"I'm out"` → Adds new row to Google Sheet
- **Route 2** → `"I'm back"`:
  - Searches rows where:
    - `sender == "Chipa"`
    - `status == "I'm out"`
  - Sorts by `timestamp` (descending)
  - Updates only the latest match

✅ Fields updated:
- `status`
- `timestamp`
- `location`
- `message`
- `battery`

## 🧩 Technical Highlights

- Avoids iOS Shortcuts' unreliable `If` logic by embedding actions inside menu options
- Uses `application/json` headers for clean API integration
- Compatible with Make.com's Google Sheets, Webhook, and Router modules

## ⚙️ Prerequisites

- iPhone with iOS 14+
- LINE app installed
- Make.com account with:
  - Webhook module
  - Router + filter logic
  - Google Sheets connection
- Google Sheet with headers:
  - `timestamp`, `sender`, `battery`, `location`, `message`, `status`

## 📦 Repository Contents

- `README.md` (this file)
- `.shortcut` export file (coming soon)
- Shortcut logic diagram (optional)


