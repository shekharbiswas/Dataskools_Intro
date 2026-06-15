# Day 2 — Visual Data Storytelling
## Capital Bikeshare: Turning Pipeline Output into a Presentation-Ready Story

> **Built on:** The pipeline you completed on Day 1 (`03_AGGREGATE`, `04_STATS`, `07_AB_TEST`)
> **Tool:** Google Sheets charts + Canva or Google Slides
> **Duration:** 6 hours
> **Difficulty:** ⭐⭐⭐
> **Feeds into:** Day 5 Presentation — this is where your visuals come from

---

## 🧭 The Big Picture

Yesterday you built a pipeline. Today you make it speak.

A pipeline full of clean data and correct statistics means nothing if no one can understand it. The last mile of any data project is **communication** — translating numbers into a visual that a business stakeholder, a classmate, or a hiring manager can look at for 10 seconds and immediately grasp.

Real-world data teams spend as much time on this layer as on the analysis itself. Today you will build the visuals that will anchor your Day 5 presentation. Every chart you create should come from your pipeline — not made up, not illustrative, but real numbers from real data you processed yourself.

By the end of today you will have:
- 4–5 presentation-ready charts pulled from your Capital Bikeshare pipeline
- A one-slide "insight card" per chart — visual + headline + 2-sentence takeaway
- All assets exported and ready to drop into your Day 5 presentation deck

---

## 🗺️ Today's Plan

| Stage | Task | Time |
|-------|------|------|
| Stage 1 | Choose your story | 30 min |
| Stage 2 | Build your charts in Google Sheets | 90 min |
| Stage 3 | Polish in Canva or Google Slides | 90 min |
| Stage 4 | Write your insight cards | 45 min |
| Stage 5 | Export and organise for Day 5 | 30 min |
| Stage 6 | Peer review (optional) | 30 min |

---

## Stage 1 — Choose Your Story (30 min)

Before opening a chart editor, answer this question in one sentence:

> **"What is the single most interesting thing my pipeline revealed?"**

Write your answer in a cell at the top of a new tab in your pipeline spreadsheet called `09_STORY`. This sentence will become the headline of your Day 5 presentation.

Then choose **one narrative arc** from the three below. Your charts today should all support that arc — not contradict each other or jump between topics.

### Narrative Arc Options

**Arc A — The Rider Gap**
> "Members and casual riders use the bikeshare system in fundamentally different ways."

Charts to build: mean duration by rider type · trip count by rider type · hourly distribution for members vs casual · weekend vs weekday split by rider type

**Arc B — The Bike Type Question**
> "Electric bikes changed how long people ride — and the A/B test tells us whether that difference is real."

Charts to build: mean duration by bike type · A/B test histogram (classic vs electric) · rider type × bike type joint distribution · p-value and effect size summary card

**Arc C — The City's Rhythm**
> "The bikeshare data is a map of how Washington DC moves — by hour, by day, by trip length."

Charts to build: trips by hour (line or bar) · trips by day of week · duration distribution histogram · peak vs off-peak comparison

> 💡 **Pick one and commit.** A focused story with 4 strong charts beats a scattered story with 8 weak ones. Your audience on Day 5 will follow one clear thread far better than three interleaved ideas.

Log your choice in `PIPELINE_LOG`:
```
[Day 2 — Stage 1] Narrative arc chosen: Arc [A / B / C]
Story headline: 
Reason for choice: 
```

---

## Stage 2 — Build Your Charts in Google Sheets (90 min)

All charts reference pre-computed values in `03_AGGREGATE`, `04_STATS`, or `07_AB_TEST`. You should not be running new COUNTIFS on raw data. If a number you need doesn't exist in those tabs, add it to `03_AGGREGATE` first.

> **Pipeline rule carried forward:** Charts are downstream consumers. If the source data changes, the chart updates automatically — because it points at the aggregate layer, not at raw rows.

### Chart 1 — Bar Chart: Comparison Across a Category

**Best for:** Arc A (rider type) or Arc B (bike type)

**Data source:** `03_AGGREGATE` — Duration by Segment table

**How to build:**
1. In `09_STORY`, create a clean 2-column table: Segment | Mean Duration (min)
   Pull values with simple `=` references to `03_AGGREGATE` — do not re-type numbers
