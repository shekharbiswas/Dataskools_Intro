# Project: Build a Data Pipeline in Google Sheets

## Capital Bikeshare — From Raw Data to A/B Test Decision

> **Dataset:** [Capital Bikeshare System Data](https://capitalbikeshare.com/system-data) — September 2023  
> **Tool:** Google Sheets only  
> **Duration:** 8 hours (one full project day)  
> **Difficulty:** ⭐⭐⭐⭐ Capstone  



## 🧭 The Big Picture

In the real world, a **data pipeline** is a series of steps that takes raw, messy data and turns it into something a business can act on. Every major company — Uber, Spotify, Netflix — runs pipelines that:

1. **Ingest** raw data from a source
2. **Clean and transform** it into a reliable format
3. **Aggregate** it into summary tables
4. **Analyse** it with statistics and probability
5. **Experiment** on it with A/B tests
6. **Report** the findings to decision-makers

Today you will build exactly this — but entirely inside Google Sheets. Each tab is one layer of your pipeline. The stats, probability, and A/B test you learned this week are not standalone exercises anymore. They are **downstream consumers** of the pipeline you build.

By the end of the day you will have one working spreadsheet that a data engineer, an analyst, and a product manager could each open and use.



## Pipeline Architecture — Your Tab Map

Before touching any data, draw this map. Create all tabs now with these exact names (you'll fill them one by one):

| Tab Name         | Pipeline Layer    | What lives here                                         |
| ---------------- | ----------------- | ------------------------------------------------------- |
| `00_RAW`         | Ingestion         | The untouched CSV import — never edited                 |
| `01_SCHEMA`      | Documentation     | Column definitions, data types, quality notes           |
| `02_CLEAN`       | Transformation    | Cleaned rows, derived columns (duration, hour, weekday) |
| `03_AGGREGATE`   | Aggregation       | Summary counts and metrics — your "fact table"          |
| `04_STATS`       | Analysis          | Descriptive statistics pulled from `03_AGGREGATE`       |
| `05_PROBABILITY` | Analysis          | Empirical probabilities and conditional probabilities   |
| `06_SAMPLING`    | Experiment Prep   | Random sample extraction for the A/B test               |
| `07_AB_TEST`     | Experiment        | The actual test, p-value, effect size                   |
| `08_DASHBOARD`   | Output / BI Layer | One-screen summary that pulls from all upstream tabs    |
| `PIPELINE_LOG`   | Meta              | Your running log of decisions and observations          |

> 💡 **Data engineering rule #1:** Raw data is sacred. `00_RAW` is never modified after import. Every change happens in a downstream tab. If something goes wrong, you can always trace back to the source.

---

## Time Budget

| Stage                         | Tab(s)                      | Time   |
| ----------------------------- | --------------------------- | ------ |
| Stage 1 — Ingest & Document   | `00_RAW`, `01_SCHEMA`       | 45 min |
| Stage 2 — Clean & Transform   | `02_CLEAN`                  | 60 min |
| Stage 3 — Aggregate           | `03_AGGREGATE`              | 60 min |
| Stage 4 — Stats Layer         | `04_STATS`                  | 60 min |
| Stage 5 — Probability Layer   | `05_PROBABILITY`            | 50 min |
| Stage 6 — Sampling & A/B Test | `06_SAMPLING`, `07_AB_TEST` | 75 min |
| Stage 7 — Dashboard           | `08_DASHBOARD`              | 50 min |
| Stage 8 — Pipeline Review     | `PIPELINE_LOG`              | 40 min |





# STAGE 1 — Ingest & Document
## Tabs: `00_RAW` and `01_SCHEMA` · 45 minutes



## 📥 Step 1A — Ingest the Raw Data (15 min)

1. Download `202309-capitalbikeshare-tripdata.csv` from [capitalbikeshare.com/system-data](https://capitalbikeshare.com/system-data)
2. Open a new Google Sheet → rename the first tab `00_RAW`
3. File → Import → Upload → choose the CSV
4. Import settings: **Replace current sheet**, separator = **Comma**, uncheck "Convert text to numbers"

> Why "uncheck convert"? Because we want to control type conversion ourselves in `02_CLEAN`, not let the tool make silent decisions. This is a data engineering discipline — **explicit over implicit.**

5. In cell `P1` (first empty column after your data), type: `IMPORTED_AT`  
   In cell `P2`, type: `=NOW()` then immediately copy → paste special → values only.  
   This timestamps your ingestion. Freeze it — don't let it recalculate.

6. Record in `PIPELINE_LOG`:

```
[Stage 1A] Raw ingestion
Date: [today]
Source: capitalbikeshare.com/system-data
File: 202309-capitalbikeshare-tripdata.csv
Row count (incl. header): =COUNTA(00_RAW!A:A)
Columns: 11
```



## 📋 Step 1B — Document the Schema (30 min)

In your `01_SCHEMA` tab, build a schema registry — the document that tells anyone opening this pipeline what every column means.

### Schema Registry Table

Build this table manually (type all labels, fill in your observations):

| #  | Column Name        | Raw Data Type   | Description                     | Example Value        | Nullable? | Notes / Issues         |
| -- | ------------------ | --------------- | ------------------------------- | -------------------- | --------- | ---------------------- |
| 1  | ride_id            | Text            | Unique identifier for each trip | 6D045F2C174B4...     | No        | Check for duplicates   |
| 2  | rideable_type      | Text            | Type of bike used               | classic_bike         | No        |                        |
| 3  | started_at         | Text (datetime) | Trip start timestamp            | 2023-09-01 00:02:... | No        | Needs parsing          |
| 4  | ended_at           | Text (datetime) | Trip end timestamp              | 2023-09-01 00:...    | No        | Needs parsing          |
| 5  | start_station_name | Text            | Name of origin station          | 14th & V St NW       | Yes       | Blank = dockless start |
| 6  | end_station_name   | Text            | Name of destination station     | ...                  | Yes       | Blank = dockless end   |
| 7  | start_lat          | Number          | Latitude of start               | 38.9...              | Yes       |                        |
| 8  | start_lng          | Number          | Longitude of start              | -77.0...             | Yes       |                        |
| 9  | end_lat            | Number          | Latitude of end                 | 38.9...              | Yes       |                        |
| 10 | end_lng            | Number          | Longitude of end                | -77.0...             | Yes       |                        |
| 11 | member_casual      | Text            | Rider membership status         | member               | No        | Only 2 values          |


### Data Quality Audit

Below your schema table, add a **quality audit section**. Use `COUNTBLANK` and `COUNTIF` on `00_RAW` to fill this in:

| Column | Blank / Null Count | % Missing | Action needed? |
|--|--|--|-|
| `start_station_name` | `=COUNTBLANK(00_RAW!E:E)` | `=[blank]/COUNTA(00_RAW!A:A)*100` | |
| `end_station_name` | | | |
| `end_lat` | | | |
| `ride_id` | | | |
| `member_casual` | | | |

Also check:
```
Distinct rideable_type values:   [list them — sort column B to find]
Distinct member_casual values:   [list them]
Duplicate ride_ids:  =COUNTA(00_RAW!A:A) - SUMPRODUCT(1/COUNTIF(00_RAW!A2:A100000,00_RAW!A2:A100000))
```

> The duplicate check formula is complex — look at it carefully. It counts how many ride_ids appear more than once. If the result is greater than 0, your data has duplicates that need handling downstream.

**Write 3 data quality observations in `PIPELINE_LOG`:**
```
[Stage 1B] Schema documentation
- Quality issue 1: 
- Quality issue 2: 
- Quality issue 3: 
- Decision: we will handle these issues in Stage 2 by...
```




# STAGE 2 — Clean & Transform
## Tab: `02_CLEAN` · 60 minutes



In `02_CLEAN`, we reference `00_RAW` — we never copy-paste raw values. This keeps the pipeline traceable: if the source changes, cleaning updates automatically.

> **Data engineering rule #2:** Transformations are documented and reproducible. Someone else should be able to read your formula and understand exactly what transformation you applied and why.

### Step 2A — Mirror the raw data (5 min)

In cell `A1` of `02_CLEAN`, type the header: `ride_id`  
In cell `A2`, type: `=00_RAW!A2`  
Do NOT fill this down yet — we'll use a filtered approach.

Actually, for Sheets performance, we'll work differently: copy the entire `00_RAW` tab content to `02_CLEAN` as **values only** (select all → copy → go to `02_CLEAN` → paste special → values only). This is your "staging" copy. From here, all new columns are computed transformations.

> Log in `PIPELINE_LOG`: "02_CLEAN is a values-copy of 00_RAW as of [date]. All new columns are derived."

### Step 2B — Add derived columns (35 min)

These are your **transformation columns** — the core of the cleaning stage. Add each as a new column to the right of your data, with a clear header name.



**Column M — `duration_min`**  
Formula for M2:
```
=IF(OR(C2="",D2=""), "", 
  ROUND(
    ((DATEVALUE(MID(D2,1,10)) + TIMEVALUE(MID(D2,12,8))) - 
     (DATEVALUE(MID(C2,1,10)) + TIMEVALUE(MID(C2,12,8)))) * 1440
  , 1))
```
What this does: extracts date and time from the raw text string, calculates the difference, converts to minutes, rounds to 1 decimal. Returns blank if either timestamp is missing.



**Column N — `hour_of_day`**  
Formula for N2:
```
=IF(C2="", "", HOUR(TIMEVALUE(MID(C2,12,8))))
```
Returns: 0–23 (the hour when the trip started)



**Column O — `day_of_week`**  
Formula for O2:
```
=IF(C2="", "", TEXT(DATEVALUE(MID(C2,1,10)), "dddd"))
```
Returns: "Monday", "Tuesday", etc.



**Column P — `is_weekend`**  
Formula for P2:
```
=IF(C2="", "", IF(OR(O2="Saturday",O2="Sunday"), "weekend", "weekday"))
```



**Column Q — `is_valid`**  
This is your **row-level quality flag** — the single most important column in the pipeline. Every row gets a TRUE or FALSE based on your cleaning rules.

Formula for Q2:
```
=AND(
  M2 <> "",
  M2 > 1,
  M2 < 180,
  A2 <> "",
  K2 <> ""
)
```

What this checks:
- Duration exists and is between 1 and 180 minutes
- `ride_id` is not blank
- `member_casual` is not blank

> **Data engineering rule #3:** Never silently delete bad rows. Flag them with `is_valid = FALSE` and keep them in the pipeline. This way you can always audit how many rows were excluded and why. Downstream tabs simply filter on `is_valid = TRUE`.



**Fill all four columns down** for all rows (click each cell, Ctrl+Shift+End to find last row, Ctrl+D to fill down). This will take a moment.

### Step 2C — Cleaning summary (20 min)

At the bottom of 02_CLEAN (or in a sidebar section), add:

| **Metric**                              | **Formula**                                              | **Value** |
| --------------------------------------- | -------------------------------------------------------- | --------- |
| Total raw rows                          | `=COUNTA(A:A)-1`                                         |           |
| Valid rows (`is_valid=TRUE`)            | `=COUNTIF(Q:Q,TRUE)`                                     |           |
| Invalid rows                            | `=COUNTIF(Q:Q,FALSE)`                                    |           |
| % rows passing quality                  | `=COUNTIF(Q:Q,TRUE)/(COUNTA(A:A)-1)*100`                 |           |
| Rows excluded for duration out of range | `=COUNTIFS(M:M,"<>"&"",M:M,"<=1")+COUNTIFS(M:M,">"&180)` |           |
| Rows excluded for blank fields          | `=COUNTIFS(Q:Q,FALSE,M:M,"<>")`                          |           |


**Log in `PIPELINE_LOG`:**
```
[Stage 2] Cleaning complete
Valid rows: 
Invalid rows: 
% pass rate: 
Exclusion reasons documented: duration out of range, blank fields
```




# STAGE 3 — Aggregate
## Tab: `03_AGGREGATE` · 60 minutes



The aggregation layer is where raw rows become **summary metrics**. This is equivalent to a SQL `GROUP BY` — instead of working with 300,000 individual trips, downstream tabs work with compact summary tables.

> **Data engineering rule #4:** Downstream layers should never touch raw rows. If `04_STATS` needs to know the mean duration for members, it should read from a pre-computed cell in `03_AGGREGATE`, not run `AVERAGEIF` on 300k rows itself. This keeps the pipeline fast and the logic centralised.

All formulas in `03_AGGREGATE` reference `02_CLEAN` and include the `is_valid=TRUE` filter using `COUNTIFS`.

### Step 3A — Overall trip metrics

Create a section called **"Overall Metrics"**:

| **Metric**            | **Formula**                                                                                                       | **Value** |
| --------------------- | ----------------------------------------------------------------------------------------------------------------- | --------- |
| Total valid trips     | `=COUNTIF(02_CLEAN!Q:Q,TRUE)`                                                                                     |           |
| Date range start      | `=MIN(IF(02_CLEAN!Q2:Q400000=TRUE,DATEVALUE(MID(02_CLEAN!C2:C400000,1,10))))` *(array formula: Ctrl+Shift+Enter)* |           |
| Date range end        | Same as above using `MAX` instead of `MIN`                                                                        |           |
| Unique start stations | Manual count or sort                                                                                              |           |
| Classic bike trips    | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!B:B,"classic_bike")`                                                        |           |
| Electric bike trips   | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!B:B,"electric_bike")`                                                       |           |
| Member trips          | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!K:K,"member")`                                                              |           |
| Casual trips          | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!K:K,"casual")`                                                              |           |
| Weekend trips         | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!P:P,"weekend")`                                                             |           |
| Weekday trips         | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!P:P,"weekday")`                                                             |           |
                                                                                                   |           |




