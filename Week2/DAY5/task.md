# Day 5 — Capstone: From Raw Data to GitHub Portfolio

> **Goal:** Bring together everything from Days 1–4 into a single end-to-end mini project — and document it on GitHub so it looks like real professional work
> **Time:** ~6 hours
> **Output:** A public GitHub repository that tells the complete story of a data project, from raw collection to insight

---

## What You Are Building

A complete, documented data project called **`german-retail-etl`** — a real-looking GitHub repository that a hiring manager, mentor, or collaborator could open and immediately understand.

By the end of today you will have:

- A cleaned, analysed dataset based on your Day 2 Mockaroo work
- A Google Sheets workbook linked as the analysis layer
- A GitHub repository with proper structure, a full README, and documented decisions
- A written case study walking through every stage of the data value chain
- A reflection connecting your work back to the roles and career paths from Day 1

This is not a tutorial. There are no step-by-step instructions for the GitHub parts — you figure out the how, and the tasks tell you the what and why. That is intentional.

---

## How Today Connects to Days 1–4

| Day | What You Learned | How It Shows Up Today |
|---|---|---|
| Day 1 | Data roles, value chain, career paths | Your README introduction + role reflection |
| Day 2 | ETL pipeline, Mockaroo, Google Sheets | The core dataset and pipeline you document |
| Day 3 | Data cleaning, EDA, finding patterns | The analysis section and your insight writeup |
| Day 4 | Data-driven decisions, feedback loops | Your recommendations and impact measurement plan |

---

## Tasks

---

### Task 1 — Set Up Your GitHub Repository

Create a new public repository called `german-retail-etl` on GitHub.

**Repository structure to create:**

```
german-retail-etl/
│
├── README.md                  ← Main project documentation (biggest file today)
├── data/
│   ├── raw/
│   │   └── mockaroo_export.csv        ← Your original Mockaroo file, unmodified
│   └── cleaned/
│       └── german_retail_cleaned.csv  ← Exported cleaned_data tab from Day 2
├── analysis/
│   └── ANALYSIS.md            ← Your EDA findings written up
├── docs/
│   ├── pipeline.md            ← ETL pipeline documentation
│   ├── data_dictionary.md     ← What every column means
│   └── decisions.md           ← Why you made the choices you made
└── REFLECTIONS.md             ← Personal reflection connecting to career goals
```

Create every file and folder now, even if empty. Commit the structure with the message:
```
initial commit: project structure
```

> **Why this matters:** Real data repositories follow conventions. A recruiter or senior engineer should be able to navigate your repo without asking you a single question.

---

### Task 2 — Export and Commit Your Data

From your Day 2 Google Sheet `ETL_Pipeline_German_Retail`:

**Export the raw data:**
- Go to the `raw_data` tab → File → Download → CSV
- Rename it `mockaroo_export.csv`
- Place it in `data/raw/`

**Export the cleaned data:**
- Go to the `cleaned_data` tab → File → Download → CSV
- Rename it `german_retail_cleaned.csv`
- Place it in `data/cleaned/`

Commit both files with the message:
```
add: raw and cleaned dataset exports from ETL pipeline
```

**Write `docs/data_dictionary.md`**

Document every column in the dataset. Use this format:

