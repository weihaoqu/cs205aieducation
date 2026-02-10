# Research Project Specification: AI-Assisted Teaching in CS 205 (Data Structures)

## 1. Overview

### Problem Statement
CS education faces a growing opportunity: AI tools can potentially transform how students learn theory-heavy subjects like data structures and algorithms. However, there is limited empirical evidence comparing AI-integrated instruction against traditional approaches in controlled settings with shared assessments.

### Research Questions
- **RQ1 (Learning Outcomes):** Does an AI-integrated teaching approach — using a custom web app with animations, interactive exercises, and AI-assisted hints — improve student performance on shared assessments compared to traditional instruction?
- **RQ2 (Student Experience):** How does AI-integrated instruction affect student engagement, self-efficacy, and perceptions of AI in learning?
- **RQ3 (AI-Specific Impact):** Can the contribution of AI-assisted features (hints and guidance) be isolated from the broader web app's effect on learning?

### Contribution
This is a quasi-experimental study comparing two parallel sections of CS 205 (Data Structures & Algorithms, Java) in Spring 2026. The study is novel in that it uses a purpose-built educational web app — not just a generic chatbot — and attempts to isolate the AI component's contribution through a within-section design.

---

## 2. Study Design

### Overall Design
**Quasi-experimental, non-equivalent groups with shared assessments.**

| | Your Section (Treatment) | Rolf's Section (Control) |
|---|---|---|
| Instruction | AI-integrated (web app + live demos) | Traditional lectures |
| Assessments | Shared quizzes, assignments, exams | Same shared assessments |
| Web App Access | Full (animations + interactives + AI hints) | None |
| Sample Size | < 50 students | < 50 students |

### AI Isolation Strategy (Recommendation)

Since you want to isolate the AI component and Rolf's section has no app access, a **within-section topic-toggle design** is recommended:

**How it works:**
- In your section, designate ~half the topics as "AI-on" and ~half as "AI-off"
- "AI-on" topics: Students see exercises with AI hints enabled during lectures
- "AI-off" topics: Same web app (animations + interactives) but AI hints disabled
- Alternate or counterbalance which topics get AI to avoid order effects

**This produces two comparisons:**
1. **Between-section** (primary): Your full section vs. Rolf's section → measures the whole intervention effect
2. **Within-section** (secondary): AI-on topics vs. AI-off topics within your section → isolates AI's specific contribution

**Why this approach:**
- Doesn't require Rolf to change anything (respects his light-role agreement)
- Every student experiences both conditions, increasing statistical power
- App analytics let you correlate AI hint usage with topic-level performance
- Already feasible mid-semester — just toggle the AI feature per topic going forward

### Threats to Validity & Controls
- **Instructor variation:** Both instructors are experienced, effective teachers with different styles — a natural feature of quasi-experimental classroom research, not a flaw. Mitigated by: shared assessments, documenting both instructors' backgrounds, and the within-section AI comparison (same instructor for both conditions)
- **Selection bias:** Students self-select into sections. Mitigated by: pre-test to establish baselines, collecting demographic/prerequisite data, and noting that sections have very similar demographics
- **Novelty effect:** Students in the treatment may perform better simply because the app is new and engaging. Mitigated by: the AI-on/off toggle (novelty applies to both conditions within your section)

---

## 3. The Intervention: AI-Enhanced Web App

### Description
A custom web application built using Claude Code, designed specifically for CS 205. The app serves as a lecture tool — projected and demonstrated during class.

### Core Features
1. **Animation Demonstrations** — Visual, step-by-step animations of data structure operations (e.g., tree rotations, graph traversals, sorting algorithm traces)
2. **Interactive Exercises** — Students follow along with exercises that require input/interaction during class (not just passive viewing)
3. **AI-Assisted Exercises (Hints & Guidance)** — When enabled, the AI provides step-by-step hints to guide students toward solutions without giving answers directly. This is the key experimental feature.

### Usage Model
- Used **during lectures only** — instructor projects the app and walks through it with students
- Students do not use the app independently outside class
- Full usage tracking is built in: time on task, exercises attempted/completed, AI interaction logs

---

## 4. App Architecture

### Tech Stack
- Web application built iteratively with Claude Code
- Specific frontend/backend frameworks: *(to be documented — capture this from the existing codebase)*