### Step 3B — Duration metrics by segment

Create a section called **"Duration by Segment"**:

| **Segment**     | **Mean Duration**                                                          | **Median (Approx)**                                                                      | **% > 30 min** | **% < 5 min** |
| --------------- | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | -------------- | ------------- |
| All valid trips | `=AVERAGEIFS(02_CLEAN!M:M,02_CLEAN!Q:Q,TRUE)`                              | `=PERCENTILE(IF(02_CLEAN!Q2:Q400000=TRUE,02_CLEAN!M2:M400000),0.5)` *(Ctrl+Shift+Enter)* |                |               |
| Members         | `=AVERAGEIFS(02_CLEAN!M:M,02_CLEAN!Q:Q,TRUE,02_CLEAN!K:K,"member")`        |                                                                                          |                |               |
| Casual riders   | `=AVERAGEIFS(02_CLEAN!M:M,02_CLEAN!Q:Q,TRUE,02_CLEAN!K:K,"casual")`        |                                                                                          |                |               |
| Classic bikes   | `=AVERAGEIFS(02_CLEAN!M:M,02_CLEAN!Q:Q,TRUE,02_CLEAN!B:B,"classic_bike")`  |                                                                                          |                |               |
| Electric bikes  | `=AVERAGEIFS(02_CLEAN!M:M,02_CLEAN!Q:Q,TRUE,02_CLEAN!B:B,"electric_bike")` |                                                                                          |                |               |
| Weekday         | `=AVERAGEIFS(02_CLEAN!M:M,02_CLEAN!Q:Q,TRUE,02_CLEAN!P:P,"weekday")`       |                                                                                          |                |               |
| Weekend         | `=AVERAGEIFS(02_CLEAN!M:M,02_CLEAN!Q:Q,TRUE,02_CLEAN!P:P,"weekend")`       |                                                                                          |                |               |


