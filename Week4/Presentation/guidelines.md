# Day 5 — Presentation Guidelines
## Capital Bikeshare Pipeline: Telling the Full Story

> **Presenting:** Everything you built across Days 1–4
> **Format:** 12–15 slides · 15–20 minutes · live Q&A
> **Audience:** Your instructor and classmates
> **What you're sharing:** Portfolio URL · GitHub repo · Google Sheets pipeline

---

## 🧭 What Today Is

This is not a summary of what you did. It is a **data presentation** — the same kind a junior analyst would give to a product team, an operations manager, or a client.

Your audience does not want to hear "first I went to Stage 1, then Stage 2." They want to hear: *"Here is a question. Here is what the data shows. Here is what it means. Here is what I would do next."*

Everything you built this week — the pipeline, the charts, the GitHub repo, the portfolio page — is evidence that supports that story. Your job today is to connect it all into one clear, confident 15–20 minute narrative.

---

## 🎯 Before You Start: The Three Links

Share these three links at the very start of your presentation, before slide 1. Write them on the board, paste them in the chat, or put them on your title slide:

```
Portfolio page:   https://[your-username].github.io
GitHub repo:      https://github.com/[your-username]/capital-bikeshare-pipeline
Google Sheets:    [your shareable pipeline link]
```

Your audience should be able to follow along in your actual pipeline while you present. This is what makes it a real data presentation — not a slide deck about a project, but a live walkthrough of work they can inspect.

---

## 📐 Slide Structure — 12–15 Slides

### Slide 1 — Title (1 slide)

```
[Your Name]
Capital Bikeshare Data Pipeline
September 2023 · Washington DC

[Your one-sentence story headline from Day 2]

Portfolio: [URL]     GitHub: [URL]
```

Keep it clean. One sentence of story. Your three links. Nothing else.

---

### Slide 2 — The Question (1 slide)

Before showing any data, tell your audience what question you set out to answer.

One question. One slide. Large text.

> *"What can one month of Capital Bikeshare trip data tell us about how different types of riders use the system — and does bike type actually change how long people ride?"*

Or your own version, aligned to the narrative arc you chose on Day 2.

**Why this slide matters:** It sets up the entire presentation as an investigation, not a demo. Your audience is now waiting for the answer — and they'll follow you through 12 more slides to get it.

---

### Slide 3 — The Data and the Pipeline (1–2 slides)

Tell your audience exactly what you worked with and how you structured your approach.

**What to cover:**
- Data source: Capital Bikeshare System Data, September 2023
- Raw row count → valid rows after cleaning → pass rate percentage
- The pipeline architecture: show the tab map as a simple flow diagram

**Suggested layout — one clean flow on the slide:**

```
00_RAW → 02_CLEAN → 03_AGGREGATE → 04_STATS
                                 → 05_PROBABILITY
                                 → 06_SAMPLING → 07_AB_TEST
                                                      ↓
                                               08_DASHBOARD
```

**One sentence to say out loud:**
> *"Every number in this presentation was computed in a layered pipeline — raw data was never touched after ingestion, and every downstream tab references the one above it. If the source data changes, every chart and figure updates automatically."*

This one sentence tells your audience you understand data engineering principles, not just spreadsheet formulas.

---

### Slide 4 — Data Quality (1 slide)

Most student presentations skip this. Don't. It is what separates a pipeline presentation from a "here are some charts" presentation.

**What to show:**
- Total raw rows ingested
- Rows excluded and why (duration out of range, blank fields)
- Final valid row count and pass rate
- The `is_valid` rule in plain English — not the formula, the logic:

> *"A trip was marked valid if it had a duration between 1 and 180 minutes, a non-blank ride ID, and a non-blank rider type. Anything outside those rules was flagged FALSE and excluded from all downstream analysis."*

**One data quality finding** that surprised you — from your `01_SCHEMA` quality audit.

---