2. Select both columns → Insert → Chart → Column chart
3. Chart title: something specific, not generic
   - ❌ "Mean Duration by Segment"
   - ✅ "Casual riders take trips 2× longer than members on average"
4. Axis label: Y-axis = "Mean trip duration (minutes)", X-axis = "Rider type"
5. Remove the legend if there is only one data series — it adds noise

**Formatting checklist:**
- [ ] Title answers a question or states a finding (not just a description)
- [ ] Both axes labelled with units
- [ ] No more than 6 bars — if you have more, group or filter
- [ ] Bars sorted (longest to shortest or logical order — not alphabetical)
- [ ] Gridlines minimal — 3–4 horizontal lines maximum

---

### Chart 2 — Line Chart: Pattern Over Time or Hour

**Best for:** Arc C (city rhythm) or Arc A (hourly behaviour gap)

**Data source:** `03_AGGREGATE` — Trips by Hour table

**How to build:**
1. In `09_STORY`, copy the Hour | Trip Count table from `03_AGGREGATE` using `=` references
2. If doing Arc A, add two more columns: `Member Trips (by hour)` and `Casual Trips (by hour)` — add these to `03_AGGREGATE` first using COUNTIFS, then reference here
3. Insert → Chart → Line chart
4. If two lines (members vs casual), keep them clearly distinct: different colours, different line weights, direct labels on the lines rather than a legend if possible

**Title framing:**
- ❌ "Trips by Hour of Day"
- ✅ "Ridership peaks at 8 AM and 5 PM — members drive the commute pattern"

**Formatting checklist:**
- [ ] X-axis shows readable hour labels (0, 2, 4... or "12 AM", "6 AM" etc.)
- [ ] Y-axis starts at 0 — never truncate a line chart Y-axis
- [ ] If two lines: label each line directly near its endpoint, or use a minimal legend
- [ ] Title states the insight, not just the variables

---

### Chart 3 — Histogram: Distribution Shape

**Best for:** All arcs — duration distribution is always interesting

**Data source:** `03_AGGREGATE` — Duration Buckets table

**How to build:**
1. In `09_STORY`, reference the Bucket Label | Count columns from `03_AGGREGATE`
2. Insert → Chart → Column chart (histograms in Sheets are just column charts with no gaps)
3. To remove gaps between bars: Edit chart → Customize → Series → set "Space between bars" to 0%
4. Add a vertical reference line for the mean: this requires a second data series
   - Add a column `Mean Line` where only the mean's approximate bucket row has a value equal to the chart's Y max — this creates a visual anchor
   - Or annotate manually in Canva after export

**What to highlight:**
- The right skew: most trips are short, a long tail stretches right
- The difference between mean and median (you calculated this in `04_STATS`)
- Any unexpected spike or gap in the distribution

**Formatting checklist:**
- [ ] No gaps between bars (this makes it look like a real histogram, not a bar chart)
- [ ] X-axis labels show the bucket ranges, not numbers
- [ ] Mean or median marked visually (line, annotation, or arrow)
- [ ] Title names the shape: "Most trips are under 15 minutes — but a long tail tells a different story"

---

### Chart 4 — A/B Test Results Visual

**Best for:** Arc B — required if you chose Arc B, strongly recommended for all arcs

**Data source:** `07_AB_TEST`

This is not a standard chart type — it is a **results card**. Build it as a formatted table in Google Sheets, then bring it into Canva for final polish.

Structure:

| | Classic Bike | Electric Bike |
|--|--|--|
| Mean duration | [ref] | [ref] |
| Std deviation | [ref] | [ref] |
| Sample size | 1,500 | 1,500 |
| **Difference** | **[ref] min** | |
| **p-value** | **[ref]** | |
| **Cohen's d** | **[ref]** | |
| **Verdict** | **Significant / Not significant at α = 0.05** | |

In Canva: use a two-column layout. Left side = the numbers table. Right side = one large sentence in bold: "Electric bike riders take trips X minutes longer on average — and this difference is statistically real." or "The difference we observed could easily be due to chance."

---

### Chart 5 — Optional: Joint Distribution Heatmap

**Best for:** Arc A or Arc B — shows the rider type × bike type intersection

**Data source:** `05_PROBABILITY` — Joint probability table

Build a 2×2 grid in Sheets using conditional formatting (Format → Conditional formatting → Colour scale) on your joint probability values. Darker = higher probability. This turns a table of numbers into a visual heat map with no chart tools needed.