For `% > 30 min` columns:
```
=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!K:K,"member",02_CLEAN!M:M,">"&30)
 / COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!K:K,"member") * 100
```



### Step 3C — Hourly distribution table

Create a section called **"Trips by Hour"** — this will feed the dashboard chart.

| Hour | Trip Count | % of Day |
||--||
| 0 | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!N:N,0)` | `=[count]/total*100` |
| 1 | | |
| ... fill rows 0–23 | | |



### Step 3D — Duration frequency table (for histogram)

Create a section called **"Duration Buckets"**:

| Bucket | Label | Count | % of Total |
|--|-|-|--|
| 0–5 | "0–5 min" | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!M:M,">"&0,02_CLEAN!M:M,"<="&5)` | |
| 5–10 | "5–10 min" | | |
| 10–15 | | | |
| 15–20 | | | |
| 20–30 | | | |
| 30–45 | | | |
| 45–60 | | | |
| 60–90 | | | |
| 90–180 | | | |

> This section is what `04_STATS` will read for its distribution analysis. The stats layer doesn't touch `02_CLEAN` directly — it reads these pre-computed buckets.

**Log in `PIPELINE_LOG`:**
```
[Stage 3] Aggregation complete
Sections built: Overall Metrics, Duration by Segment, Hourly Distribution, Duration Buckets
All formulas reference 02_CLEAN with is_valid=TRUE filter
Downstream tabs (04, 05, 07, 08) should read from this tab only
```


