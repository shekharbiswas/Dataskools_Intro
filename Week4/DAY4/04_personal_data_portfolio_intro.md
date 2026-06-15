# Day 4 — Personal Data Portfolio
## From Pipeline to Portfolio: Presenting Yourself as a Data Thinker

> **Built on:** Pipeline (Day 1) · Visuals (Day 2) · GitHub repo (Day 3)
> **Tools:** GitHub Pages or Carrd · Markdown · Your existing repo
> **Duration:** 6 hours
> **Difficulty:** ⭐⭐
> **Feeds into:** Day 5 Presentation — your portfolio page is the link you share with your audience

---

## 🧭 The Big Picture

You have a pipeline. You have visuals. You have a documented GitHub repository.

Today you turn all of that into a **portfolio page** — a single link you can share with anyone, that tells the story of who you are as a data thinker, not just what tasks you completed.

This matters beyond the classroom. When someone considers you for a job, an internship, or a course, they Google you. A portfolio page that loads cleanly, shows a real project, and explains your thinking in plain English is worth more than a CV that lists "Google Sheets" under Skills.

By the end of today you will have:
- A live portfolio page with your name, your project, and your insights
- Your Capital Bikeshare repo linked and your insight cards embedded
- A "next project" section that shows you think beyond assigned work
- Everything ready to share as a URL during your Day 5 presentation

---

## 🗺️ Today's Plan

| Stage | Task | Time |
|-------|------|------|
| Stage 1 | Reflect — what kind of data thinker are you? | 45 min |
| Stage 2 | Build your portfolio page | 120 min |
| Stage 3 | Write your project feature section | 60 min |
| Stage 4 | Draft your "next project" idea | 45 min |
| Stage 5 | Final checks and Day 5 prep | 30 min |

---

## Stage 1 — Reflect: What Kind of Data Thinker Are You? (45 min)

Before building anything, do this thinking first. The words you write here will go directly into your portfolio page — so they need to be honest and specific, not generic.

### 1A — Look back at the pipeline you built

Open your Google Sheets pipeline. Scroll through every tab — `00_RAW` to `08_DASHBOARD`. Think about the whole journey.

Then answer these four questions in your `journal/day4-reflection.md` file (create it now in your GitHub repo). Write in full sentences, not bullet points. Aim for 200–300 words total.

---

**Question 1: Which stage of the pipeline did you find most satisfying — and why?**

Think specifically. Not "I liked the analysis" — which tab? Which formula? Which moment did something click?

> Example of a weak answer: *"I enjoyed the statistics part."*
>
> Example of a strong answer: *"Stage 5 — the probability layer. When I calculated P(electric | member) vs P(electric | casual) and saw they were different, I realised that 'electric bike preference' isn't a fixed property of the bike — it's connected to who the rider is. That felt like a real discovery, not just a formula result."*

---

**Question 2: What surprised you most — something the data showed that contradicted your intuition?**

Before you ran the pipeline, what did you expect to find? Where did the data push back?

---

**Question 3: What would you want to know next if you had 12 months of data instead of one month?**

This is the question that separates a student who completed an assignment from a data thinker who got curious. There is no wrong answer — but there is a shallow one ("I would look at more months") and a deep one ("I would look at whether the member–casual duration gap narrows in winter, when leisure cycling drops off and only commuters remain").

---

**Question 4: What kind of data problems do you want to work on in the future?**

Not job titles — data problems. The kind of questions you find genuinely interesting. Some examples to spark thinking:

- "I want to understand why people churn — why they stop using a product or service"
- "I want to work with health data because I care about preventable disease"
- "I want to analyse financial market data — specifically whether retail investors behave differently from institutional ones"
- "I want to look at education data — does school attendance actually predict outcomes?"

Your answer here will become the "About Me" section of your portfolio page.

---

### 1B — Identify your data identity

Based on your four answers, complete this sentence:

> *"I am most drawn to data work that involves ________, because ________."*

Write this in your journal. You will use it verbatim on your portfolio page.