### Key Modules
| Module | Purpose |
|---|---|
| Animation Engine | Renders step-by-step DS operation visualizations |
| Exercise System | Interactive problems with input validation |
| AI Hint System | LLM-powered hint generation with step-by-step guidance |
| Analytics/Logging | Tracks all user interactions, timestamps, exercise outcomes |
| Topic Configuration | Toggle AI hints on/off per topic (needed for the study) |

### Feature Toggle Requirement (for the study)
The app needs a per-topic configuration to enable or disable AI hints. This is critical for the within-section AI isolation design. Implementation:
- Admin/instructor panel to set AI-on or AI-off per topic
- When AI is off, hint buttons are hidden or disabled — students still see animations and interactive exercises
- Logging captures which condition each exercise was in

### Data Export
The app should export structured data for analysis:
- Per-student, per-exercise: attempts, time spent, hints requested, hints used, correctness
- Per-topic: AI condition (on/off)
- Anonymized student IDs (mapped to consent status)

---

## 5. Control Condition (Rolf's Section)

Professor Rolf teaches the same CS 205 course with:
- Traditional lecture format (slides, whiteboard, live coding)
- Same textbook and topic sequence
- **Identical assessments** (quizzes, assignments, exams) — designed collaboratively
- No access to the web app
- Standard office hours and course resources

Rolf is a **co-author with a light role**: helps coordinate shared assessments and allows data collection from his section, but the PI leads research design and analysis.

---

## 6. Participants & Setting

- **Course:** CS 205 — Data Structures & Algorithms (Java)
- **Topics covered:** Arrays, linked lists, stacks, queues, trees, heaps, hash tables, graphs, sorting, searching, algorithm analysis (Big-O), recursion, dynamic programming, greedy algorithms
- **Enrollment:** < 50 students per section (total N < 100)
- **Demographics:** Both sections have very similar student demographics (time of day, major mix, prerequisite profiles)
- **IRB:** Approved
- **Semester:** Spring 2026 (started January 2026 — study is currently underway)

---

## 7. Data Collection Plan

### 7a. Pre-Test (URGENT — administer ASAP if not already done)
- **Purpose:** Establish baseline knowledge to control for pre-existing differences
- **Timing:** Week 2-3 of semester (if missed, administer immediately — some baseline data is better than none)
- **Both sections** take the same pre-test

### 7b. Shared Assessments (3-5 total)
- Quizzes, assignments, and/or exams shared between sections
- Primary quantitative outcome measure
- Score breakdowns by topic enable the within-section AI comparison

### 7c. Student Surveys (3 constructs)
Administered in the treatment section (and ideally the control section too):

| Construct | What it measures |
|---|---|
| Engagement & Motivation | How engaged and motivated students feel, perceived value of class activities |
| Self-Efficacy | Confidence in ability to learn and apply data structures concepts |
| Perceptions of AI | Views on AI as a learning tool — helpfulness, trust, ethical concerns |

**Timing:** Pre-survey (beginning of term) + post-survey (end of term). Consider a brief mid-semester pulse survey.

### 7d. Qualitative Interviews
- **Format:** Semi-structured, individual interviews
- **Timing:** End of term only
- **Sample:** 8-12 students from the treatment section (purposive sampling for diversity in performance levels and engagement)
- **Duration:** 20-30 minutes each
- **Focus:** Lived experience with the app, perceptions of AI hints, what helped/hindered learning

### 7e. App Analytics (Treatment Section Only)
- Per-exercise: AI hints requested, hints used, time on task, correctness
- Per-topic: AI-on vs. AI-off condition
- Aggregate patterns: engagement over time, drop-off, which features used most

---

## 8. Instruments & Measures (Recommendations)

### Pre/Post Concept Test
There is no widely-adopted validated concept inventory for data structures specifically. Recommended approach:

1. **Adapt the SCS1** (Second Computer Science 1 Assessment) for DS-relevant items
2. **Supplement with custom items** covering DS-specific concepts: linked list operations, tree traversals, hash collisions, Big-O analysis, sorting algorithm behavior
3. **Target:** 15-20 multiple-choice questions, ~20 minutes to administer
4. **Validation:** Have Rolf and 1-2 other DS instructors review items for content validity. If time permits, compute item discrimination and difficulty from the data.