# STAGE 4 — Statistics Layer
## Tab: `04_STATS` · 60 minutes



> **Important framing:** In a real pipeline, a data analyst or BI tool sits at this layer. They don't re-process raw data — they query the aggregation layer. That's what we do here: `04_STATS` reads from `03_AGGREGATE`, not from `02_CLEAN`.

### Step 4A — Five-number summary

Create a **"Distribution Summary"** section. Pull values from `03_AGGREGATE` where you already computed them, and add the ones missing:

| Statistic | All Trips | Members | Casual |
|--|--||--|
| Mean | `=03_AGGREGATE!C4` *(reference your cell)* | | |
| Median | | | |
| Std Dev | `=STDEV(IF(02_CLEAN!Q2:Q400000=TRUE,02_CLEAN!M2:M400000))` *(Ctrl+Shift+Enter)* | | |
| Min | `=MINIFS(02_CLEAN!M:M,02_CLEAN!Q:Q,TRUE)` | | |
| 25th Pctile | array formula with PERCENTILE+IF | | |
| 75th Pctile | | | |
| Max | | | |

### Step 4B — Describe the distribution shape

From the duration bucket counts you built in `03_AGGREGATE`, answer:

1. Which bucket has the highest count? What does that tell us about how people use bikeshare?
2. Is the distribution right-skewed, left-skewed, or symmetric? How do you know from the mean vs median gap?
3. Calculate the **coefficient of variation**: `Std Dev / Mean * 100`. If this is over 100%, the data is extremely variable. What does your result mean?
4. Write the mean vs median gap as a sentence a non-technical manager would understand.

### Step 4C — Comparison chart

Using the **"Duration by Segment"** table from `03_AGGREGATE`, build a bar chart in `04_STATS`:

1. Copy the segment names and mean duration values into a clean 2-column table in this tab
2. Insert → Chart → Bar chart
3. Title: "Mean Trip Duration by Rider Segment"
4. Add data labels to each bar (click chart → Edit → Customize → Series → Data labels)

