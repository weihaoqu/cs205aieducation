# Google Sheets Backend Setup

Follow these steps to create the Google Sheets backend that receives pre-test submissions from `pretest.html`.

---

## Step 1: Create the Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com) and create a new spreadsheet
2. Name it: **CS205 Pre-Test Responses (Spring 2026)**
3. Rename the first sheet tab to: **PreTest**
4. Add these column headers in Row 1:

| A | B | C | D | E | F | G | H | I | J–AI | AJ–BG |
|---|---|---|---|---|---|---|---|---|------|-------|
| Timestamp | AnonID | Section | Duration(s) | Major | PrereqGrade | PriorCourses | AIUsage | QuizScore | Q1–Q20 | E1–A8 |

Full column list (copy-paste into Row 1, one per cell):

```
Timestamp  AnonID  Section  Duration  Major  PrereqGrade  PriorCourses  AIUsage  QuizScore  Q1  Q2  Q3  Q4  Q5  Q6  Q7  Q8  Q9  Q10  Q11  Q12  Q13  Q14  Q15  Q16  Q17  Q18  Q19  Q20  E1  E2  E3  E4  E5  E6  E7  E8  S1  S2  S3  S4  S5  S6  S7  S8  A1  A2  A3  A4  A5  A6  A7  A8
```

That's 53 columns total.

---

## Step 2: Add the Apps Script

1. In your Google Sheet, go to **Extensions → Apps Script**
2. Delete any existing code in `Code.gs`
3. Paste the following:

```javascript
/**
 * CS205 Pre-Test Response Receiver
 * Accepts POST requests from pretest.html and appends rows to the sheet.
 */

// Handle POST requests from the web app
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("PreTest");
    if (!sheet) {
      sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    }

    var data = JSON.parse(e.postData.contents);

    // Build the row in column order
    var row = [
      new Date().toISOString(),           // Timestamp (server-side)
      data.anonId || "",                   // AnonID
      data.section || "",                  // Section
      data.duration || "",                 // Duration (seconds)
      data.major || "",                    // Major
      data.prereqGrade || "",              // Prereq Grade
      data.priorCourses || "",             // Prior Courses
      data.aiUsage || "",                  // AI Usage
      data.quizScore || 0,                 // Quiz Score
    ];

    // Quiz answers Q1–Q20
    for (var i = 1; i <= 20; i++) {
      row.push(data["Q" + i] || "");
    }

    // Survey answers E1–E8, S1–S8, A1–A8
    var surveyKeys = [
      "E1","E2","E3","E4","E5","E6","E7","E8",
      "S1","S2","S3","S4","S5","S6","S7","S8",
      "A1","A2","A3","A4","A5","A6","A7","A8"
    ];
    surveyKeys.forEach(function(key) {
      row.push(data[key] || "");
    });

    sheet.appendRow(row);

    return ContentService
      .createTextOutput(JSON.stringify({ status: "ok" }))
      .setMimeType(ContentService.MimeType.JSON);

  } catch (err) {
    return ContentService
      .createTextOutput(JSON.stringify({ status: "error", message: err.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

// Handle GET requests (for testing — visit the URL in a browser)
function doGet(e) {
  return ContentService
    .createTextOutput("CS205 Pre-Test receiver is running. Use POST to submit data.")
    .setMimeType(ContentService.MimeType.TEXT);
}
```

4. Click **Save** (Ctrl+S)

---

## Step 3: Deploy as Web App

1. Click **Deploy → New deployment**
2. Click the gear icon next to "Select type" and choose **Web app**
3. Configure:
   - **Description:** CS205 Pre-Test Receiver
   - **Execute as:** Me (your email)
   - **Who has access:** Anyone
4. Click **Deploy**
5. **Authorize** the script when prompted (click through the "unsafe app" warning — this is your own script)
6. Copy the **Web app URL** — it looks like:
   ```
   https://script.google.com/macros/s/AKfycbx.../exec
   ```

---

## Step 4: Connect to pretest.html

1. Open `pretest.html` in a text editor
2. Find this line near the top of the `<script>` section:
   ```javascript
   const SHEETS_URL = "YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL";
   ```
3. Replace it with your actual URL:
   ```javascript
   const SHEETS_URL = "https://script.google.com/macros/s/AKfycbx.../exec";
   ```
4. Save and deploy

---

## Step 5: Test It

1. Open `pretest.html` in a browser
2. Fill out a test response (use ID "0000-00" as a test)
3. Check your Google Sheet — a new row should appear within a few seconds
4. Delete the test row before administering to students

---

## Step 6: Share the Sheet

Share the spreadsheet with both instructors:

1. Click **Share** in the top right
2. Add both email addresses with **Editor** access
3. Both instructors can now view live responses as students submit

---

## Troubleshooting

**No data appearing?**
- Check that the URL in `pretest.html` matches the deployed URL exactly
- Re-deploy the script (Deploy → Manage deployments → Edit → New version → Deploy)
- Check the Apps Script execution log: Extensions → Apps Script → Executions

**CORS errors in console?**
- The app uses `mode: "no-cors"` for the fetch request, which means you won't see the response in the browser, but data still gets through. Check the sheet directly.

**Need to update the script?**
- After editing, you must create a new deployment version for changes to take effect
- Deploy → Manage deployments → Edit → Version: New version → Deploy

---

## Data Dictionary

| Column | Description | Values |
|--------|-------------|--------|
| Timestamp | Server-side ISO timestamp | Auto-generated |
| AnonID | Student self-generated ID | 6 digits (4 phone + 2 month) |
| Section | Treatment or control | `treatment` / `control` |
| Duration | Time to complete (seconds) | Integer |
| Major | Student's major | CS / CE / IS / Math / Other |
| PrereqGrade | Prerequisite course grade | A / B / C / D / Other |
| PriorCourses | Prior programming courses | 0 / 1 / 2 / 3+ |
| AIUsage | Prior AI tool usage | Never / A few times / Monthly / Weekly / Daily |
| QuizScore | Number correct out of 20 | 0–20 |
| Q1–Q20 | Individual quiz answers | A / B / C / D |
| E1–E8 | Engagement items | 1–5 (Likert) |
| S1–S8 | Self-efficacy items | 1–5 (Likert) |
| A1–A8 | AI perception items | 1–5 (Likert) |

**Note:** Items A3 and A6 are reverse-coded. Reverse them before computing the AI Perceptions construct average.
