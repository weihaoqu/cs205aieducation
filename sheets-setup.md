# Google Sheets Backend — 5-Minute Setup

No column names to type. The script auto-creates headers on the first submission.

---

## Step 1: Create a Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com) → **Blank spreadsheet**
2. Name it: **CS205 Pre-Test Responses**
3. That's it — leave the sheet completely empty

---

## Step 2: Paste the Script

1. In the sheet, go to **Extensions → Apps Script**
2. Delete everything in `Code.gs` and paste this:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);

  // Column order — headers auto-created on first submission
  var cols = [
    "Timestamp","AnonID","Section","Duration","Major","PriorCourses","QuizScore",
    "Q1","Q2","Q3","Q4","Q5","Q6","Q7","Q8","Q9","Q10",
    "Q11","Q12","Q13","Q14","Q15","Q16","Q17","Q18","Q19","Q20",
    "E1","E2","E3","E4","E5","E6","E7","E8",
    "S1","S2","S3","S4",
    "A1","A2","A3","A4"
  ];

  // Map from column name to JSON key
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

  // Build row
  var row = cols.map(function(col) {
    if (col === "Timestamp") return new Date().toISOString();
    var key = keyMap[col] || col;  // Q1, E1, S1, A1 etc. match directly
    return data[key] !== undefined ? data[key] : "";
  });

  sheet.appendRow(row);

  return ContentService
    .createTextOutput(JSON.stringify({status: "ok"}))
    .setMimeType(ContentService.MimeType.JSON);
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

---

## Step 4: Paste the URL into pretest.html

Open `pretest.html`, find this line near the top of the `<script>`:

```javascript
const SHEETS_URL = "YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL";
```

Replace it with your URL:

```javascript
const SHEETS_URL = "https://script.google.com/macros/s/AKfycb.../exec";
```

Then push to GitHub. Done — responses will appear in your Google Sheet automatically.

---

## Step 5: Test

1. Open the live pretest, fill out a test response (use ID `000099`)
2. Check the Google Sheet — a new row should appear in ~5 seconds
3. Delete the test row before giving it to students

---

## Columns Reference (39 total, all auto-created)

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
