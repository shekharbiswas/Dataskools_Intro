# Spreadsheet ETL Guide: Clean, Explore, Reflect
## Lina's Lemonade Stand — With a Missing Data Decision Framework

> **Dataset:** Lina's Lemonade Stand (1,000 transactions · 25 columns)
> **Tool:** Google Sheets only
> **Core skill this session builds:** Not just *finding* missing data — but making and documenting a defensible decision about what to do with each gap

---

## Before You Touch Any Formula

Open the **Missing Data Method Reference** sheet (the 10-method table). Keep it visible in a second tab or a second window throughout this entire session.

Every time you find a missing value today, your job is not to fill it immediately. Your job is to answer three questions first:

```
1. WHY is this value missing? (MCAR / MAR / MNAR — or unknown)
2. WHICH of the 10 methods is appropriate for this column and this reason?
3. WHAT would happen to my analysis if I chose the wrong method?
```

Only after answering those three questions do you fill — or deliberately choose not to fill.

> **The key principle from the method table:**
> *"The choice of imputation method is not a data cleaning decision, it is an analytical decision. The wrong method does not just fill gaps — it manufactures data that changes means, correlations, and p-values."*

Add a tab to your spreadsheet called `IMPUTATION_LOG`. Every decision you make today goes in there. Format:

| Column | Method Used | Reason for Missing | Why This Method | Rows Affected |
|--------|-------------|-------------------|-----------------|---------------|
| `tip_amount` | | | | |
| `cups_sold` | | | | |
| `customer_rating` | | | | |

You will fill this log as you work through the steps below.

---

## Your Dataset — Columns to Know

Before touching any formula, open the spreadsheet and read every column header.

| Column | What it contains |
|--------|-----------------|
| `TRANSACTION ID` | Unique ID per sale |
| `DATE` | Sale date (YYYY-MM-DD) |
| `TIMESTAMP` | Exact date and time of transaction |
| `DAY OF WEEK` | Monday–Sunday |
| `IS WEEKEND` | TRUE or FALSE |
| `MONTH` | June / July / August |
| `HOUR OF DAY` | Hour the sale happened (8–18) |
| `LOCATION` | Which of 5 stand locations |
| `FLAVOR` | Which of 6 lemonade flavors |
| `CUPS SOLD` | Number of cups per transaction |
| `PRICE PER CUP` | Price after any promotions |
| `REVENUE` | Total money from that transaction |
| `COGS` | Cost of goods (lemons, sugar, cups) |
| `GROSS PROFIT` | Revenue minus cost |
| `TIP AMOUNT` | Tip received |
| `WEATHER` | Sunny / Partly Cloudy / Cloudy / Rainy |
| `TEMP F` | Temperature in Fahrenheit |
| `MORNING SUN AFTER RAIN` | TRUE if yesterday was rainy and today is sunny |
| `ICE LBS USED` | Pounds of ice used |
| `LEMONS USED` | Lemons used per transaction |
| `PROMOTION` | Promotion type applied (if any) |
| `PAYMENT METHOD` | Cash / Card / Mobile Pay |
| `CUSTOMER TYPE` | New / Returning / Regular |
| `WAIT TIME MIN` | Minutes the customer waited |
| `CUSTOMER RATING` | Customer satisfaction score (1.0–5.0) |

> **Before you start:** Scroll through the data. You will notice some cells are highlighted in red. Do not fill them yet. First, understand *why* they are empty.

---

## Step 1 — Extract Your Data