```markdown
## Data Dictionary — German Retail Transactions

### raw dataset (`mockaroo_export.csv`)

| Column | Data Type | Description | Example Value | Notes |
|---|---|---|---|---|
| transaction_id | Integer | Unique identifier for each transaction | 1, 2, 3 | Auto-incremented by Mockaroo |
| timestamp | Datetime | Date and time of transaction | 2024-03-15 14:22:00 | Format: YYYY-MM-DD HH:MM:SS |
| customer_name | String | Full name of the customer | Anna Müller | Synthetic — not real |
| city | String | German city where sale occurred | Berlin | One of 8 cities |
| product | String | Product purchased | Laptop | One of 8 products |
| category | String | Product category | Electronics | Electronics or Zubehör |
| quantity | Integer | Number of units purchased | 3 | Should be ≥ 1 |
| unit_price_eur | Float | Price per unit in euros | 849.99 | 2 decimal places |
| payment_method | String | How the customer paid | Kreditkarte | One of 4 methods |
| satisfaction_score | Integer | Customer satisfaction rating | 4 | Scale 1–5 |
| returned | String | Whether item was returned | Nein | Ja or Nein |

### cleaned dataset (`german_retail_cleaned.csv`)

All columns from raw_data, plus:

| Column | Data Type | Description | Example Value | Notes |
|---|---|---|---|---|
| total_revenue_eur | Float | quantity × unit_price_eur | 2549.97 | Derived field added during Transform |
| data_quality_flag | String | Row-level quality marker | OK | OK or REVIEW — see pipeline.md |
```

Fill this out completely. Commit with:
```
docs: add data dictionary for raw and cleaned datasets
```

---

### Task 3 — Document the ETL Pipeline

Write `docs/pipeline.md` — a full technical description of the pipeline you built over Days 2 and 3. This is the most technical document in your repo. Every stage, every decision, every formula logic should be traceable here.

**Template to fill in:**

```markdown
# ETL Pipeline Documentation

## Overview

Brief 2–3 sentence description of what this pipeline does and why it exists.

---

## Pipeline Diagram

[Extract] → [Raw Storage] → [Transform] → [Clean Storage] → [Load] → [Dashboard]
Mockaroo      data/raw/       Sheets        data/cleaned/    Sheets    Dashboard tab

---

## Stage 1 — Extract

**Source:** Mockaroo (mockaroo.com)
**Format:** CSV
**Row count:** [your number]
**Fields:** [list them]
**Known issues at extraction:** [list every quality issue you seeded — this is important]

---

## Stage 2 — Transform

### 2a — Cleaning

| Issue | Rows Affected | Rule Applied | Outcome |
|---|---|---|---|
| Lowercase city names | 3 | Standardised to title case | Fixed |
| Invalid quantity (≤ 0) | 3 | Set to 1 or row deleted | Fixed |
| Missing satisfaction score | 4 | Imputed with city average | Estimated — flagged |
| Zero unit price | 2 | Set to NULL | Flagged for review |
| Invalid timestamp (1899) | 1 | Row deleted | Removed |

### 2b — Enrichment

| New Column | Formula Logic | Reason Added |
|---|---|---|
| total_revenue_eur | quantity × unit_price_eur | Required for revenue analysis |
| data_quality_flag | OR check on quality fields | Transparency for downstream users |

### 2c — Aggregation (Google Sheets formulas used)

| Summary Table | Formula Used | Sheet Tab |
|---|---|---|
| Revenue by city | SUMIF, COUNTIF, AVERAGEIF | transformed_data |
| Returns by city | COUNTIFS | transformed_data |
| Product performance | SUMIF, AVERAGEIF | transformed_data |
| Payment breakdown | COUNTIF | transformed_data |

---

## Stage 3 — Load

**Destination:** Google Sheets `dashboard` tab + CSV exports in `data/cleaned/`
**Consumers:** Store manager / business analyst
**Refresh cadence:** Manual — data must be re-exported when source updates

---

## Tool Used at Each Stage

| Stage | Tool | Why |
|---|---|---|
| Extract | Mockaroo | Free, realistic synthetic data with custom fields |
| Store raw | Google Sheets (`raw_data` tab) | Accessible, no setup required |
| Transform | Google Sheets formulas + manual fixes | Visible, auditable, beginner-friendly |
| Store clean | Google Sheets (`cleaned_data` tab) + CSV export | Portable and shareable |
| Load | Google Sheets (`dashboard` tab) | Charts and KPI cells without extra tools |

---

## Limitations of This Pipeline

[Write at least 4 honest limitations — e.g. not automated, single user only,
no data validation before loading, imputation risk, manual refresh, no error logging]
```

Commit with:
```
docs: add full ETL pipeline documentation
```

---