### Slides 5–7 — Key Findings (3 slides, one finding per slide)

These are your insight cards from Day 2. One card per slide. No bullet points.

Each slide has three elements only:
1. The headline (your finding stated as a fact)
2. The chart or visual (your insight card image)
3. The takeaway (spoken aloud — not on the slide)

**Slide 5 — Finding 1**
Your strongest finding about rider behaviour (member vs casual split, duration gap, or hourly pattern — depending on your arc).

**Slide 6 — Finding 2**
Your second most interesting finding — a different angle from Finding 1.

**Slide 7 — Finding 3**
The distribution shape — your histogram with mean vs median gap explained. This slide connects to the stats layer and shows your analytical depth.

**How to present each finding slide:**
1. Read the headline aloud
2. Point to the most important feature of the chart (the gap, the spike, the tail)
3. Say your takeaway in 2 sentences
4. Move on — do not over-explain

---

### Slide 8 — The Probability Layer (1 slide)

This slide shows that your analysis went deeper than averages and bar charts.

**What to cover:**
- P(member) and P(casual) — the base split
- Your independence test result: is electric bike preference independent of rider type?
- P(electric | member) vs P(electric | casual) vs P(electric overall)
- Your one-sentence business implication

**Suggested layout — a simple 3-number comparison:**

```
P(electric bike | member) = XX%
P(electric bike | casual) = XX%
P(electric bike | overall) = XX%

Finding: electric bike preference [IS / IS NOT] independent of rider type.
```

**One sentence to say out loud:**
> *"This matters for fleet decisions — if electric bike preference differs between rider types, Capital Bikeshare can't treat 'electric bike demand' as a single number. They need to model it separately for members and for casual riders."*

---

### Slides 9–10 — The A/B Test (2 slides)

This is the centrepiece of the analytical work. Give it two slides.

**Slide 9 — The Hypothesis and Setup**

```
EXPERIMENT: Classic vs Electric Bike Trip Duration

H₀: No difference in mean trip duration between classic and electric bikes
H₁: Electric bikes produce different mean trip durations

Sample: 1,500 classic + 1,500 electric (randomly selected from valid trips)
Method: Welch's two-sample t-test · α = 0.05
```

Say out loud why you used Welch's t-test (two independent samples, unequal variance — the safest default). This shows you understand the test, not just the formula.

**Slide 10 — The Result**

```
Classic mean:    XX.X min     Electric mean:   XX.X min
Difference:      XX.X min     p-value:         X.XXX
Cohen's d:       X.XX         Effect size:     [Small / Medium / Large]

Verdict: [REJECT / FAIL TO REJECT] H₀ at α = 0.05
```

Then — and this is the most important part of the whole presentation — say this in plain English:

> *"A p-value of [X] means that if there were truly no difference between classic and electric bike trip durations, we would see a gap this large by random chance [X% of the time / less than 1% of the time]. That is [too common / rare enough] to be convincing evidence of a real effect."*

Then address the confound:
> *"But we also checked whether this difference might simply be a disguised member–casual effect. When we looked at members only — and casual riders only — the classic vs electric gap [still held / disappeared / reversed]. So rider type [does / does not] fully explain what we found."*

---

### Slide 11 — The Dashboard (1 slide)

Show a screenshot of your `08_DASHBOARD` tab.

This slide has one job: show that your pipeline has a real output layer — not just analysis tabs, but something a business user could open and read without understanding any of the formulas behind it.

Point to:
- The pipeline health strip (where the data came from, pass rate)
- The key metric cards
- The A/B test results section
- Your 2-sentence recommendation

Say: *"Every number on this dashboard is a reference to an upstream tab. The dashboard itself contains no calculations — it only displays."*

---

### Slide 12 — GitHub and Portfolio (1 slide)

Show your GitHub repo and your portfolio page — live, in a browser, not as a screenshot.