- Open [Google Sheets](https://sheets.google.com)
- Go to `File > Import > Upload` and upload the Excel file
- Choose **"Insert new sheet(s)"** — do not replace the current sheet

**Why this matters:** Inserting instead of replacing preserves your original data. Treat raw data as your source of truth — never edit it directly. This is the same principle as `00_RAW` in a pipeline: the source is immutable.

❓ **Investigate:** If you import the file and later realise you made changes to the raw sheet, how would you recover the original? Which of the 10 methods, if applied to the wrong column, could cause damage that would be impossible to reverse without going back to the raw source?

---

## Step 2 — Audit All Missing Values First

Before doing anything else, build a complete missing value audit. This tells you *where* the problems are and — more importantly — *how many* are missing, which determines which methods are even available to you.

In a new section or tab, run `COUNTBLANK` on every column with suspected gaps:

```
=COUNTBLANK(J2:J1001)   ← CUPS SOLD
=COUNTBLANK(O2:O1001)   ← TIP AMOUNT
=COUNTBLANK(Q2:Q1001)   ← TEMP F
=COUNTBLANK(U2:U1001)   ← PROMOTION
=COUNTBLANK(Y2:Y1001)   ← CUSTOMER RATING
```

Then calculate the **% missing** for each:
```
=COUNTBLANK(J2:J1001)/COUNTA(A2:A1001)*100
```

Record everything in your `IMPUTATION_LOG`.

### What the % missing tells you about your method options

| % Missing | What it rules out |
|-----------|------------------|
| < 2% | Almost any method works — Method 10 (Drop) is safe here |
| 2–10% | Mean/Median imputation (Method 1) acceptable as a baseline |
| 10–30% | Method 1 compresses variance — use Method 4, 6, or 7 instead |
| > 30% | Method 8 (Multiple Imputation) or Method 5 (Flag) — never drop, never use simple mean |

❓ **Investigate:** `PROMOTION` has more than 500 missing values — over half the column. Before assuming this means "no promotion was running," what else could explain that many blanks? Does Method 10 (Drop) make any sense here? Name the method that would let you keep the information that something *was* missing, rather than pretending the gap doesn't exist.

---

## Step 3 — Classify Each Gap: MCAR, MAR, or MNAR

For each column with missing values, you need to classify the *type* of missingness before choosing a method. This is the most important step in the entire session.

Run these cross-checks to test for patterns:

**Is `CUPS SOLD` missingness random?**
```
=COUNTIFS(P2:P1001,"Rainy",J2:J1001,"")    ← blank cups on rainy days
=COUNTIFS(P2:P1001,"Sunny",J2:J1001,"")    ← blank cups on sunny days
```

**Is `TIP AMOUNT` missingness linked to payment method?**
```
=COUNTIFS(V2:V1001,"Cash",O2:O1001,"")     ← blank tips on cash transactions
=COUNTIFS(V2:V1001,"Card",O2:O1001,"")     ← blank tips on card transactions
```

**Is `CUSTOMER RATING` missingness linked to wait time?**
```
=COUNTIFS(X2:X1001,">"&15,Y2:Y1001,"")    ← blank ratings when wait > 15 min
=COUNTIFS(X2:X1001,"<="&5,Y2:Y1001,"")    ← blank ratings when wait ≤ 5 min
```

### What the three types mean for your method choice

| Type | Definition | Safe methods | Dangerous methods |
|------|-----------|-------------|------------------|
| **MCAR** | Missing Completely At Random — no pattern | Method 1, 10 | None particularly |
| **MAR** | Missing At Random — pattern linked to *another* column | Method 4, 6, 7 | Method 1, 10 |
| **MNAR** | Missing Not At Random — pattern linked to the *missing value itself* | Method 5, 8 | Method 1, 4, 10 |

Fill in the "Reason for Missing" column of your `IMPUTATION_LOG` for each column now.

❓ **Investigate:** `CUSTOMER RATING` is missing more often when `WAIT TIME MIN` is high. Without looking at definitions — is this MCAR, MAR, or MNAR? Customers who had long waits are less likely to leave a rating — does the missingness depend on another column (wait time, which you *can* see) or on the rating value itself (which you *cannot* see)? Why does the answer to that question change which imputation method is appropriate?

---

## Step 4 — Apply the Right Method to Each Column

Work through each column with missing values. For each one: classify → choose method → apply formula → log the decision.

---

### `TEMP F` — Forward Fill (Method 2)

**Why:** Temperature changes slowly throughout a day. A missing reading at 10 AM is almost certainly close to the reading at 9 AM. This is exactly the use case Method 2 describes: *"time-series data where values change slowly."*

**Check first:** Are the rows ordered by `TIMESTAMP`? If not, sort by `TIMESTAMP` before applying forward fill — Method 2 requires temporal order.

**Apply in Sheets:**
In a helper column next to `TEMP F`:
```
=IF(ISBLANK(Q2), Q1, Q2)
```
Fill down. This carries the last known temperature forward into each blank cell.

**Boundary check:** If `TEMP F` is blank for more than 3–4 consecutive rows (a sensor outage spanning hours), forward fill may produce values that are too stale. Flag those rows with Method 5 (add a `temp_was_filled` indicator column) so downstream analysis can exclude or weight them appropriately.

**Log entry:**
```
TEMP F | Method 2 (Forward Fill) | MAR — sensor gaps, short duration | 
Temporal ordering confirmed | [n] rows affected
```

❓ **Investigate:** You apply forward fill to `TEMP F` and find a 6-hour gap in the middle of a July afternoon. The temperature at 11 AM was 78°F. By 5 PM it had dropped to 68°F after a storm. Your forward fill put 78°F into all six missing hours. What does Method 2's warning — *"DO NOT USE when gaps are long and conditions could have changed dramatically"* — mean in practical terms here? What would Method 8 (Multiple Imputation) do differently?

---

### `TIP AMOUNT` — Domain-Rule Imputation (Method 9) + Indicator Flag (Method 5)

**Why Method 9 first:** The method table states it directly: *"tip_amount where payment_method = Cash → fill with 0.00 (cash transactions by design cannot have digital tips)."* This is not a statistical assumption — it is a business rule. Cash customers do not leave digital tips. The gap is not missing data; it is a known zero.

**Apply Method 9:**
```
=IF(AND(ISBLANK(O2), V2="Cash"), 0, O2)
```

**Why Method 5 next:** After applying Method 9 to cash rows, some `TIP AMOUNT` blanks remain — these belong to card and mobile pay transactions where the tip genuinely was not recorded. For these, do not fill with zero (that would understate the true tip rate). Instead, apply Method 5:

Add a column `TIP_WAS_MISSING`:
```
=IF(AND(ISBLANK(O2), V2<>"Cash"), 1, 0)
```

This flags the remaining genuine gaps without inventing a value. Downstream analysis can then report tip averages with and without these rows, showing the uncertainty honestly.

**Log entry:**
```
TIP AMOUNT | Method 9 then Method 5 | 
Cash rows → MCAR (by design, $0) | Non-cash rows → MNAR (tip amount unknown) |
Method 9 applied to [n] cash rows · Method 5 flag added for [n] non-cash rows
```

❓ **Investigate:** A classmate fills all missing `TIP AMOUNT` with zero — including the card transactions. Walk through what Method 1's warning says: *"it compresses variance and makes the column look less spread out than it really is."* In this specific case, does filling non-cash tips with zero make the average tip *higher* or *lower* than the truth? Does it make tipping *look* more or less common than it really is?

---

### `CUPS SOLD` — Group-Based Imputation (Method 4)

**Why:** You found that `CUPS SOLD` is missing more often on rainy days. This means missingness is linked to `WEATHER` — which is MAR, not MCAR. Method 1 (overall mean) would mix sunny-day averages into rainy-day gap fills, pulling the imputed values away from the true likely values. Method 4 fills each gap with the mean for that specific weather condition — a much more defensible choice.

**Build the group means first** in a separate reference section:
```
=AVERAGEIF(P2:P1001,"Sunny",J2:J1001)       → mean cups on Sunny days
=AVERAGEIF(P2:P1001,"Rainy",J2:J1001)       → mean cups on Rainy days
=AVERAGEIF(P2:P1001,"Cloudy",J2:J1001)      → mean cups on Cloudy days
=AVERAGEIF(P2:P1001,"Partly Cloudy",J2:J1001) → mean cups on Partly Cloudy days
```

**Apply Method 4:**
```
=IF(ISBLANK(J2),
  AVERAGEIF(P$2:P$1001, P2, J$2:J$1001),
  J2)
```
This fills each blank with the average cups sold under the same weather condition as that row.

**Can we go further?** Yes — Method 4 allows nesting: instead of just `WEATHER`, use `WEATHER + FLAVOR` to get an even more specific group mean. Use `AVERAGEIFS` for two conditions:
```
=IF(ISBLANK(J2),
  AVERAGEIFS(J$2:J$1001, P$2:P$1001, P2, I$2:I$1001, I2),
  J2)
```

**Log entry:**
```
CUPS SOLD | Method 4 (Group-Based — Weather condition) | 
MAR — missing more on rainy days | 
Group means used: Sunny/Rainy/Cloudy/Partly Cloudy | [n] rows affected
```

❓ **Investigate:** After applying Method 4, you compute the average `CUPS SOLD`. Compare it to what you would have got with Method 1 (overall mean fill). Is it higher, lower, or the same? Why does the difference matter if rainy days genuinely sell fewer cups?

---

### `CUSTOMER RATING` — Indicator Flag (Method 5)

**Why not fill this column at all:** Your cross-check showed that ratings are missing *more often* when wait times are long. This is MNAR — the missingness is linked to the value that would have been recorded (dissatisfied customers do not leave ratings). Method 1 would produce a mean from only the happier customers who *did* rate. Method 4 would do the same within groups. Any imputed value would be systematically too high.

Method 5 is the honest choice: do not invent a rating. Instead, record that it was missing.

**Apply Method 5:**

Add a column `RATING_WAS_MISSING`:
```
=IF(ISBLANK(Y2), 1, 0)
```

Fill `CUSTOMER RATING` blanks with the column mean *only for technical completeness* (some formulas break on blanks):
```
=IF(ISBLANK(Y2), AVERAGE(Y$2:Y$1001), Y2)
```

But in every downstream analysis, filter or weight using `RATING_WAS_MISSING = 0` to work with only genuine ratings — and report the missingness rate alongside any average you publish.

**Log entry:**
```
CUSTOMER RATING | Method 5 (Indicator Flag) | 
MNAR — missing more when wait time is high (dissatisfied non-response) |
Do NOT use imputed values in rating averages — use RATING_WAS_MISSING filter |
[n] rows flagged
```

❓ **Investigate:** You publish an average `CUSTOMER RATING` of 4.2 based on the non-missing rows. The true average (if all customers had rated) might be 3.6. Who is harmed if Lina makes a business decision based on 4.2? What one sentence would you add to any chart or report showing the average rating to make it honest?

---

### `PROMOTION` — Indicator Flag + Domain Rule (Methods 5 and 9)

**Why:** Over 500 rows have no `PROMOTION` value. Before filling anything, apply Method 9 first: if the `HOUR OF DAY` is between 14:00 and 15:00, fill as `"Happy Hour"` — this is a known business rule, not a guess.

```
=IF(AND(ISBLANK(U2), G2>=14, G2<15), "Happy Hour", U2)
```

For remaining blanks after the domain rule is applied: add a `PROMOTION_WAS_MISSING` flag (Method 5). Do not invent a promotion type for rows where you have no business rule to apply.

**Log entry:**
```
PROMOTION | Method 9 then Method 5 |
Happy Hour rows → domain rule (deterministic) |
Remaining blanks → MNAR or unknown, flagged only |
[n] domain-rule fills · [n] flagged
```

❓ **Investigate:** You apply the Happy Hour domain rule and fill 47 rows. A classmate asks: "How do you know those rows weren't actually running a different promotion?" What is the difference between a domain rule that is a *fact* and one that is an *assumption* — and why does Method 9 warn that *"applying wrong rules creates systematic bias worse than leaving the gap"*?

---

### `GROSS PROFIT` — Domain-Rule Imputation (Method 9)

**Why:** `GROSS PROFIT = REVENUE - COGS`. If both columns are present, the gap in `GROSS PROFIT` has a deterministic answer. No statistics needed.

```
=IF(AND(ISBLANK(N2), NOT(ISBLANK(L2)), NOT(ISBLANK(M2))), L2-M2, N2)
```

**Log entry:**
```
GROSS PROFIT | Method 9 (Domain Rule) | MCAR — calculation gap, not true missing |
Formula: REVENUE - COGS | Only applied where both inputs are present | [n] rows
```

---

## Step 5 — Validate Your Imputation Decisions

After filling, do not trust the results blindly. Run these checks.

### Check 1 — Distribution shape before and after

For `CUPS SOLD` and `TEMP F`, compare:
```
Before fill: =AVERAGE(J2:J1001)   =STDEV(J2:J1001)
After fill:  [same formulas on your helper column]
```

If the standard deviation *dropped significantly* after filling, you may have over-applied Method 1 or used group means that are too similar — compressing the natural variability in the data. This is the "variance compression" warning from Method 1.

### Check 2 — Verify REVENUE integrity

`REVENUE` should equal `CUPS SOLD × PRICE PER CUP`. After imputing `CUPS SOLD`, check whether any newly filled values create inconsistencies:
```
=IF(ISBLANK(K2), "SKIP", ROUND(J2*K2,2))
```
Compare to `REVENUE`. Flag mismatches.

### Check 3 — Sanity check the flag columns

Count how many rows are flagged:
```
=COUNTIF(Z2:Z1001, 1)   ← RATING_WAS_MISSING
=COUNTIF(AA2:AA1001, 1)  ← TIP_WAS_MISSING
```

These counts should match your original `COUNTBLANK` results (adjusted for domain-rule fills). If they don't, you have a formula gap somewhere.

❓ **Investigate:** After imputation, you compute the average `CUSTOMER RATING` two ways: (a) including imputed mean-fills, (b) filtering on `RATING_WAS_MISSING = 0`. The results are 4.2 and 4.2 — identical. Does that mean Method 5 was unnecessary? Or does it mean something else? (Hint: what does it tell you about how the mean-fill was calculated?)

---

## Step 6 — Summary Statistics on Clean Data

Now that every column has a documented imputation decision, compute your summary statistics. These are now trustworthy enough to use — with the caveats you've documented.

For `CUPS SOLD` (your imputed helper column):
```
=AVERAGE([helper column])
=MEDIAN([helper column])
=STDEV([helper column])
=MIN([helper column]) / =MAX([helper column])
```

For `CUSTOMER RATING` — use only genuine ratings:
```
=AVERAGEIF(Z2:Z1001, 0, Y2:Y1001)   ← average where RATING_WAS_MISSING = 0
```

❓ **Investigate:** Your mean `CUPS SOLD` is higher than the median. What does the mean–median gap tell you about the shape of the distribution? Now consider: you filled missing `CUPS SOLD` values using weather-group means (Method 4). Could the imputation itself have *created* or *changed* the skew — not just described it? How would you test this?

---

## Step 7 — Visualise With Honest Labels

Build three charts. Each one must acknowledge the imputation decisions behind it.

### Chart 1 — Bar chart: Average cups sold by weather condition

Use `AVERAGEIF` on your imputed `CUPS SOLD` column.

Add a text note below or beside the chart:
> *"[n] missing CUPS SOLD values were filled using the mean for each weather condition (Method 4 — Group-Based Imputation). Rainy-day averages may be understated if missing values skewed toward low-traffic periods."*

### Chart 2 — Bar chart: Average customer rating by flavor

Use `AVERAGEIF` on `CUSTOMER RATING` **filtered by `RATING_WAS_MISSING = 0`**.

Add a note:
> *"[n] ratings excluded (missing — MNAR pattern: higher missingness when wait time > 15 min). Displayed averages likely overstate true satisfaction."*

### Chart 3 — Line chart: Daily total revenue

Use a pivot table: `DATE` → `SUM of REVENUE`.

Add a note:
> *"[n] transactions have missing REVENUE and are excluded from daily totals. If missing revenue rows are concentrated on high-traffic days, totals may be understated."*

❓ **Investigate:** Chart 2 shows `Lavender` with the highest average rating. Before telling Lina to make more Lavender, check: (a) how many Lavender transactions exist, (b) what the `RATING_WAS_MISSING` rate is specifically for Lavender, (c) whether Lavender is more often purchased by `Regular` customers who rate more generously. What is the risk of making a stock decision based on a subgroup with a high missing-rating rate?

---

## Step 8 — Build an Honest Dashboard

Create a new sheet called `DASHBOARD`. This is the one page anyone opens to understand Lina's business.

**What to include:**

- **KPI strip:** Total cups sold · Total revenue · Average rating (genuine only) · Best location
- **Bar chart:** Average cups sold by weather (imputed, Method 4)
- **Line chart:** Daily revenue (with missing-row caveat)
- **Pie chart:** Revenue share by flavor
- **Data quality strip at the bottom** — one row per imputed column:

| Column | Method Used | Rows Affected | Caveat |
|--------|-------------|---------------|--------|
| `TEMP F` | Forward Fill (M2) | [n] | Long gaps (>3 hrs) may be stale |
| `CUPS SOLD` | Group Mean (M4) | [n] | Rainy-day fills use weather-group average |
| `TIP AMOUNT` | Domain Rule + Flag (M9+M5) | [n]+[n] | Non-cash tip gaps flagged, not filled |
| `CUSTOMER RATING` | Flag only (M5) | [n] | MNAR — averages use genuine ratings only |
| `PROMOTION` | Domain Rule + Flag (M9+M5) | [n]+[n] | Happy Hour rule applied; remainder flagged |
| `GROSS PROFIT` | Domain Rule (M9) | [n] | Revenue − COGS where both present |

> This data quality strip is what separates a professional dashboard from a student exercise. It tells anyone who reads the dashboard exactly how trustworthy each number is — and where the gaps remain.

❓ **Investigate:** Your dashboard shows "Total Revenue: $4,820." A classmate looks at it and says "Great — Lina made $4,820." Write the one sentence you would add to the dashboard to make that number honest, given what you know about the missing revenue rows and the imputation decisions you made.

---

## Step 9 — Complete Your IMPUTATION_LOG

Go back to the `IMPUTATION_LOG` tab and fill in every row. By the end of this session it should look something like this:

| Column | Method Used | Reason for Missing | Why This Method | Rows Affected |
|--------|-------------|-------------------|-----------------|---------------|
| `TEMP F` | M2 Forward Fill | MAR — sensor outage gaps, short duration, time-ordered | Sensor values change slowly; temporal structure valid | [n] |
| `CUPS SOLD` | M4 Group Mean (Weather) | MAR — more missing on rainy days | MAR requires group fill; overall mean would bias rainy estimates upward | [n] |
| `TIP AMOUNT` (cash) | M9 Domain Rule | By design — cash = no digital tip | Deterministic rule, not statistical assumption | [n] |
| `TIP AMOUNT` (non-cash) | M5 Flag | MNAR — gap amount unknown | Imputing would manufacture unknown values; flag preserves uncertainty | [n] |
| `CUSTOMER RATING` | M5 Flag | MNAR — unhappy customers don't rate | Any imputed value would be systematically too high | [n] |
| `PROMOTION` (14–15h) | M9 Domain Rule | Known Happy Hour business rule | Deterministic fill, verified by business logic | [n] |
| `PROMOTION` (other) | M5 Flag | Unknown — insufficient evidence for rule | Cannot safely impute promotion type without a rule | [n] |
| `GROSS PROFIT` | M9 Domain Rule | Calculation gap — both inputs present | Revenue − COGS is exact, not statistical | [n] |

This log is your evidence that every decision was deliberate. In a real data team, this document would be reviewed before any analysis is published.

---

## Self-Reflection Prompts

Answer these in your `IMPUTATION_LOG` or a separate journal entry. Write in full sentences.

1. You applied six different imputation methods to six different columns in the same dataset. What was the deciding factor each time — the *type of missingness*, the *percentage missing*, or the *column's relationship to other columns*? Was it ever more than one factor at once?

2. `CUSTOMER RATING` and `TIP AMOUNT` both ended up as Method 5 flags rather than filled values. But the reasons were slightly different. Explain the difference in your own words — without using the words MCAR, MAR, or MNAR.

3. You used Method 4 (Group-Based) for `CUPS SOLD` instead of Method 1 (Overall Mean). Calculate both and compare the results for rainy-day rows. How much does the choice of method change the imputed value? Does the difference matter for Lina's business decisions?

4. Lina asks: "Which flavor should I stop selling?" What would a lazy analyst answer using the dashboard as-is? What would a careful analyst check first, given what you now know about `CUSTOMER RATING`'s missing data pattern?

5. If you presented this dashboard to Lina without showing her the `IMPUTATION_LOG` — would that be dishonest? Does it depend on what decision she's making?

6. Which of the 10 methods would you most want to apply if you had Python available instead of Google Sheets — and why? What does Sheets make difficult that a programming language would make straightforward?

7. You found that forward-filling `TEMP F` over a 6-hour gap produced values that were wrong by the afternoon. What does this tell you about the relationship between *method appropriateness* and *gap size* — not just data type?

8. If Lina hired you as her data analyst full-time, what is the single most important data *collection* habit you would introduce to reduce the type of missingness you found today — before it becomes an imputation problem?

---

## Summary

By completing this session, you have run a complete ETL workflow — with a missing data decision layer that most beginner analysts skip entirely:

- You **extracted** and protected your raw data source
- You **classified** each gap as MCAR, MAR, or MNAR before touching it
- You **chose** a method from the 10-method framework based on the type of missingness, the percentage missing, and the column's analytical role
- You **applied** six different methods across six columns — none of them identical
- You **documented** every decision in an `IMPUTATION_LOG` that makes your work auditable and reproducible
- You **visualised** and **dashboarded** with honest caveats attached to every imputed metric

The hardest part of data work is not the formulas. It is resisting the temptation to fill every gap with the same method — or to fill gaps at all when flagging them is the more honest choice. Every chart you built today sits on top of decisions you made about missing data — and those decisions change the story the data tells.

> *"The wrong method does not just fill gaps — it manufactures data that changes means, correlations, and p-values."*
> Always document which method was used and why.