### Survey Scales (Recommended Validated Instruments)
| Construct | Recommended Scale |
|---|---|
| Engagement | Adapted from the Student Course Engagement Questionnaire (SCEQ) by Handelsman et al. |
| Self-Efficacy | Adapted Computer Science Self-Efficacy Scale (Ramalingam & Wiedenbeck) |
| AI Perceptions | Custom scale — draw items from recent AI-in-education literature (e.g., Wang et al. 2023, Kasneci et al. 2023) |

Use 5-point Likert scales. Keep surveys short (< 30 items total) to maximize response rates.

### Interview Protocol (Outline)
1. General experience with the course
2. Experience with the web app — what was useful, what wasn't
3. Specific probes about AI hints — did they help? How? When didn't they help?
4. Comparison to other courses — how was this class different?
5. Suggestions for improvement
6. Views on AI in education broadly

---

## 9. Analysis Plan (Recommendations)

### Quantitative Analysis

**For small samples (N < 50 per group), prioritize effect sizes over p-values.**

| Comparison | Method | Notes |
|---|---|---|
| Between-section scores | Mann-Whitney U (non-parametric) or independent t-test | Report Cohen's d effect size |
| Controlling for pre-test | ANCOVA with pre-test score as covariate | Check assumptions; use robust ANCOVA if violated |
| Within-section AI-on vs AI-off | Paired t-test or Wilcoxon signed-rank | Compare topic-level scores within your section |
| Learning gains | Normalized gain (Hake) from pre-test to post-test | Standard in physics/CS education research |
| Survey data | Mann-Whitney U for between-group Likert comparisons | Report medians and IQR |
| App analytics | Correlation/regression: AI hint usage vs. performance | Exploratory — look for dose-response patterns |

**Bayesian analysis** is worth considering as a supplement: it handles small samples more gracefully and lets you quantify evidence for/against your hypothesis (rather than just failing to reject null).

**Software:** R (with packages: `effsize`, `BayesFactor`, `lme4`) or Python (`scipy`, `pingouin`, `bambi`).

### Qualitative Analysis
- **Method:** Thematic analysis (Braun & Clarke 2006)
- **Process:** Transcribe interviews → open coding → develop themes → review themes → write narrative
- **Integration:** Use a convergent mixed-methods design — present quantitative and qualitative findings side by side, noting where they converge or diverge

---

## 10. Ethical Considerations

### IRB & Consent
- IRB approval: **Obtained**
- Informed consent must be collected from all students in both sections
- Students who don't consent still take the same course and assessments — their data is simply excluded from analysis
- Consent forms should clearly explain: what data is collected, how it's anonymized, that participation is voluntary and won't affect grades

### Data Privacy
- All student data must be de-identified before analysis
- App analytics should use anonymized student IDs
- Interview recordings/transcripts stored securely, accessible only to research team
- No individual student performance shared with the other instructor

### Fairness
- **Key concern:** If the AI-enhanced approach is significantly better, students in Rolf's section received inferior instruction
- **Mitigation:** After the study, share the web app and findings with Rolf so the improvement can benefit all future students
- **AI-off topics:** Students in your section miss AI hints for some topics. Mitigated by: all students still get the full app (just without AI for certain topics), and the number of AI-off topics should be balanced

### AI-Specific Ethics
- Document which AI model powers the hint system (and its limitations)
- AI hints should guide, not give answers — verify this is the case
- Acknowledge in the paper that the app was built using AI tools (Claude Code)

---

## 11. Risk Mitigation