Click through:
- The README — show the Key Findings section with real numbers
- One analysis file (`ab_test_results.md` or `stats_summary.md`)
- Your portfolio page — show the case study section and the embedded insight cards

Say: *"Everything I've presented today is publicly documented and reproducible. Anyone can open my GitHub repo, download the source data from Capital Bikeshare, and rebuild this pipeline from scratch following the README and pipeline log."*

This is the professional standard — reproducible, documented, public.

---

### Slide 13 — What I Learned (1 slide)

One slide. Three sentences maximum. Your own voice — not bullet points.

Not "I learned about data pipelines." Specific and personal:

> *"The hardest part was Stage 2 — getting the `duration_min` formula right across thousands of text-formatted timestamps. The most surprising finding was [specific number or result]. If I could redo one thing, I would have added an hourly breakdown by rider type to `03_AGGREGATE` from the start — the segmented analysis in Stage 7 showed that this would have made the confound check much cleaner."*

---

### Slide 14 — What I'd Investigate Next (1 slide)

Your "next project" from Day 4, condensed to one slide.

```
If I had 12 months of data instead of one month, I would investigate:

[Your question — one sentence]

Because: [why it matters — one sentence]

I would need: [what data or skills — one sentence]
```

This slide matters. It tells your audience that the Capital Bikeshare project was the beginning of your data curiosity — not the end of an assignment.

---

### Slide 15 — Close and Links (1 slide)

```
Thank you.

[Your one-sentence story headline — repeated from Slide 1]

Portfolio:  [URL]
GitHub:     [URL]
Pipeline:   [Google Sheets URL]

Questions?
```

Repeat your headline. Repeat your links. End clean.

---

## ⏱️ Time Budget — 15–20 Minutes

| Section | Slides | Time |
|---------|--------|------|
| Title + question | 1–2 | 1 min |
| Data and pipeline | 3–4 | 2 min |
| Data quality | 4 | 1 min |
| Key findings | 5–7 | 4 min |
| Probability layer | 8 | 2 min |
| A/B test | 9–10 | 4 min |
| Dashboard | 11 | 1 min |
| GitHub + portfolio | 12 | 1 min |
| What I learned + next steps | 13–14 | 2 min |
| Close | 15 | 30 sec |
| **Total** | **12–15** | **~18 min** |

Leave 2–5 minutes for questions at the end.

> **Pacing rule:** If you catch yourself reading from the slide, you are going too slow and too deep. Every slide should be on screen for no more than 90 seconds. The words on the slide are anchors — your spoken words are the explanation.

---

## 🎤 Presentation Delivery — What to Do and What to Avoid

### Do

**Start with the question, not the data.** Your first 60 seconds should make your audience curious. "I had 90,000 trips and a question" is more engaging than "I downloaded a CSV and imported it into Google Sheets."

**Say the number, then say what it means.** Never let a number stand alone.
- ❌ *"The p-value was 0.003."*
- ✅ *"The p-value was 0.003 — which means this difference would appear by chance less than 1 in 300 times. That's convincing evidence."*

**Point at your chart.** Use your cursor or a pointer. Tell people exactly where to look. "Notice this spike at 8 AM" — then pause while they find it.

**Slow down for the A/B test.** This is the most technically dense part. Give it more time than feels comfortable. If your audience follows nothing else, they should follow the p-value interpretation.

**Make eye contact when you deliver findings.** Look at your slides when you're describing process. Look at your audience when you're delivering a finding. The finding is the reason they're there.

### Avoid

**Don't read your slides.** If your slide says "Casual riders average 24 minutes per trip," don't say "Casual riders average 24 minutes per trip." Say "That 24-minute average is what you'd expect from someone exploring the city, not commuting through it."

**Don't apologise for limitations.** Every dataset has limitations. State them once, briefly, as part of your "what I'd investigate next" slide — not as a disclaimer scattered through the presentation.