### Task 4 — Exploratory Data Analysis (EDA) Write-Up

Open your Google Sheet `transformed_data` tab. Look at all four summary tables carefully. Then write `analysis/ANALYSIS.md`.

This is not a list of numbers. It is a written story about what the data says. Write it as if you are presenting findings to a store manager in Berlin who does not know what SUMIF means.

**Structure to follow:**

```markdown
# Exploratory Data Analysis — German Retail Transactions

## Dataset at a Glance

- Total transactions: [n]
- Date range: [from] to [to]
- Cities covered: Berlin, München, Hamburg, Frankfurt, Köln, Stuttgart, Düsseldorf, Leipzig
- Products covered: 8

---

## Key Findings

### 1. Revenue by City

[Write 3–5 sentences. Which city generates the most revenue? Is it surprising given
population size? Is there a city underperforming relative to its size?
What question would you ask next if you were a regional manager?]

### 2. Product Performance

[Which product drives the most revenue? Which has the best satisfaction scores?
Is there a product with high sales volume but low satisfaction — a potential churn risk?
What would you recommend to the product team?]

### 3. Return Rates

[Which city has the highest return rate? Which product is returned most?
Is there a pattern — are expensive items returned more? What action could this drive?]

### 4. Payment Methods

[Is there a dominant payment method? Does Banküberweisung or Kreditkarte dominate?
What does this tell you about customer trust or checkout experience?]

### 5. Satisfaction Patterns

[What is the overall average? Are there products or cities with notably low scores?
How reliable is this data given 4 values were imputed?]

---

## Anomalies & Data Quality Notes

[Anything unusual — outliers, patterns that seem suspicious,
things you would verify with the source before presenting to a client]

---

## Questions This Data Cannot Answer

[What would you want to know that this dataset does not contain?
Write at least 3. This is the most important section — it shows analytical maturity.]

---

## Recommended Next Steps

[3 concrete, specific actions a German retail business could take based on your findings.
Not "improve satisfaction" — something like "investigate return rates in München for Laptops
before the Q4 campaign, as the 18% return rate is above the 12% city average"]
```

Commit with:
```
analysis: add EDA findings and business recommendations
```

---

### Task 5 — Write the Main README

This is the most important file in your repository. A good README is 50% of the impression the project makes. Write it for three audiences at once: a hiring manager who skims, a data professional who reads carefully, and a future version of yourself returning after 6 months.

**Write `README.md` using this structure:**

```markdown
# German Retail ETL Pipeline

A complete end-to-end data project simulating a retail analytics pipeline for
8 German cities — built as part of a structured data learning curriculum.

---

## Project Summary

One paragraph. What does this project do, what data does it use, what does it produce,
and what can someone learn from reading it?

---

## The Data Value Chain in This Project

| Stage | What Happens in This Project |
|---|---|
| 1. Collection | Mockaroo generates synthetic retail transactions for 8 German cities |
| 2. Storage | Raw CSV saved to data/raw/ — unmodified, never edited |
| 3. Processing | Google Sheets: nulls handled, formats standardised, invalid rows removed |
| 4. Analysis | SUMIF/AVERAGEIF aggregations by city, product, payment method |
| 5. Visualization | Dashboard tab with KPI cards and 3 charts, labelled in German |
| 6. Insight | EDA findings — top city, return patterns, product satisfaction gaps |
| 7. Decision | Specific recommendations for store managers based on findings |

---

## Repository Structure

[Paste the folder tree from Task 1]

---

## Pipeline at a Glance

[Paste the one-line pipeline diagram from docs/pipeline.md]

---

## Key Findings

[Your 3 strongest, most specific insights from ANALYSIS.md.
Each should name a city or product and a number — not just a direction.]

---

## Tools Used

| Tool | Purpose | Stage |
|---|---|---|
| Mockaroo | Synthetic data generation | Extract |
| Google Sheets | Cleaning, transformation, aggregation, dashboarding | Transform + Load |
| GitHub | Version control and documentation | Throughout |

---

## Google Sheets Workbook

[Paste the share link to your ETL_Pipeline_German_Retail Google Sheet here]
Set sharing to "Anyone with the link can view" before pasting.

---

## What I Learned

[3–5 honest, specific bullet points. Not "I learned ETL".
For example: "Imputing missing values with a city average is fast but hides
the real distribution — I would not use it for a satisfaction score KPI"]

---

## About This Project

Built during Week 1 of a self-directed data learning curriculum.
Topics covered: data roles, ETL pipelines, data cleaning, EDA, and decision-making with data.

[Your name] · [Date completed]
```