This is your **stats layer output** — a clean visual that the dashboard can reference.

### Step 4D — Standard deviation insight

Look at your Std Dev values for members vs casual riders. Answer:

1. Which group has higher variability in ride duration?
2. What does a high standard deviation tell a bikeshare operations manager about that group's behaviour?
3. If the standard deviation for casual riders is, say, 18 minutes and the mean is 22 minutes, calculate the range that covers most trips (mean ± 2 std dev). Write it as: *"Most casual trips last between X and Y minutes."*

**Log in `PIPELINE_LOG`:**
```
[Stage 4] Statistics layer complete
Key finding 1: 
Key finding 2: 
Distribution shape: [right-skewed / symmetric / other]
Mean vs median gap: [X] minutes — implies [your interpretation]
```

# STAGE 5 — Probability Layer
## Tab: `05_PROBABILITY` · 50 minutes



### Step 5A — Empirical probability table

Pull counts from `03_AGGREGATE` (do not re-run COUNTIFS on raw data). Build:

| Event | Count (from 03_AGGREGATE) | Total Valid Trips | Probability | As % |
|-|--|-|-||
| P(casual) | `=03_AGGREGATE!C10` | `=03_AGGREGATE!C3` | `=B2/C2` | |
| P(member) | | | | |
| P(electric bike) | | | | |
| P(classic bike) | | | | |
| P(weekend) | | | | |
| P(duration > 30 min) | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!M:M,">"&30)` | | | |
| P(duration < 5 min) | | | | |

> For P(duration > 30 min) we do need to go to `02_CLEAN` since `03_AGGREGATE` doesn't have that exact cut — but log that as a gap in your aggregate layer that you'd fix in a real pipeline.

**Sanity checks:**
```
P(casual) + P(member) = ?    → should be 1.0 (or 100%)
P(electric) + P(classic) = ? → should be 1.0 — if not, are there other bike types?
```

### Step 5B — Conditional probability: the independence test

**Question: Is electric bike preference independent of rider type?**

Calculate:

| Probability | Formula | Value |
|||-|
| P(electric) overall | already done | |
| P(electric \| member) | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!K:K,"member",02_CLEAN!B:B,"electric_bike") / COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!K:K,"member")` | |
| P(electric \| casual) | same for casual | |

**The independence rule:** If these three values are equal (or very close), electric bike choice is independent of rider type. If they differ significantly, the variables are **dependent** — knowing someone's rider type tells you something about their bike preference.

Write your finding in 3 sentences: what you found, whether they are dependent or independent, and what it means for a business decision about electric bike fleet allocation.


### Step 5C — Joint probability table

Build a 2×2 joint probability matrix:

|  | Classic Bike | Electric Bike | **Row Total** |
|--|-|--|--|
| **Member** | `=COUNTIFS(02_CLEAN!Q:Q,TRUE,02_CLEAN!K:K,"member",02_CLEAN!B:B,"classic_bike")/[total]` | | |
| **Casual** | | | |
| **Column Total** | | | **1.0** |

All four cells should sum to 1.0. The row totals should equal P(member) and P(casual). The column totals should equal P(classic) and P(electric). If they don't, you have a formula error.

This table is called a **joint distribution**. It shows the probability of *both* conditions being true at once.


### Step 5D — Probability interpretation questions

Answer each in 2–3 sentences:

1. A random trip is selected from the dataset. You're told it used an electric bike. Does that make it more or less likely to be a casual rider? By how much? Use your conditional probability values.

2. The company wants to target advertising for their electric bikes. Based on this probability analysis, should they target members or casual riders first? Justify with numbers.

3. If you picked 10 random trips, what is the probability that *all 10* are from members? (Use P(member)^10.) Is this a likely or unlikely outcome?

**Log in `PIPELINE_LOG`:**
```
[Stage 5] Probability layer complete
Independence finding: electric bike choice [is / is not] independent of rider type
P(electric|member) = X%  vs  P(electric|casual) = Y%  vs  P(electric overall) = Z%
Business implication: 
```



# STAGE 6 — Sampling & A/B Test
## Tabs: `06_SAMPLING` and `07_AB_TEST` · 75 minutes



## 06_SAMPLING — Extracting a Clean Test Dataset (25 min)

Before running an A/B test, we need a well-prepared sample. This is the "experiment prep" stage.

### Step 6A — Why we sample here

We have ~300k rows. Running `TTEST` in Sheets on the full dataset would be slow and Sheets may struggle. More importantly, in a real pipeline, a data engineer would prepare a **curated extract** for the experiment team — only the columns and rows they need, in a clean format.

### Step 6B — Add a random key to `02_CLEAN`

Go to `02_CLEAN`. In the first empty column after your derived columns (column R), add the header `rand_key` and in R2:
```
=IF(Q2=TRUE, RAND(), "")
```
Fill down. This assigns a random number only to valid rows.