**Don't skip the confound check.** It is the most intellectually honest part of your A/B test. If you skip it, a sharp question from the audience will expose the gap. Address it proactively.

**Don't end on "that's it."** End on your headline. You opened with a question — close by answering it, in one sentence, looking at your audience.

---

## ❓ Preparing for Questions

These are the questions most likely to come from your instructor or classmates. Prepare a one-sentence answer for each.

| Question | What they're testing | Prepare |
|----------|---------------------|---------|
| "Why did you choose duration as the outcome variable for your A/B test?" | Do you understand the design choice? | You had a clean continuous variable, a natural split (bike type), and a business reason to care about duration differences. |
| "What does Cohen's d tell you that the p-value doesn't?" | Do you understand practical vs statistical significance? | p-value tells you the difference is real. Cohen's d tells you whether it's big enough to act on. |
| "Could there be other explanations for the classic vs electric duration difference?" | Do you understand confounds? | Yes — you checked rider type as a confound in Stage 7F. Summarise what you found. |
| "What would you do differently if you had more data?" | Can you think beyond the assignment? | Your answer from Slide 14 — give it without looking at the slide. |
| "Why is `00_RAW` never edited?" | Do you understand the immutable source principle? | If the source is modified, you lose the ability to trace errors back to the origin. Every transformation should be auditable. |
| "What does the pipeline log tell someone who opens your repo?" | Do you understand documentation? | It records every decision made during construction — cleaning rules, exclusion rationale, test design choices — so the work is reproducible and auditable. |

---

## ✅ Day 5 Presentation Checklist

### Before you present

- [ ] Three links tested and working: portfolio · GitHub repo · Google Sheets (view access confirmed)
- [ ] All insight card images display on portfolio page
- [ ] Slide deck: 12–15 slides, no placeholder text, all numbers real
- [ ] A/B test plain-English interpretation written and practised out loud
- [ ] Confound check answer prepared (2 sentences)
- [ ] "What I learned" slide written in your own voice
- [ ] "Next project" slide written with question, data source, and method
- [ ] Timed yourself: target 15–18 minutes, leaving 2–5 minutes for questions

### Slide-by-slide check

This is just a guideline and you can change the overall structure that fits you the best.

- [ ] Slide 1 — Title + story headline + three links
- [ ] Slide 2 — The question (one sentence, large text)
- [ ] Slide 3 — Pipeline architecture diagram + data source
- [ ] Slide 4 — Data quality: pass rate, exclusion rules, one surprising finding
- [ ] Slides 5–7 — Three insight cards: headline + chart + spoken takeaway
- [ ] Slide 8 — Probability layer: independence test result + business implication
- [ ] Slide 9 — A/B test setup: hypothesis + sample + method
- [ ] Slide 10 — A/B test result: p-value + Cohen's d + verdict + confound check
- [ ] Slide 11 — Dashboard screenshot with pipeline health + recommendation
- [ ] Slide 12 — GitHub repo + portfolio page (live browser, not screenshot)
- [ ] Slide 13 — What I learned (3 sentences, your voice)
- [ ] Slide 14 — Next project (question + data + method)
- [ ] Slide 15 — Close: repeated headline + three links

---

## 💡 The One Thing

If you remember nothing else from these guidelines, remember this:

**Your audience is not evaluating whether you completed the tasks. They are evaluating whether you understand what you built.**

Anyone can import a CSV and run a TTEST. What matters is whether you can explain *why* you chose a two-tailed Welch's test, *what* the p-value actually means, *whether* the effect size is large enough to act on, and *what* you would investigate next.

That understanding — shown through confident, plain-English explanation of your own pipeline — is what makes a data presentation worth 15 minutes of someone's time.

You built something real this week. Now tell its story.

---

> *"The goal is to turn data into information, and information into insight."*
> — Carly Fiorina
Done

Claude Fable 5 is currently unavailable.
Learn more(opens in new tab)