### Risk: Small Sample Size (< 50 per group)
- **Impact:** May lack statistical power to detect small effects
- **Mitigations:**
  - Use effect sizes (Cohen's d) as the primary outcome metric — meaningful even with small N
  - Use Bayesian analysis to quantify evidence strength
  - The within-section AI comparison increases power (paired design)
  - Consider combining with a future replication semester for a larger pooled analysis
  - Frame the paper as a "case study with quantitative evidence" rather than claiming generalizability

### Risk: Instructor Confound
- **Impact:** Differences in outcomes could be due to instructor quality, not the app
- **Mitigations:**
  - Document both instructors' teaching styles, experience, and course materials
  - The within-section AI toggle compares AI-on vs AI-off under the SAME instructor — this is your strongest evidence for AI's specific impact
  - Collect student evaluations of both instructors for context
  - Acknowledge this limitation transparently in the paper

### Risk: Student Compliance
- **Impact:** Low survey response rates, disengaged app usage, or shallow interview responses
- **Mitigations:**
  - Keep surveys short (< 10 minutes)
  - Offer small incentives for survey completion (extra credit, gift cards — check IRB)
  - Administer pre/post surveys during class time
  - For interviews, recruit with a personal email and offer scheduling flexibility
  - App engagement is less of a concern since YOU control it during lectures

---

## 12. Timeline — Spring 2026

*Semester started January 2026. Current date: February 10, 2026.*

| Week | Dates (approx.) | Action Items |
|---|---|---|
| **1-3** | Jan 12 – Jan 30 | ~~Semester start, administer pre-test and pre-survey~~ |
| **4** ⚡ | **Feb 2 – Feb 6** | **URGENT: If pre-test/pre-survey not yet done, administer THIS WEEK** |
| **5** ⚡ | **Feb 9 – Feb 13** | Implement AI feature toggle in the web app. Decide AI-on/AI-off topic assignments. Begin logging. |
| **6-7** | Feb 16 – Feb 27 | First shared assessment (quiz/assignment). Continue lectures with AI toggle active. |
| **8-9** | Mar 2 – Mar 13 | Midterm exam (shared). Optional: mid-semester pulse survey. |
| **10** | Mar 16 – Mar 20 | Spring break (if applicable). Review mid-semester data. |
| **11-13** | Mar 23 – Apr 10 | Continue lectures with AI toggle. Second shared assessment. |
| **14-15** | Apr 13 – Apr 24 | Final exam (shared). Administer post-test and post-survey. |
| **16** | Apr 27 – May 1 | Conduct end-of-term interviews (8-12 students). |
| **May-Jun** | | Data cleaning, analysis, write-up. |
| **Jul-Aug** | | Draft paper. Target SIGCSE 2027 submission (deadline typically mid-August). |

---

## 13. Publication Strategy (Recommendations)

### Primary Target: SIGCSE Technical Symposium 2027
- **Why:** Premier CS education venue, strong fit for empirical classroom studies, high visibility
- **Format:** Full paper (6 pages ACM format) or experience report
- **Deadline:** Typically mid-August 2026
- **Framing:** "Empirical comparison of AI-integrated vs. traditional instruction in a data structures course, with a novel within-section design to isolate AI's contribution"

### Alternative/Additional Venues
| Venue | Fit | Notes |
|---|---|---|
| **ITiCSE 2027** | Strong | International audience, accepts empirical studies, deadline ~Jan 2027 |
| **AIED 2027** | Good | If you emphasize the AI system design and hint mechanism |
| **Computers & Education** (journal) | High if results are strong | High-impact journal, longer format allows full mixed-methods write-up |
| **ACM TOCE** (journal) | Strong | CS-education-specific journal, rigorous peer review |
| **L@S 2027** | Moderate | If you emphasize the platform/technology aspect |

### Framing the Contribution
Lead with what's unique:
1. **Purpose-built educational tool** (not just "we let students use ChatGPT")
2. **Within-section AI isolation design** (methodological contribution)
3. **Mixed methods** with app analytics (rich data beyond just test scores)
4. **Shared assessments** between sections (stronger control than most CS-ed studies)

---

## 14. Open Questions

1. **Pre-test status:** Has the pre-test been administered yet? If not, this is the #1 priority this week.
2. **AI toggle implementation:** How much work is needed to add a per-topic AI on/off toggle to the web app?
3. **Topic assignment:** Which specific topics will be AI-on vs. AI-off? Should be balanced by difficulty and timing.
4. **Rolf's buy-in on surveys:** Will Rolf administer the same surveys in his section? Having comparison survey data would strengthen the paper significantly.
5. **Incentives:** Has IRB approved incentives (extra credit, gift cards) for survey/interview participation?
6. **Concept test items:** Who will develop the pre/post concept test questions? Timeline for item review?
7. **Interview training:** Who will conduct the interviews? Are they trained in qualitative methods?