**Freeze the random numbers:** Select column R → Copy → Paste Special → Values only. This locks the random order.

### Step 6C — Build the experiment extract in `06_SAMPLING`

We want 3,000 rows: 1,500 classic bike trips and 1,500 electric bike trips, randomly selected.

1. Go to `02_CLEAN`
2. Filter: `is_valid = TRUE` AND `rideable_type = classic_bike`
3. Sort by `rand_key` ascending
4. Copy the first 1,500 rows (columns: `ride_id`, `rideable_type`, `member_casual`, `duration_min`, `hour_of_day`, `is_weekend` only)
5. Paste as values into `06_SAMPLING` starting at row 2
6. Repeat for electric bike (rows 1502–3001)
7. Add a header row: `ride_id | rideable_type | member_casual | duration_min | hour_of_day | is_weekend`

**Document in `PIPELINE_LOG`:**
```
[Stage 6] Experiment sample prepared
Classic bike rows: 1,500 (randomly selected from valid rows)
Electric bike rows: 1,500 (randomly selected from valid rows)
Selection method: RAND() frozen, sort ascending, take first N
Columns included: ride_id, rideable_type, member_casual, duration_min, hour_of_day, is_weekend
```


## 07_AB_TEST — The Experiment (50 min)

### Step 7A — Write the hypothesis (do this before looking at any numbers from `06_SAMPLING`)

```
EXPERIMENT: Classic vs Electric Bike Trip Duration
=================================================
H₀ (Null):       There is NO difference in mean trip duration between 
                 classic bike riders and electric bike riders.
                 Any observed difference is due to random chance.

H₁ (Alternative): Electric bike riders have [longer/shorter/different — choose]
                  mean trip durations compared to classic bike riders.

Significance level: α = 0.05
(This means: we will only reject H₀ if there is less than a 5% 
chance the result is due to random chance.)

My prediction:    I expect electric rides to be [longer/shorter] because...

Business decision this will inform:
[Write what Capital Bikeshare might decide based on this result —
fleet size, pricing, maintenance budget, station planning?]
```



### Step 7B — Descriptive stats for both groups

In `07_AB_TEST`, create a comparison table pulling from `06_SAMPLING`:

| Metric | Classic (Group A) | Electric (Group B) |
|--||-|
| n (sample size) | `=COUNTIF(06_SAMPLING!B:B,"classic_bike")` | |
| Mean duration | `=AVERAGEIF(06_SAMPLING!B:B,"classic_bike",06_SAMPLING!D:D)` | |
| Std Dev | `=STDEV(IF(06_SAMPLING!B2:B3001="classic_bike",06_SAMPLING!D2:D3001))` *(Ctrl+Shift+Enter)* | |
| Median | array formula with PERCENTILE+IF | |
| % > 30 min | COUNTIFS / COUNTIF * 100 | |
| % < 5 min | | |

**Observed difference:**
```
Absolute difference (min) = Mean(Classic) - Mean(Electric)
Relative difference (%)   = Absolute difference / Mean(Classic) * 100
```

### Step 7C — Side-by-side histogram

Build a frequency table with separate columns for classic and electric (same bin structure as Stage 3), using COUNTIFS on `06_SAMPLING`. Create a column chart. This is the visual representation of your A/B test groups.



### Step 7D — Run the TTEST

Prepare two single columns of duration values (you already have them in `06_SAMPLING` — classic is rows 2–1501, electric is rows 1502–3001).

In an empty cell:
```
=TTEST(06_SAMPLING!D2:D1501, 06_SAMPLING!D1502:D3001, 2, 3)
```

Arguments:
- Range 1: classic bike durations
- Range 2: electric bike durations  
- `2` = two-tailed (we test for any difference, not just one direction)
- `3` = Welch's t-test (two samples, unequal variance — the safest default)

Record your p-value. Then write:

```
p-value result: ___________

Plain English interpretation:
"If there were truly no difference between classic and electric bike 
trip durations, we would see a difference this large or larger by 
chance [X% / less than 5% / etc.] of the time.

Therefore, we [REJECT / FAIL TO REJECT] H₀.

The difference IS / IS NOT statistically significant at α = 0.05."
```

### Step 7E — Effect size (practical significance)

A statistically significant result only tells you the difference is real. Effect size tells you whether it is *worth acting on*.

Calculate manually:

```
Pooled Std Dev = SQRT((StdDev_Classic^2 + StdDev_Electric^2) / 2)
Cohen's d      = Absolute difference / Pooled Std Dev
```

In Sheets:
```
Pooled SD = =SQRT((C5^2 + D5^2)/2)   [where C5, D5 are your std dev cells]
Cohen's d  = =[absolute difference cell] / [pooled SD cell]
```