Commit with:
```
docs: write main project README
```

---

### Task 6 — Decisions Log

Write `docs/decisions.md`. This file is rare in beginner portfolios and immediately signals maturity to anyone reading your repo. Every meaningful choice you made has a reason. Document them.

```markdown
# Decision Log

## Decision 1 — Imputing missing satisfaction scores with city average

**Date:** [today]
**Context:** 4 rows had blank satisfaction_score values after extraction
**Options considered:**
- Delete the rows entirely
- Impute with the overall dataset average
- Impute with the city-level average
- Leave as NULL

**Decision:** Imputed with city-level average
**Reason:** City-level average is more contextually accurate than the overall average.
Deleting rows would unnecessarily reduce dataset size.
**Risk:** Imputed values reduce variance in satisfaction data. Any analysis of
satisfaction score distributions should note that 4 values are estimated, not real.

---

## Decision 2 — Deleting the 1899 timestamp row rather than correcting it

**Context:** One row had a timestamp year of 1899 — clearly a data entry error
**Options considered:**
- Attempt to guess the correct date
- Replace with today's date
- Delete the row
- Keep and flag it

**Decision:** [your choice]
**Reason:** [your reasoning]
**Risk:** [what you might lose by making this choice]

---

## Decision 3 — Setting zero-price rows to NULL rather than estimating a price

[Fill in following the same format]

---

## Decision 4 — Using SUMIF/AVERAGEIF formulas rather than pivot tables

[What are the tradeoffs between formulas and pivot tables for this use case?
Why did you choose one over the other?]

---

## Decision 5 — Keeping the raw_data tab unedited and copying to cleaned_data

[Why is this separation important? What would break if you edited raw_data directly?]
```

Write at least 5 decisions total. Commit with:
```
docs: add decision log for pipeline and analysis choices
```

---

### Task 7 — Personal Reflection

Write `REFLECTIONS.md`. This is the most personal file in your repository. It connects all five days to where you want to go. Future employers and collaborators read this to understand how you think, not just what you built.

```markdown
# Reflections — Week 1 Data Learning

## What I Built This Week

One paragraph summary of what exists now that did not exist 5 days ago.
Be specific — name the files, the tools, the data.

---

## Day-by-Day Connections

### Day 1 → Day 5
How did understanding data roles change how I approached this project?
Which role did I feel closest to while working — analyst, engineer, or scientist?
Which parts of the pipeline felt natural, and which felt unfamiliar?

### Day 2 → Day 5
What from the ETL theory on Day 2 did I only truly understand when I built it myself?
What surprised me about the Transform stage?
What would I have done differently if I had to build the pipeline again from scratch?

### Day 3 → Day 5
What was the messiest part of the data?
What would I have missed in the analysis if I had skipped cleaning entirely?
Did cleaning reveal anything about the data that changed my analysis?

### Day 4 → Day 5
Look at your three recommendations in ANALYSIS.md.
Are they genuinely data-driven, or are they assumptions dressed in numbers?
How would you measure whether those recommendations actually worked
if the store manager acted on them?

---

## What I Would Tell Someone Starting This Tomorrow

3 things — honest and specific. Not "trust the process."

---

## Career Direction

Based on what I enjoyed and struggled with this week:
- The role I find most interesting is: [and why — be specific]
- The skill gap I need to close first is:
- The type of data problem I want to work on eventually is:

---

## Open Questions

List every question this week raised that you do not yet have an answer to.
These become the agenda for Week 2.
```

