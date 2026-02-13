# Google Sheets Backend — Setup Guide

The script auto-detects whether incoming data is a **pretest** or **quiz** submission and routes it to the correct sheet tab.

---

## Step 1: Create a Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com) → **Blank spreadsheet**
2. Name it: **CS205 Responses**
3. Rename the first tab to **PreTest**
4. Add a second tab named **Quizzes**
5. Leave both tabs completely empty — headers are auto-created on first submission

---

## Step 2: Paste the Script

1. In the sheet, go to **Extensions → Apps Script**
2. Delete everything in `Code.gs` and paste this:

```javascript
function doPost(e) {
  var ss = SpreadsheetApp.getActiveSpreadsheet();

  // Data arrives as a form field named "data" containing JSON
  var raw = e.parameter && e.parameter.data ? e.parameter.data : e.postData.contents;
  var data = JSON.parse(raw);

  // Detect payload type: quizzes have a "quiz" field, pretest does not
  if (data.quiz) {
    handleQuiz(ss, data);
  } else {
    handlePretest(ss, data);
  }

  return ContentService
    .createTextOutput(JSON.stringify({status: "ok"}))
    .setMimeType(ContentService.MimeType.JSON);
}

// ── PRETEST HANDLER ─────────────────────────────────────
function handlePretest(ss, data) {
  var sheet = ss.getSheetByName("PreTest");
  if (!sheet) {
    sheet = ss.insertSheet("PreTest");
  }

  var cols = [
    "Timestamp","AnonID","Section","Duration","Major","PriorCourses","QuizScore",
    "Q1","Q2","Q3","Q4","Q5","Q6","Q7","Q8","Q9","Q10",
    "Q11","Q12","Q13","Q14","Q15","Q16","Q17","Q18","Q19","Q20",
    "E1","E2","E3","E4","E5","E6","E7","E8",
    "S1","S2","S3","S4",
    "A1","A2","A3","A4"
  ];

  var keyMap = {
    "Timestamp": "_ts", "AnonID": "anonId", "Section": "section",
    "Duration": "duration", "Major": "major", "PriorCourses": "priorCourses",
    "QuizScore": "quizScore"
  };

  // Auto-write headers if Row 1 is empty
  if (sheet.getLastRow() === 0) {
    sheet.appendRow(cols);
    sheet.getRange(1, 1, 1, cols.length).setFontWeight("bold");
  }

  var row = cols.map(function(col) {
    if (col === "Timestamp") return new Date().toISOString();
    var key = keyMap[col] || col;
    return data[key] !== undefined ? data[key] : "";
  });

  sheet.appendRow(row);
}

// ── QUIZ HANDLER ────────────────────────────────────────
function handleQuiz(ss, data) {
  var sheet = ss.getSheetByName("Quizzes");
  if (!sheet) {
    sheet = ss.insertSheet("Quizzes");
  }

  var cols = [
    "Quiz","Timestamp","AnonID","Section","Duration","Score","Total",
    "Q1","Q2","Q3","Q4","Q5","Q6","Q7","Q8"
  ];

  var keyMap = {
    "Quiz": "quiz", "Timestamp": "timestamp", "AnonID": "anonId",
    "Section": "section", "Duration": "duration", "Score": "score", "Total": "total"
  };

  // Auto-write headers if Row 1 is empty
  if (sheet.getLastRow() === 0) {
    sheet.appendRow(cols);
    sheet.getRange(1, 1, 1, cols.length).setFontWeight("bold");
  }

  var row = cols.map(function(col) {
    var key = keyMap[col] || col;
    return data[key] !== undefined ? data[key] : "";
  });

  sheet.appendRow(row);
}

function doGet() {
  return ContentService.createTextOutput("CS205 receiver is running.");
}
```

3. Click **Save** (Ctrl+S)

---

## Step 3: Deploy

1. Click **Deploy → New deployment**
2. Gear icon → choose **Web app**
3. Set:
   - **Execute as:** Me
   - **Who has access:** Anyone
4. Click **Deploy**, then **Authorize** (click through the "unsafe app" warning — it's your own script)
5. Copy the URL it gives you (looks like `https://script.google.com/macros/s/AKfycb.../exec`)

> **Updating an existing deployment?** Go to **Deploy → Manage deployments**, click the pencil icon, set **Version** to "New version", and click **Deploy**. You must select "New version" for changes to take effect.

---

## Step 4: Paste the URL into your HTML files

The URL is already configured in all quiz files (`quiz1.html` through `quiz6.html`) and `pretest.html`. If you need to change it, search for `SHEETS_URL` in each file.

---

## Step 5: Test

1. Open any live quiz, fill out a test response
2. Check the Google Sheet:
   - Pretest submissions → **PreTest** tab
   - Quiz submissions → **Quizzes** tab
3. A new row should appear in ~5 seconds
4. Delete the test row before giving it to students

---

## Columns Reference

### PreTest tab (39 columns)

| Columns | What |
|---------|------|
| Timestamp | Server-side timestamp |
| AnonID | 6-char anonymous ID (4 phone digits + 2-digit birth month) |
| Section | `treatment` (Weihao) or `control` (Rolf) |
| Duration | Seconds spent on the quiz+survey |
| Major | CS / CE / IS / Math / Other |
| PriorCourses | 0 / 1 / 2 / 3+ |
| QuizScore | 0–20 |
| Q1–Q20 | Quiz answers (A/B/C/D) |
| E1–E8 | Engagement survey (1–5 Likert) |
| S1–S4 | Self-Efficacy survey (1–5 Likert) |
| A1–A4 | AI Perceptions survey (1–5 Likert) |

### Quizzes tab (15 columns)

| Columns | What |
|---------|------|
| Quiz | Quiz ID (`quiz1` through `quiz6`) |
| Timestamp | Client-side ISO timestamp |
| AnonID | 6-char anonymous ID (4 phone digits + 2-digit birth month) |
| Section | `treatment` (Weihao) or `control` (Rolf) |
| Duration | Seconds spent on the quiz |
| Score | Number correct (0–8) |
| Total | Always 8 |
| Q1–Q8 | Student's answers (A/B/C/D) |