| Measure | Value | Interpretation |
||-|-|
| Absolute difference (min) | | Meaningful in context? |
| Relative difference (%) | | |
| p-value | | Significant if < 0.05 |
| Cohen's d | | Small <0.2 / Medium 0.2–0.5 / Large >0.5 |
| Overall verdict | | Real AND important / Real but minor / Not conclusive |



### Step 7F — Confound check

Remember from Stage 5: casual riders both ride longer AND may prefer electric bikes. That means the apparent classic-vs-electric difference might actually be a *casual-vs-member* difference in disguise. This is called a **confounding variable**.

Check it:

| Comparison | Classic Mean | Electric Mean | Difference |
|--|-|--|--|
| All riders | | | |
| Members only | `=AVERAGEIFS(D:D,B:B,"classic_bike",C:C,"member")` | | |
| Casual only | `=AVERAGEIFS(D:D,B:B,"classic_bike",C:C,"casual")` | | |

Answer: When you control for rider type (look at members only, then casual only), does the classic vs electric difference still exist? Is it larger, smaller, or reversed? Write 3 sentences.

**Log in `PIPELINE_LOG`:**
```
[Stage 7] A/B test complete
Test: Classic vs Electric bike trip duration
n per group: 1,500
p-value: 
Cohen's d: 
Verdict: 
Confound check: rider type [does / does not] explain the difference
```



# STAGE 7 — Dashboard
## Tab: `08_DASHBOARD` · 50 minutes



The dashboard is the **output layer** of your pipeline — the one tab a business user would bookmark. It should tell the full story at a glance, pulling from all upstream tabs, and never contain raw data or complex formulas of its own.

> **Data engineering rule #5:** Dashboards display — they don't compute. Every number on the dashboard should be a simple reference (`=04_STATS!C5`) to a cell that was already computed upstream.

### Step 7A — Structure your dashboard

Design it in three sections, stacked top to bottom:



**Section 1 — Pipeline Health** (top strip, 5 rows)

| Label | Value |
|-|-|
| Data source | Capital Bikeshare — September 2023 |
| Raw rows ingested | `=COUNTA(00_RAW!A:A)-1` |
| Valid rows (after cleaning) | `=03_AGGREGATE!C3` |
| Data quality pass rate | `=02_CLEAN summary cell` |
| Last updated | `=[your timestamp from 00_RAW!P2]` |

Use a light grey background for this strip. It tells anyone opening the file: *"Here is where the data came from and how clean it is."*



**Section 2 — Key Metrics** (middle section)

Create 6 "metric cards" — each is just a cell with a large bold number and a small label below it. Reference `03_AGGREGATE`:

```
Total Trips          |  % Member Riders     |  % Electric Bikes
[=03_AGGREGATE!C3]   |  [formula]           |  [formula]

Mean Trip Duration   |  Most Popular Hour   |  Weekend Trip Share
[=04_STATS ref]      |  [=from hourly tab]  |  [formula]
```

To make numbers visually large: select the value cell → Format → Font size → 24 or 28. Bold. Use a dark colour.  
Label cells below each: font size 9, grey, italic.



**Section 3 — Experiment Results** (bottom section)

A clean summary of the A/B test. Again, all values reference `07_AB_TEST`:

```
A/B TEST: Classic vs Electric Bike Trip Duration
─────────────────────────────────────────────────
Classic mean:   [ref]     Electric mean:  [ref]
Difference:     [ref]     p-value:        [ref]
Effect size:    [ref]     Significant?:   [ref]

RECOMMENDATION:
[Type your 2-sentence recommendation here directly — 
 this is the one thing on the dashboard that is hand-written,
 because it requires human judgement, not a formula.]
```



### Step 7B — Add two charts

Copy (do not recreate) the two most important charts from upstream tabs into this dashboard:

1. Your **duration distribution histogram** from `04_STATS` — paste as a linked image (right-click chart → Copy chart → paste into dashboard)
2. Your **A/B test side-by-side histogram** from `07_AB_TEST`



### Step 7C — Dashboard design rules

Apply these formatting choices — they matter in a real data job:

- No gridlines visible (View → uncheck Gridlines)
- Column A is narrow (20px) — acts as a left margin
- Section headers use a coloured background band (dark blue or dark green text on very light colour)
- All numbers formatted consistently (1 decimal place for durations, whole numbers for counts, % symbol for percentages)
- No raw data visible anywhere on this tab

**Log in `PIPELINE_LOG`:**
```
[Stage 7] Dashboard complete
Sections: Pipeline Health, Key Metrics, Experiment Results
All values reference upstream tabs — no independent computations
Charts: duration histogram (from 04_STATS), A/B test histogram (from 07_AB_TEST)
```


# STAGE 8 — Pipeline Review & Documentation
## Tab: `PIPELINE_LOG` · 40 minutes