Commit with:
```
reflections: add week 1 personal reflection and next steps
```

---

### Task 8 — Final Review & Polish

Before sharing the repository, do a full review pass. Do not skip this — the difference between a portfolio piece and a practice exercise is finishing it properly.

**README check:**
- [ ] Can someone understand the project within 10 seconds of opening it?
- [ ] Is every section complete — no placeholder text remaining?
- [ ] Is the data value chain table filled in for all 7 stages?
- [ ] Are the key findings specific — city names, numbers, not just directions?
- [ ] Is the Google Sheets share link added and working?

**Repository check:**
- [ ] All files committed: `README.md`, `pipeline.md`, `data_dictionary.md`, `ANALYSIS.md`, `decisions.md`, `REFLECTIONS.md`
- [ ] Both CSVs are in the correct folders (`data/raw/` and `data/cleaned/`)
- [ ] At least 8 commits with meaningful, readable messages
- [ ] Repository is set to **Public**

**Google Sheets check:**
- [ ] Sharing is set to "Anyone with the link can view"
- [ ] All 5 tabs exist and are complete: `raw_data`, `change_log`, `cleaned_data`, `transformed_data`, `dashboard`
- [ ] Dashboard charts are titled in German

**GitHub profile check:**
- [ ] Your GitHub profile has a name and a one-line bio
- [ ] `german-retail-etl` is pinned to your profile

**Final commit:**
```
chore: final review and polish — ready to share
```

---

## Deliverables

By end of Day 5 you should have a public GitHub repository containing:

- [ ] `README.md` — full project documentation with value chain table, findings, share link
- [ ] `data/raw/mockaroo_export.csv` — original unmodified Mockaroo export
- [ ] `data/cleaned/german_retail_cleaned.csv` — cleaned dataset export
- [ ] `docs/data_dictionary.md` — every column documented for both datasets
- [ ] `docs/pipeline.md` — ETL documentation with tool table and limitations
- [ ] `docs/decisions.md` — at least 5 documented decisions with reasoning and risk
- [ ] `analysis/ANALYSIS.md` — written EDA with 5 finding sections and 3 recommendations
- [ ] `REFLECTIONS.md` — personal week 1 reflection connecting all days to career direction
- [ ] At least 8 meaningful commits with descriptive messages
- [ ] Repository public and pinned to GitHub profile
- [ ] Google Sheets workbook shared and linked in README

---

## What Good Looks Like

A strong Day 5 repository is one where:

- The README tells the full story without opening any other file
- The pipeline doc is honest about limitations, not just achievements
- The data dictionary has no empty cells or placeholder text
- The EDA asks better questions than it answers
- The decisions log shows you thought about tradeoffs, not just solutions
- The reflections mention actual moments from the week, not just topics

A weak deliverable is a repo where the README says "this is a data project" and the EDA section says "Berlin had the most revenue."

---

## Key Concepts Demonstrated in This Project

| Concept | Where It Appears |
|---|---|
| Data value chain (7 stages) | README table |
| ETL pipeline | docs/pipeline.md + data/raw vs data/cleaned |
| Data lineage | change_log (Day 2) + decisions.md |
| Data dictionary | docs/data_dictionary.md |
| EDA | analysis/ANALYSIS.md |
| Data-driven recommendations | ANALYSIS.md — Recommended Next Steps |
| Version control | Git commit history |
| Technical documentation | All docs/ files |
| Data quality management | data_quality_flag column + decisions.md |
| Imputation | cleaned_data tab + Decision 1 |

---

## Resources

- [GitHub — Creating a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
- [GitHub — Writing a good README](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes)
- [Markdown guide — Basic syntax](https://www.markdownguide.org/basic-syntax/)
- [Mockaroo — Synthetic data generator](https://mockaroo.com)
- [Google Sheets — SUMIF, COUNTIF, AVERAGEIF reference](https://support.google.com/docs/table/25273)



