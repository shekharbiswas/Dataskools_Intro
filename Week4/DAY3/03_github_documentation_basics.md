# Day 3 — GitHub Documentation Basics
## Capital Bikeshare Pipeline: Making Your Work Public and Findable

> **Built on:** The pipeline (Day 1) and insight cards (Day 2) you've already completed

> **Tool:** GitHub + Markdown

> **Duration:** 6 hours

> **Difficulty:** ⭐⭐

> **Feeds into:** Day 4 Portfolio — your repo link goes on your personal page; Day 5 — you reference it in your presentation

---

## 🧭 The Big Picture

You've built a real data pipeline. You've created presentation-ready visuals. Today you make all of it **public, navigable, and professional** — so that anyone (a potential employer, a classmate, a recruiter) can open a single link and immediately understand what you did, how you did it, and what you found.

This is what separates a data project that lives on your laptop from one that lives in your portfolio.

In the real world, every data engineering and analytics team uses version control and documentation. A GitHub repository is not just storage — it is the evidence that you can work like a professional. The README is not just a description — it is the first thing a hiring manager reads when they click your profile.

By the end of today you will have:
- A public GitHub repository for your Capital Bikeshare project
- A README that tells the full story of your pipeline
- Organised folders that mirror the structure of your pipeline
- A reflection journal entry that shows your thinking, not just your output
- A repo link ready to embed in your Day 4 portfolio

---

## 🗺️ Today's Plan

| Stage | Task | Time |
|-------|------|------|
| Stage 1 | Create your repository and folder structure | 45 min |
| Stage 2 | Write your README.md | 90 min |
| Stage 3 | Upload your pipeline assets | 60 min |
| Stage 4 | Write your data journal entry | 45 min |
| Stage 5 | Final checks and repo polish | 30 min |

---

## Stage 1 — Create Your Repository and Folder Structure (45 min)

### 1A — Create the repo

