# Student Reading Deep Dive Report Template (v2.0 - CoT Optimized)

## Overview
This template generates a concise executive summary report for student reading performance analysis. To ensure accuracy and consistency, the process is divided into two distinct steps:

- **Analysis Phase (CoT):** Extract data, calculate metrics, and draft content.
- **Generation Phase (HTML):** Render the final report using the validated content.

---

## CRITICAL: Data File Ingestion
**ALWAYS ingest data files in chunks (roughly 1/3 of the files at a time).**

Do **NOT** attempt to read all data files in a single operation. Safe approach:

- **First batch:** Read 2–3 files (e.g., MAP PDFs, Student Progress CSV)
- **Second batch:** Read 2–3 more files (e.g., Daily Metrics CSV, ORF data)
- **Third batch:** Read remaining files (e.g., Reading Reports, Skills data)

---

## STEP 1: PRE-COMPUTATION ANALYSIS (Mandatory)
**INSTRUCTION:** Before generating any HTML, you MUST output a plain-text analysis block. This acts as your scratchpad to ensure logic is sound before formatting.

### 1. Placement & Baseline Verification
- **Extract Effective Grade:** (From `Student_Progress_Tra_*.csv` ONLY) → `[Value]`
- **Extract R90:** (From `Student_Progress_Tra_*.csv`) → `[Value]`

**Check ORF Prerequisite**
- **Target:** Pass Grade N-1 Benchmark (e.g., for G2 placement, must pass Level J/G1)
- **Actual:** `[Student WCPM] / [Target WCPM]`
- **Status:** `[PASS / FAIL / NOT TAKEN]`

**ORF Passing Benchmarks (must meet BOTH WCPM AND 95% accuracy):**
- Level D (Grade K): 20 WCPM @ 95% accuracy
- Level J (Grade 1): 67 WCPM @ 95% accuracy
- Level M (Grade 2): 104 WCPM @ 95% accuracy

**Placement Logic Check**
- Does Actual App Placement == Effective Grade? `[Yes/No]`
- If NO, is it because ORF Prerequisite was failed? `[Yes/No]`
- **Conclusion:** `[Premature Placement / Correct / Too Low]`

### 2. Curriculum Alignment Check (Crucial for Low Growth)
- **Identify MAP Need:** Look at “Skills to Develop” in MAP PDF. Pick the top 1–2 skills → `[Skill Names]`
- **Check App Coverage:** Did the student work on these skills in the App Skills CSV? `[Yes/No]`
- **Mastery Check:** If Yes, was it mastered (>80%)? `[Yes/No]` or `[Doom Loop (3+ failures)]`
- **Verdict:** `[Aligned / Gap / Misaligned]`

### 3. Metric Calculations
- **Growth Calculation:** `Winter 2025 RIT - Winter 2024 RIT` (If unavailable, use Fall→Winter)

**Active Time Calculation**
- **ClearFluency:** Sum Step 1 + Step 2 + Step 3 minutes only. Exclude “Other” time.
- **All Apps:** Calculate average based on *Days with Activity* (not calendar days).

**ClearFluency Fluency Bar**
- Extract Actual WCPM and Target WCPM for all stories.
- Calculate: `(Avg Actual / Avg Target) * 100` → `[Fluency %]`

### 4. Root Cause Drafting (Mad Libs Style)
Draft **3 specific root causes** using this exact formula:

- **Cause 1 (Behavior/Time):**  
  `"Label: [Explanation] [Metric: X min vs Target Y] [Date Range]"`
- **Cause 2 (Placement/Curriculum):**  
  `"Label: [Explanation] [Metric: XP/Min X.X] [Comparison: Effective Grade X vs App Grade Y]"`
- **Cause 3 (Operational/Other):**  
  `"Label: [Explanation] [Metric: Days paused / Weeks gap]"`

### 5. App Effectiveness Verdicts
For each active app:

**[App Name]**
- ORF Change: `[Start WCPM] → [End WCPM]`
- Accuracy Pattern: `[Avg %]`
- XP/Min: `[Value]`
- **Verdict:** `[EFFECTIVE / MIXED / INEFFECTIVE]` because…

---

## STEP 2: HTML REPORT GENERATION
Once the analysis above is complete, generate `index-v2.html`.

### Visual Design Specifications
- **Header:** `<h1>Reading Deep Dive: [Student Name]</h1>`

**Color Palette**
- `--danger:` #e53e3e  
- `--warning:` #d69e2e  
- `--success:` #38a169  
- `--text:` #2d3748  

### Section 1: Root Cause Analysis
- Metric cards use calculations from Step 1
- Root cause callouts copied verbatim from Step 1
- **Rule:** Every root cause must include a *Date, Number, and Comparison*

### Section 2: Remediation Plan
- Numbered list
- Red border for #1, Yellow for #2–3
- Immediate, actionable steps only

### Section 3: Supporting Data (Strict Schema)
**A. Data Tables**
- RIT Trajectory
- MAP Domain Scores (with color badges)

**B. ORF Placement Check**
- Columns: Text Level, WCPM, Accuracy, Benchmark
- **NO DATE COLUMN**

**C. MAP-Indicated Placement vs Start of Year Placement**
- "Actual Placement" column should be labeled "Start of Year Placement"
- This shows the mismatch between MAP recommendation and initial placement at year start

**C. App Performance Visualization**
Render order:
1. Fluency Bar (ClearFluency ONLY)
2. XP/Day
3. Active Time/Day
4. Accuracy
5. XP/Min

**HTML Structure Rule (CRITICAL)**
`.app-verdict` must be a sibling of `.app-row`, not a child.

---

## Final Output Integrity Checklist
- Header title is **“Reading Deep Dive: [Name]”**
- Label is exactly **“Active Time/Day”**
- `.app-verdict` is outside `.app-row`
- ClearFluency Fluency Bar is first
- ORF table has **no Date column**
- Effective Grade comes from Student_Progress CSV

---

## Example Usage Prompt
Perform a reading deep dive for **[Student Name]** using the attached files.

Follow the 2-step process:
1. Output the Analysis Block (Step 1)
2. Output `index-v2.html` (Step 2)

**Data files:** `[List files]`