You've been adding notes throughout the day. Now write the final sections.

### Final Log Entry — Pipeline Summary

```
=====================================================
PIPELINE: Capital Bikeshare Analytics Pipeline
BUILT BY: [your name]
DATE: [today]
=====================================================

DATA FLOW:
00_RAW → 01_SCHEMA → 02_CLEAN → 03_AGGREGATE
                                      ↓
                   04_STATS ← 03_AGGREGATE → 05_PROBABILITY
                                      ↓
                             06_SAMPLING → 07_AB_TEST
                                                ↓
                                          08_DASHBOARD
=====================================================

STAGE SUMMARY:
[Stage 1] Raw rows ingested: ___
[Stage 2] Valid rows after cleaning: ___ (___%)
[Stage 3] Aggregation: ___ segments built
[Stage 4] Key stat finding: ___
[Stage 5] Key probability finding: ___
[Stage 6] Sample size per A/B group: 1,500
[Stage 7] A/B test result: p=___, Cohen's d=___, verdict=___
[Stage 8] Dashboard: complete

DATA QUALITY ISSUES FOUND:
1. 
2. 
3. 

DECISIONS MADE (and why):
- Cleaning rule (duration 1–180 min): chosen because ___
- Sampling method (RAND() frozen): chosen because ___
- Test type (two-tailed Welch's): chosen because ___

LIMITATIONS OF THIS PIPELINE:
1. 
2. 
3. 

IF I HAD MORE TIME / A REAL DATABASE, I WOULD:
1. 
2. 
=====================================================
```

### Final reflection questions

Answer these in `PIPELINE_LOG` as your closing entry:

1. **The pipeline advantage:** Before today, you ran stats and formulas directly on raw data. What is the advantage of the layered pipeline approach — what would happen if the source CSV was updated with a new month of data?

2. **The immutable source rule:** Why is it important that `00_RAW` is never edited? What could go wrong if you allowed modifications to the raw data tab?

3. **Stats as a consumer:** In Week 3 you learned descriptive stats, probability, and A/B testing as separate topics. How does today change how you think about them — are they separate skills or stages of the same workflow?

4. **What you'd automate:** If you were moving this pipeline from Sheets into a real data engineering tool (like dbt, Airflow, or even Python), which stage would you automate first and why?





## ✅ Full Project Deliverables Checklist

### Tabs
- [ ] `00_RAW` — raw CSV imported, timestamp recorded
- [ ] `01_SCHEMA` — schema registry complete, quality audit done
- [ ] `02_CLEAN` — 5 derived columns added, `is_valid` flag, cleaning summary
- [ ] `03_AGGREGATE` — 4 sections: overall, duration by segment, hourly, buckets
- [ ] `04_STATS` — five-number summary, distribution chart, coefficient of variation
- [ ] `05_PROBABILITY` — probability table, independence test, joint distribution matrix
- [ ] `06_SAMPLING` — 3,000 row extract (1,500 per group), random selection documented
- [ ] `07_AB_TEST` — hypothesis written, TTEST run, effect size calculated, confound check done
- [ ] `08_DASHBOARD` — 3 sections, all values from upstream, no gridlines, 2 charts
- [ ] `PIPELINE_LOG` — all stage entries complete, final summary written

### Questions & Write-ups
- [ ] 3 data quality observations (Stage 1)
- [ ] Distribution shape described in plain English (Stage 4)
- [ ] Independence test finding written in 3 sentences (Stage 5)
- [ ] p-value interpreted in plain English (Stage 7)
- [ ] Effect size categorised and business verdict written (Stage 7)
- [ ] Confound check answered in 3 sentences (Stage 7)
- [ ] 2-sentence recommendation on dashboard (Stage 7)
- [ ] 4 final reflection questions answered (Stage 8)



## 💡 Bonus Challenges

**If you finish early — pick any one:**

1. **Add a third segment to the A/B test:** Re-run the TTEST comparing *weekday* vs *weekend* trips. Does the time of riding affect duration more or less than bike type?

2. **Catch Simpson's Paradox:** Re-run the classic vs electric mean comparison separately for members and casual. Does the direction of the difference flip between groups? If so, you've found Simpson's Paradox — document it in `PIPELINE_LOG` and explain what it means in plain English.

3. **Hourly peak analysis:** Using your `03_AGGREGATE` hourly table, identify the top 3 peak hours and the 3 quietest hours. Calculate how much longer or shorter trips are during peak hours vs off-peak. Add a "Peak Analysis" section to `08_DASHBOARD`.



> Statistics, probability, and A/B testing are not isolated techniques. They are **layers in a pipeline** — each one depends on the quality and structure of what came before it. A beautiful A/B test built on uncleaned, undocumented data is worthless. A boring, well-documented cleaning stage makes every analysis downstream trustworthy. That is data engineering.