1. Go to [github.com](https://github.com) and sign in (or create a free account)
2. Click the **+** icon → **New repository**
3. Name it: `capital-bikeshare-pipeline`
   - Use lowercase and hyphens — this is the standard for repo names
   - ❌ `CapitalBikeshare` `My Project` `bikeshare_FINAL`
   - ✅ `capital-bikeshare-pipeline`
4. Set visibility to **Public** — your portfolio depends on this being findable
5. Check **"Add a README file"** — GitHub will create a blank one
6. Click **Create repository**

### 1B — Build your folder structure

Your folder structure should mirror the pipeline you built. Every tab in your spreadsheet maps to a folder or file here. Anyone who opens your repo should immediately see the pipeline architecture.

```
capital-bikeshare-pipeline/
│
├── README.md                        ← The front page — written in Stage 2
│
├── data/
│   ├── README.md                    ← "Raw data not included — see source link below"
│   └── schema.md                    ← Copy your 01_SCHEMA column definitions here
│
├── pipeline/
│   ├── pipeline_log.md              ← Your PIPELINE_LOG entries, cleaned up
│   └── cleaning_rules.md            ← The is_valid logic and exclusion decisions from Stage 2
│
├── charts/
│   ├── chart1_rider_duration.png
│   ├── chart2_hourly_pattern.png
│   ├── chart3_duration_histogram.png
│   └── chart4_ab_test_results.png
│
├── insight_cards/
│   ├── card1_[name].png             ← Your Canva exports from Day 2
│   ├── card2_[name].png
│   └── card3_[name].png
│
├── analysis/
│   ├── stats_summary.md             ← Key numbers from 04_STATS
│   ├── probability_findings.md      ← Independence test result from 05_PROBABILITY
│   └── ab_test_results.md           ← Full A/B test writeup from 07_AB_TEST
│
└── journal/
    └── day3-reflection.md           ← Written in Stage 4
```

### How to create folders in GitHub

GitHub does not allow empty folders. To create a folder, you create a file inside it.

For each folder:
1. Click **Add file** → **Create new file**
2. In the filename box, type: `data/README.md`
   (Typing the `/` automatically creates the folder)
3. Add a one-line placeholder: `# Data`
4. Click **Commit new file**

Repeat for each folder: `pipeline/`, `charts/`, `insight_cards/`, `analysis/`, `journal/`

> 💡 **Naming rule:** Every file name should describe its contents, not its sequence.
> - ❌ `chart1.png` `analysis_v2.md` `final_final.md`
> - ✅ `chart_rider_duration_by_type.png` `ab_test_results.md`

Log in `PIPELINE_LOG`:
```
[Day 3 — Stage 1] GitHub repo created
Repo URL: https://github.com/[your-username]/capital-bikeshare-pipeline
Folder structure: data/, pipeline/, charts/, insight_cards/, analysis/, journal/
Visibility: Public
```

---

## Stage 2 — Write Your README.md (90 min)

The README is the most important file in your repository. It is read before anything else. A weak README makes a strong project invisible. A strong README makes even a simple project look professional.

Your README has one job: **let a stranger understand your project in under 2 minutes.**

### The structure

Copy this template into your `README.md` and fill it in:

---

```markdown
# Capital Bikeshare Data Pipeline
### A Google Sheets pipeline built on September 2023 trip data

---

## What This Project Is

A complete data pipeline built entirely in Google Sheets, taking raw Capital Bikeshare 
trip data from ingestion through cleaning, aggregation, statistical analysis, probability 
modelling, and A/B testing — ending in a business-ready dashboard.

**One-sentence story:**  
[Your story headline from Day 2 — e.g., "Casual riders and members use the same bikes 
in fundamentally different ways, and an A/B test reveals that bike type drives real 
differences in trip length."]

---

## The Pipeline Architecture

| Tab | Layer | What it does |
|-----|-------|--------------|
| `00_RAW` | Ingestion | Untouched CSV import — never modified |
| `01_SCHEMA` | Documentation | Column definitions, data types, quality notes |
| `02_CLEAN` | Transformation | Derived columns, `is_valid` flag, cleaning summary |
| `03_AGGREGATE` | Aggregation | Summary metrics — the "fact table" for downstream tabs |
| `04_STATS` | Analysis | Five-number summary, distribution shape, CoV |
| `05_PROBABILITY` | Analysis | Empirical probabilities, independence test, joint distribution |
| `06_SAMPLING` | Experiment prep | Random 3,000-row extract (1,500 per group) |
| `07_AB_TEST` | Experiment | Welch's t-test, p-value, Cohen's d, confound check |
| `08_DASHBOARD` | Output | One-screen summary referencing all upstream tabs |

---

## Key Findings

[Fill these in from your own pipeline results]

- **Total valid trips (September 2023):** XX,XXX
- **Data quality pass rate:** XX%
- **Members vs casual split:** XX% members / XX% casual
- **Mean trip duration — members:** XX.X min · **casual:** XX.X min
- **Most popular hour:** [X AM/PM]
- **A/B test result (classic vs electric):** p = X.XXX · Cohen's d = X.XX · [Significant / Not significant]

---

## The A/B Test

**Hypothesis:** Electric bike riders take longer trips than classic bike riders.

**Method:** Welch's two-sample t-test (α = 0.05) on a random sample of 1,500 classic 
and 1,500 electric bike trips drawn from valid September 2023 records.

**Result:** [Write your plain-English interpretation here — 2–3 sentences maximum.]

---

## Visuals

[Your insight cards — embed them using Markdown image syntax]

![Rider Duration Comparison](insight_cards/card1_rider_duration.png)
*[Your 2-sentence takeaway for this card]*

![Hourly Pattern](insight_cards/card2_hourly_pattern.png)
*[Your 2-sentence takeaway]*

![A/B Test Results](insight_cards/card4_ab_test.png)
*[Your 2-sentence takeaway]*

---

## Tools Used

- **Google Sheets** — entire pipeline built here (no Python, no SQL, no external tools)
- **Canva** — insight card design and chart polish
- **GitHub** — version control and portfolio publication

---

## Data Source

Capital Bikeshare System Data — September 2023  
Available at: [capitalbikeshare.com/system-data](https://capitalbikeshare.com/system-data)  
Raw data not included in this repo. Download the file `202309-capitalbikeshare-tripdata.csv` 
from the source link and import into `00_RAW` to reproduce this pipeline.

---

## What I Learned

[Write 3–5 sentences in your own voice — not bullet points. What surprised you? 
What was harder than expected? What would you do differently with more time or better tools?]

---

## Pipeline Log

See [`pipeline/pipeline_log.md`](pipeline/pipeline_log.md) for a full record of 
decisions made at each stage — cleaning rules, exclusion rationale, test design choices.
```

---

### README writing rules

**The Key Findings section is the most important part after the opening.** Fill it with your actual numbers. A README with placeholder text (`XX,XXX`) looks unfinished. A README with real numbers (`87,432 valid trips · 72.3% pass rate`) looks like the work of someone who actually ran the pipeline.

**The "What I Learned" section should sound like you.** This is not a list of tools. It is the one section where a recruiter or instructor sees how you think. Write it in full sentences. Include one thing that surprised you and one thing you'd do differently.

**Never write a README in second person.** This is your project, not a tutorial for others.
- ❌ "In this project, you will learn how to..."
- ✅ "This pipeline processes September 2023 Capital Bikeshare trip data through..."

---

## Stage 3 — Upload Your Pipeline Assets (60 min)

### 3A — Upload your charts and insight cards

From your `Day5_Presentation/` folder (created on Day 2):

1. Go to your repo on GitHub
2. Click into the `charts/` folder
3. Click **Add file** → **Upload files**
4. Drag in all chart PNGs from Day 2
5. In the commit message box, write: `Add pipeline charts from Day 2 visual analysis`
6. Click **Commit changes**

Repeat for `insight_cards/` folder.

> 💡 **Commit messages matter.** In a real team, someone else reads these. Write them as if explaining to a colleague what you changed and why — not just "added files" or "update".
> - ❌ `update` `final` `stuff` `asdfgh`
> - ✅ `Add rider duration bar chart from 03_AGGREGATE` `Upload A/B test results insight card`

### 3B — Write and upload your analysis summaries

Create three Markdown files in the `analysis/` folder. Each one is a clean writeup of findings from the corresponding pipeline tab — no formulas, no Sheets syntax, just the results and their interpretation.

**`analysis/stats_summary.md`**

```markdown
# Statistical Summary — September 2023 Trip Data

## Five-Number Summary: Trip Duration (all valid trips)

| Statistic | All Trips | Members | Casual |
|-----------|-----------|---------|--------|
| Mean | | | |
| Median | | | |
| Std Dev | | | |
| 25th Pctile | | | |
| 75th Pctile | | | |
| Min | | | |
| Max | | | |

## Distribution Shape

[2–3 sentences: Is it right-skewed? What is the mean–median gap? 
What does the coefficient of variation tell us?]

## Key Insight

[1 sentence that a non-technical manager would immediately understand]
```

**`analysis/probability_findings.md`**

```markdown
# Probability Analysis — Rider and Bike Type

## Empirical Probabilities

| Event | Probability |
|-------|------------|
| P(member) | |
| P(casual) | |
| P(electric bike) | |
| P(classic bike) | |
| P(duration > 30 min) | |

## Independence Test: Is electric bike preference independent of rider type?

P(electric | member) = XX%  
P(electric | casual) = XX%  
P(electric overall) = XX%

Finding: [1–2 sentences — are they dependent or independent? What does that mean?]

## Business Implication

[2 sentences — what should Capital Bikeshare do with this information?]
```

**`analysis/ab_test_results.md`**

```markdown
# A/B Test: Classic vs Electric Bike Trip Duration

## Hypothesis

H₀: There is no difference in mean trip duration between classic and electric bike riders.  
H₁: Electric bike riders have different mean trip durations than classic bike riders.  
Significance level: α = 0.05

## Results

| Metric | Classic Bike | Electric Bike |
|--------|-------------|---------------|
| n | 1,500 | 1,500 |
| Mean duration (min) | | |
| Std deviation (min) | | |

p-value: [your value]  
Cohen's d: [your value] — [Small / Medium / Large] effect  
Verdict: [Reject / Fail to reject] H₀

## Plain English Interpretation

[2–3 sentences. What does the p-value mean in plain language? 
Is the effect size worth acting on?]

## Confound Check

[2–3 sentences from your Stage 7F analysis — does rider type explain the difference?]

## Recommendation

[The 2-sentence recommendation from your 08_DASHBOARD — copied here verbatim]
```

### 3C — Add your pipeline log

Copy your `PIPELINE_LOG` entries from Google Sheets into `pipeline/pipeline_log.md`. Clean up any shorthand — write it so that someone who never saw your spreadsheet could follow the decisions you made at each stage.

Add a short intro at the top:

```markdown
# Pipeline Log — Capital Bikeshare Analytics Pipeline

This log records every significant decision made during pipeline construction: 
data quality issues found, cleaning rules applied, design choices and their rationale.

Built in Google Sheets. September 2023 Capital Bikeshare data.
```

---

## Stage 4 — Write Your Data Journal Entry (45 min)

Create `journal/day3-reflection.md`. This is not a summary of what you did — it is a reflection on how you're thinking. Journals are what separate analysts who grow from analysts who just execute tasks.

Answer these four questions in full sentences (aim for 150–250 words total — not more):

**1. What surprised you most in the data?**
Not "I found that members ride shorter" — go deeper. Was the magnitude of the difference surprising? Did any number contradict your intuition before you ran the analysis?

**2. What was the hardest part of building this pipeline?**
Be specific. Not "it was hard" — which stage? Which formula? What did you have to do to get past it?

**3. What does your A/B test result actually mean for Capital Bikeshare?**
Pretend you're explaining it to the Head of Operations. What should they do (or not do) based on your p-value and Cohen's d?

**4. If you had one more day and access to 12 months of data instead of one month, what would you investigate?**
This question is about thinking like a data analyst — what questions does your current analysis leave unanswered?

> 💡 **Journal tip:** The best journal entries make a reader feel like they're watching someone think, not just reading a list of facts. Write the thing that occurred to you that you didn't put in any other document.

---

## Stage 5 — Final Checks and Repo Polish (30 min)

Before you close your laptop today, go through this checklist on your GitHub repo page.

### Repo health checklist

**Structure**
- [ ] All six folders exist: `data/`, `pipeline/`, `charts/`, `insight_cards/`, `analysis/`, `journal/`
- [ ] No folders are empty (GitHub doesn't show empty folders anyway — every folder has at least one file)
- [ ] No file is named `final`, `v2`, `updated`, or `copy`

**README**
- [ ] Opens with a clear project title and one-sentence story
- [ ] Pipeline architecture table is complete (all 9 tabs listed)
- [ ] Key Findings section has real numbers — no placeholders
- [ ] At least 2 insight card images are embedded and display correctly
- [ ] "What I Learned" is written in your own voice, in full sentences
- [ ] Data source link works

**Analysis files**
- [ ] `stats_summary.md` — table filled with real values
- [ ] `probability_findings.md` — independence test finding written in plain English
- [ ] `ab_test_results.md` — p-value, Cohen's d, plain English interpretation, recommendation

**Assets**
- [ ] At least 3 chart PNGs uploaded to `charts/`
- [ ] At least 3 insight card PNGs uploaded to `insight_cards/`
- [ ] Images display correctly when you click on them in GitHub

**Commits**
- [ ] Every commit has a descriptive message (not "update" or "add files")
- [ ] At least 5 commits total — shows the work was done in stages, not dumped all at once

### Copy your repo URL

Your repo URL is: `https://github.com/[your-username]/capital-bikeshare-pipeline`

Save this. You will paste it into your portfolio page tomorrow (Day 4) and reference it in your Day 5 presentation.

Log in `PIPELINE_LOG`:
```
[Day 3] GitHub documentation complete
Repo URL: 
README: complete (Key Findings filled with real numbers)
Analysis files: stats_summary.md · probability_findings.md · ab_test_results.md
Assets uploaded: [count] charts · [count] insight cards
Journal entry: journal/day3-reflection.md
Repo checklist: all items cleared
```

---

## 🧭 What This Feeds Into

| Day | What you're building toward |
|-----|----------------------------|
| Day 1 | Pipeline — your data source and analysis |
| Day 2 | Visuals — your charts and insight cards |
| Day 3 (today) | GitHub repo — your work made public and documented |
| Day 4 | Portfolio page — your repo link and insight cards go live on a personal site |
| Day 5 | Presentation — you walk the audience from the question to the pipeline to the finding |

Tomorrow, your GitHub URL and your insight card images go into your personal portfolio page. Make sure everything is uploaded and your README is complete before you finish today.

---

## ✅ Day 3 Deliverables Checklist

- [ ] Public GitHub repo: `capital-bikeshare-pipeline`
- [ ] Folder structure: `data/` · `pipeline/` · `charts/` · `insight_cards/` · `analysis/` · `journal/`
- [ ] `README.md` — complete with real numbers, embedded images, data source link
- [ ] `analysis/stats_summary.md` — five-number summary with real values
- [ ] `analysis/probability_findings.md` — independence test finding in plain English
- [ ] `analysis/ab_test_results.md` — p-value, effect size, recommendation
- [ ] `pipeline/pipeline_log.md` — full log from Days 1–3
- [ ] `charts/` — at least 3 chart PNGs
- [ ] `insight_cards/` — at least 3 insight card PNGs
- [ ] `journal/day3-reflection.md` — 150–250 words, 4 questions answered
- [ ] All commit messages are descriptive
- [ ] Repo URL saved and ready for Day 4

---

## 💡 Bonus Challenges

**If you finish early — pick one:**

1. **Add a `data/schema.md`** that reproduces your `01_SCHEMA` column definitions in Markdown table format. This is what a data engineering team calls a "data dictionary" — and it's the first thing a new analyst on your team would read.

2. **Write a `pipeline/cleaning_rules.md`** that documents every condition in your `is_valid` formula as a numbered list, with the reason for each rule. Example: "Rule 3: duration must be ≤ 180 minutes. Rationale: trips longer than 3 hours are almost certainly data errors — a bike left unreturned, not an actual journey."

3. **Add a GitHub topic tag to your repo.** Click the gear icon next to "About" on your repo page. Add tags: `data-engineering` `google-sheets` `data-pipeline` `bikeshare` `statistics`. This makes your repo discoverable when recruiters or other learners search GitHub by topic.

---

> Good documentation is not decoration. It is the difference between work that lives and work that dies on your laptop. The pipeline you built on Day 1 is only useful if someone else — or future you, six months from now — can open it and understand it in ten minutes. Today you made that possible.