---

## Stage 2 — Build Your Portfolio Page (120 min)

You have two options. Choose based on how much time you want to invest in design vs content.

---

### Option A — GitHub Pages (recommended, free, lives on your GitHub)

GitHub Pages turns a Markdown file in your repo into a live website at `https://[your-username].github.io`

**Setup:**

1. Go to github.com → click your profile icon → **Your repositories** → **New**
2. Name the repo exactly: `[your-username].github.io`
   (Replace `[your-username]` with your actual GitHub username — this exact naming is what activates GitHub Pages)
3. Set to **Public** · Check **Add a README file** · Click **Create repository**
4. Click into `README.md` → click the pencil icon to edit
5. Write your portfolio content here in Markdown (template below)
6. Click **Commit changes**
7. Go to **Settings** → **Pages** → Source: **Deploy from a branch** → Branch: `main` → **Save**
8. Wait 2–3 minutes → your page is live at `https://[your-username].github.io`

---

### Option B — Carrd (no-code, faster to design, free tier available)

1. Go to [carrd.co](https://carrd.co) → **Build your own** → **Portfolio**
2. Choose the simplest template (you will replace all content)
3. Build section by section using the template below as your content guide
4. Publish → copy your Carrd URL

---

### Portfolio page template

Whether you use GitHub Pages or Carrd, your page should have these five sections in this order:

---

#### Section 1 — Your Name and Data Identity (the header)

```
[Your Name]
Data analyst in training · [City] · [Month Year]

[Your one-sentence data identity from Stage 1B]
```

Keep it short. This is not a bio — it is a positioning statement. The reader should know who you are and what you care about in 10 seconds.

---

#### Section 2 — Featured Project: Capital Bikeshare Pipeline

This is the centrepiece of your portfolio. Structure it like a case study, not a description.

```
## Capital Bikeshare Data Pipeline
September 2023 · Google Sheets · 87,000+ valid trips

### The Question
[One sentence: what business question does this pipeline answer?]

### What I Built
A 9-layer data pipeline in Google Sheets — ingestion, cleaning, aggregation, 
statistical analysis, probability modelling, A/B testing, and a business dashboard — 
applied to real Capital Bikeshare trip data from Washington DC.

### Key Findings
· [Finding 1 — with a real number]
· [Finding 2 — with a real number]  
· [Finding 3 — the A/B test result in one sentence]

### The A/B Test
[Your plain-English p-value interpretation from ab_test_results.md — 2 sentences]

[Embed your best insight card image here]

[→ View the full pipeline on GitHub](https://github.com/[your-username]/capital-bikeshare-pipeline)
```

**The key difference between a portfolio case study and a project description:**
- A description says what you did: *"I cleaned data and ran statistics."*
- A case study says what you found and why it matters: *"Casual riders take 2× longer trips than members — a finding that suggests the system serves two fundamentally different populations, with different pricing implications."*

Write yours as a case study.

---

#### Section 3 — Visuals

Embed two or three of your best insight cards. On GitHub Pages, use:

```markdown
![Rider Duration Comparison](https://raw.githubusercontent.com/[your-username]/capital-bikeshare-pipeline/main/insight_cards/card1_rider_duration.png)

![A/B Test Result](https://raw.githubusercontent.com/[your-username]/capital-bikeshare-pipeline/main/insight_cards/card4_ab_test.png)
```

On Carrd: use the Image block and upload the PNG files directly.

These visuals do three things on a portfolio page:
1. They show that you can produce presentation-quality output — not just run formulas
2. They let a viewer grasp your findings without reading a word
3. They prove the work is real — these are actual charts from actual data

---

#### Section 4 — What I'm Exploring Next

This section is forward-facing. It shows that the Capital Bikeshare project was the beginning of your data curiosity, not the entirety of it.

Write 3–5 sentences about a data question you want to investigate next. Use your answer to Question 3 and 4 from Stage 1 as the starting point.

Structure it like this:

```
## What I'm Exploring Next

[One sentence: the question or topic that interests you]

[Two sentences: why this question matters and what data you'd need to investigate it]

[One sentence: what skills you'd need to develop to answer it — 
and what you've already started building through this pipeline]
```

> This section is important for Day 5. When your instructor or classmates ask "what would you do next?" — you already have a written, thoughtful answer. You're not improvising.

---

#### Section 5 — Contact / Links

Keep it minimal:

```
## Find My Work

GitHub: github.com/[your-username]  
Project repo: github.com/[your-username]/capital-bikeshare-pipeline  
[Email — optional]
```

---

## Stage 3 — Write Your Project Feature Section (60 min)

Go back to your portfolio page's Section 2 and make it excellent. This is the part that gets read most carefully — spend real time here.

### The "Key Findings" bullets

Each bullet should follow this structure: **[What] — [number] — [so what]**

| Weak | Strong |
|------|--------|
| "Members take shorter trips" | "Members average 11.8 min per trip vs 24.3 min for casual riders — a 2× gap that suggests two distinct use cases: commuting vs leisure" |
| "Electric bikes are popular" | "38% of valid September trips used electric bikes, with higher uptake among casual riders (43%) than members (35%)" |
| "The A/B test was significant" | "p = 0.003: the duration difference between classic and electric bikes is almost certainly real, not a result of our random sample" |

Go back to your pipeline and pull the real numbers. Do not estimate or round aggressively. The specificity of the number is what makes the finding credible.

### The case study opening line

This is the single most important sentence on your portfolio page. It answers: *"Why should I care about this project?"*

Write 3 versions. Pick the best one.

Examples:
- "Washington DC's bikeshare system generated 90,000+ trips in a single September — and the data reveals that members and casual riders are using the same infrastructure in fundamentally different ways."
- "I built a complete data pipeline from scratch — ingestion to A/B test — using only Google Sheets, on real Capital Bikeshare trip data."
- "What does a city look like in data? September 2023 Capital Bikeshare data shows commuter peaks at 8 AM and 5 PM, a 2× duration gap between rider types, and a statistically significant difference between bike types — all from a single month of rides."

Write all three in your journal. Choose the one that best matches your narrative arc from Day 2.

---

## Stage 4 — Draft Your "Next Project" Idea (45 min)

The "What I'm Exploring Next" section needs to be specific enough to be credible. Anyone can write "I want to analyse health data." Not everyone can write this:

> *"I want to investigate whether daily step counts are actually predictive of self-reported energy levels — or whether sleep quality is a stronger predictor. I'd collect my own data using a phone health app over 8 weeks, clean it in Google Sheets following the same pipeline structure I built for Capital Bikeshare, and test the relationship using correlation and simple regression."*

That is a real project idea. It names the question, the data source, the method, and the connection to skills already built.

### How to develop yours

Use this framework:

**1. The question** (one sentence — ends with a "?")**:**
What do you actually want to know? Start with something you're genuinely curious about in your own life, your city, your field of interest.

**2. The data** (where would you get it?)**:**
- Your own phone's health or screen time data
- A public dataset from [data.gov](https://data.gov), [kaggle.com/datasets](https://kaggle.com/datasets), or your city's open data portal
- Data you would collect yourself (a simple Google Form, a daily journal)

**3. The method** (which pipeline stages would you use?)**:**
Which parts of the Capital Bikeshare pipeline apply? Would you need new techniques? What would you look up?

**4. The output** (what would you show?)**:**
A chart? An A/B test? A dashboard? A recommendation?

Write this in `journal/day4-reflection.md` under a heading called `## Next Project Idea`. Even if it's rough — having it written means you can develop it into your second portfolio project after this week.

---

## Stage 5 — Final Checks and Day 5 Prep (30 min)

### Portfolio page checklist

**Content**
- [ ] Your name and one-sentence data identity are in the header
- [ ] The Capital Bikeshare project is featured as a case study (not a description)
- [ ] Key findings section has real numbers — no placeholders
- [ ] A/B test result is stated in plain English
- [ ] At least 2 insight card images are embedded and load correctly
- [ ] GitHub repo link works and opens your `capital-bikeshare-pipeline` repo
- [ ] "What I'm Exploring Next" section is written (not blank, not a placeholder)
- [ ] Contact/links section has your GitHub URL

**Design**
- [ ] Page loads cleanly on a phone (check by opening your URL on your phone)
- [ ] Text is readable without zooming — no tiny fonts
- [ ] Images are not broken (test by opening the page in an incognito window)
- [ ] No placeholder text remaining (no "Lorem ipsum", no "Your name here")

### Prepare your Day 5 presentation link

Write this down somewhere you can access it instantly during your presentation tomorrow:

```
Portfolio page URL:  https://[your-username].github.io  
                     OR https://[your-name].carrd.co

GitHub repo URL:     https://github.com/[your-username]/capital-bikeshare-pipeline

Google Sheets pipeline: [paste the shareable link here — make sure it's set to "Anyone with the link can view"]
```

You will share these three links at the start of your Day 5 presentation.

Log in `PIPELINE_LOG`:
```
[Day 4] Portfolio complete
Portfolio URL: 
GitHub repo URL: 
Sections: header · Capital Bikeshare case study · visuals · next project · links
Next project idea: [one sentence summary]
Day 5 links: saved and tested
```

---

## 🧭 What This Feeds Into — The Full Arc

| Day | What you built | What it shows |
|-----|---------------|---------------|
| Day 1 | Data pipeline | You can engineer data — clean it, structure it, validate it |
| Day 2 | Visuals and insight cards | You can communicate findings — charts, headlines, takeaways |
| Day 3 | GitHub repo | You can document work — README, analysis files, commit history |
| Day 4 (today) | Portfolio page | You can present yourself — case study, data identity, next steps |
| Day 5 | Presentation | You can tell the full story — from raw data to business recommendation |

Tomorrow you walk your audience through the whole arc in 10 minutes. Everything you've built this week is evidence. Your job on Day 5 is to connect it into a single coherent story.

---

## ✅ Day 4 Deliverables Checklist

- [ ] `journal/day4-reflection.md` — 4 questions answered, data identity sentence written, next project idea drafted
- [ ] Portfolio page live at a public URL (GitHub Pages or Carrd)
- [ ] Header: name + one-sentence data identity
- [ ] Capital Bikeshare case study: question · what I built · key findings (with real numbers) · A/B test result
- [ ] At least 2 insight cards embedded and loading correctly
- [ ] GitHub repo link working
- [ ] "What I'm Exploring Next" section written
- [ ] Page tested on mobile
- [ ] Three Day 5 links saved: portfolio URL · repo URL · Google Sheets link

---

## 💡 Bonus Challenges

**If you finish early — pick one:**

1. **Write a second project stub.** Your portfolio page's "next project" section is stronger if it shows a project already in progress, not just an idea. Start a new GitHub repo called `[your-topic]-analysis`, add a README with just the question, the data source, and the method. Link it from your portfolio page under "Coming Soon." This signals momentum, not just intention.

2. **Add a skills section to your portfolio page.** Not a list of tools — a list of *capabilities*. The difference:
   - ❌ "Google Sheets, Canva, GitHub"
   - ✅ "Building multi-layer data pipelines · Translating statistical findings into plain-English business recommendations · Designing A/B experiments from hypothesis to effect size"
   These come directly from what you built this week. Write them as sentences a hiring manager would find meaningful.

3. **Share your portfolio link with one person outside this course** — a friend, a family member, a professional contact — and ask them: "Does this make sense to you? Can you tell what I did and why it matters?" Their confusion (if any) is your most valuable feedback before Day 5.

---

> A portfolio is not a list of things you did. It is evidence of how you think. The Capital Bikeshare pipeline shows that you can take raw, messy data and turn it into a business recommendation — using nothing but a spreadsheet, a clear process, and careful thinking. That is a real skill. Today you made it visible.