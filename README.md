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