Export as image for the presentation.

---

## Stage 3 — Polish in Canva or Google Slides (90 min)

Your Sheets charts are accurate but plain. This stage is about giving them the visual quality that makes a presentation feel professional.

### Export your charts from Sheets

For each chart: click the three-dot menu in the top right corner of the chart → Download → PNG

Save with clear names: `chart1_rider_duration.png`, `chart2_hourly_pattern.png` etc.

### Build one "Insight Card" per chart in Canva or Google Slides

An insight card has four elements:

```
┌─────────────────────────────────────────────────┐
│  HEADLINE (bold, large — states the finding)     │
│                                                  │
│  [Your chart — takes ~60% of the card space]     │
│                                                  │
│  Takeaway text (2 sentences — see Stage 4)       │
│                                                  │
│  Source: Capital Bikeshare, Sep 2023 · n=XXXX    │
└─────────────────────────────────────────────────┘
```

**Canva: how to get there**
1. canva.com → Create a design → Presentation (16:9)
2. One slide per insight card
3. Upload your chart PNG: Uploads → Upload files → drag the chart in
4. Resize to fill ~60% of the slide
5. Add a text box above: 28–32pt bold for the headline
6. Add a text box below: 14–16pt regular for the takeaway

**Design rules — keep it simple:**

| Element | Rule |
|---------|------|
| Background | White or very light grey (#F8F8F8) — not dark |
| Headline font | Any bold sans-serif (Montserrat Bold, Inter Bold, DM Sans Bold) |
| Body font | Same family, regular weight |
| Accent colour | One colour only — pick from the chart colours so everything matches |
| Text size | Headline ≥ 28pt · Takeaway ≥ 14pt · Source ≥ 10pt |
| Alignment | Left-aligned text · Chart centred |
| White space | Leave 40–60px margin on all edges — never crowd the edges |

**What to avoid:**

- ❌ More than 2 font families on one card
- ❌ Shadows, gradients, or clip art
- ❌ Chart title AND slide headline saying the same thing — pick one, remove the other
- ❌ Bullet points — this is a visual card, not a slide of text
- ❌ Putting the takeaway inside the chart — keep it below, in the card layout

---

## Stage 4 — Write Your Insight Cards (45 min)

Every chart needs three pieces of text. Write these before you put them in Canva — getting the words right matters more than the font.

### The Headline

One sentence. States the finding as a fact, not a description of the chart.

| Weak (describes) | Strong (states the finding) |
|--|--|
| "Trip Duration by Rider Type" | "Casual riders take trips twice as long as members" |
| "Hourly Trip Distribution" | "Bikeshare becomes a commuter tool after 7 AM — then goes quiet until 5 PM" |
| "Classic vs Electric Duration" | "Electric bikes don't just change how you ride — they change how long you ride" |

### The Takeaway (2 sentences)

Sentence 1: What the data shows, in plain numbers.
Sentence 2: Why it matters — what a decision-maker would do with this information.

Template:
> "[Specific finding with numbers.] This suggests [implication for the business or for understanding rider behaviour]."

Examples:

> "Casual riders average 24.3 minutes per trip versus 11.8 minutes for members. This suggests the system serves two fundamentally different populations — commuters who need short, reliable rides, and leisure riders who are exploring."

> "The p-value from our A/B test was 0.003, well below the 0.05 threshold. This means the duration difference between classic and electric bikes is almost certainly real, not a result of our random sample."

> "Trip volume spikes at 8 AM and 5 PM, following the pattern of a typical office commute. Off-peak hours (10 AM–3 PM) see dramatically lower usage — a pricing or incentive opportunity."

### The Source Line

Always include: `Source: Capital Bikeshare System Data, September 2023 · n = [valid trip count]`

This is what separates a professional chart from a student exercise. It tells the viewer exactly where the data came from.

---

## Stage 5 — Export and Organise for Day 5 (30 min)

Your Day 5 presentation will be built from what you create today. Organise everything now so Day 5 is assembly, not scrambling.

### File structure

Create a folder (Google Drive or local) called `Day5_Presentation`. Inside it:

```
Day5_Presentation/
├── charts/
│   ├── chart1_rider_duration.png
│   ├── chart2_hourly_pattern.png
│   ├── chart3_duration_histogram.png
│   ├── chart4_ab_test_results.png
│   └── chart5_heatmap.png        ← if you built it
├── insight_cards/
│   ├── card1_rider_gap.png       ← exported from Canva
│   ├── card2_city_rhythm.png
│   ├── card3_ab_test.png
│   └── card4_...png
└── story_outline.txt             ← see below
```

### Write your story outline

In `story_outline.txt`, write the running order for your Day 5 presentation — just bullet points at this stage:

```
PRESENTATION OUTLINE — [Your Name]
Story arc: Arc [A / B / C] — [Your one-sentence headline]

Slide 1 — Opening hook
  What question does this dataset answer?
  Why should the audience care?

Slide 2 — The pipeline (brief)
  Where did the data come from?
  How did you clean and validate it?
  One number: how many valid trips in September 2023?

Slide 3 — [Chart 1 insight card]
  Finding:
  Takeaway:

Slide 4 — [Chart 2 insight card]
  Finding:
  Takeaway:

Slide 5 — [Chart 3 insight card]
  Finding:
  Takeaway:

Slide 6 — A/B Test result
  Hypothesis:
  Result (p-value, Cohen's d):
  What this means for the business:

Slide 7 — Conclusion
  Restate the one-sentence story
  One recommendation based on your findings
  What you would investigate next if you had more data

```

This outline is your homework going into Day 3 and 4. We will build the full deck on Day 5.

Log in `PIPELINE_LOG`:
```
[Day 2] Visual storytelling complete
Arc chosen: 
Charts built: [list them]
Insight cards created: [count]
Exported to: Day5_Presentation/
Story outline: written
```

---

## Stage 6 — Peer Review (Optional, 30 min)

If working in a group: share one insight card with a classmate. Ask them:

1. Without reading the takeaway — what do you think the chart is saying?
2. Is the headline clear enough that you could repeat it to someone else?
3. What would make it easier to understand in 5 seconds?

Take one piece of feedback and update the card before exporting.

---

## 🧭 What This Feeds Into

| Day | What you're building toward |
|-----|----------------------------|
| Day 2 (today) | Charts and insight cards from your pipeline |
| Day 3 | GitHub — your pipeline and visuals go into a public repo |
| Day 4 | Personal portfolio page — your charts and repo link go live |
| Day 5 | Presentation — you tell the full story using everything from Days 1–4 |

Your pipeline produced the numbers. Today's work makes those numbers visible. Day 3 makes them findable. Day 4 makes them yours. Day 5 makes them count.

---

## ✅ Day 2 Deliverables Checklist

### Charts (in Google Sheets)
- [ ] Chart 1 — bar/column comparison, sourced from `03_AGGREGATE`
- [ ] Chart 2 — line chart showing pattern over time or hour
- [ ] Chart 3 — duration histogram with mean marked
- [ ] Chart 4 — A/B test results card (in Sheets, polished in Canva)
- [ ] Chart 5 — joint distribution heatmap *(optional)*

### Insight Cards (in Canva or Google Slides)
- [ ] One card per chart: headline + chart + 2-sentence takeaway + source line
- [ ] Headlines state findings, not descriptions
- [ ] Consistent design across all cards (same font, same accent colour)
- [ ] Exported as PNG to `Day5_Presentation/insight_cards/`

### Story Structure
- [ ] Narrative arc chosen and logged in `PIPELINE_LOG`
- [ ] `story_outline.txt` written — slide-by-slide structure for Day 5
- [ ] All files organised in `Day5_Presentation/` folder

---

## 💡 Bonus Challenges

**If you finish early — pick one:**

1. **Annotated chart:** In Canva, add one annotation arrow pointing to the most interesting feature of your histogram (e.g., the right-skew tail) with a 5-word label. Annotations turn a chart into a guided tour.

2. **Two-chart comparison layout:** Put the "members vs casual" duration bar chart side-by-side with the "members vs casual" hourly line chart on one Canva slide. Write one headline that connects both. What does seeing them together reveal that neither shows alone?

3. **Executive summary card:** Design one card (not a chart — just text and numbers) that summarises the entire pipeline in 5 data points: total trips, % members, mean duration, most popular hour, A/B test verdict. Think of it as the one card a CEO would look at. Keep it to 50 words or less.

---

> Data without a story is just a spreadsheet. A story without data is just an opinion. What you're building is both — and that combination is what data analysts get paid for.