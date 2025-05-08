# 📅 Spreadsheet ETL Guide: Clean, Explore, Reflect

This guide walks you through a real-world data project using a Google Spreadsheet. 

You’ve already worked with this dataset — Lina’s Lemonade Stand — in earlier lessons. Now, it’s time to return to it with fresh eyes and deeper skills.

You’ll apply the full ETL (Extract, Transform, Load) process again — and go beyond it: exploring, summarizing, visualizing, and reflecting like a professional data analyst.

No programming or coding experience is needed. All you need is your previous spreadsheet and a curious, growth-focused mindset.

<img width="300" alt="image" src="https://github.com/user-attachments/assets/3cb3d2f3-ca0c-4d70-86c6-0595bfccab2e" />


> **Why are we revisiting the same dataset?**  
> Because real confidence is built through **repetition with reflection**. Each time you revisit familiar data, you’ll notice new patterns, apply cleaner logic, and ask smarter questions.  
> Analysts don’t just use tools — they think through data.
> That habit grows with each thoughtful iteration.

> It might seem exhausting but this is a process.

<br>

## 🎯 What You’ll Do

- Revisit your existing spreadsheet (Lina’s Lemonade Stand or a similar dataset)
- Apply the data cleaning and exploration techniques you've been learning
- Detect inconsistencies, outliers, and missing values
- Create meaningful summaries and visualizations
- Reflect on what the data reveals and how you interpreted it

> You’re not just fixing data — you’re learning to think critically, ask questions, and interpret what the numbers are really telling you.

<br>

## 🛠️ Step-by-Step ETL + Analysis (20+ Concepts Explained)

### 1. **Extract your data**

Start by getting your dataset into a fresh working environment.

- Open [Google Sheets](https://sheets.google.com)
- Go to `File > Import > Upload` and upload your spreadsheet (e.g., an Excel file or CSV)
- Choose the option: “Insert new sheet(s)”

**Why this matters:** By inserting your data into a new sheet instead of overwriting the current one, you preserve your original data. This is a good habit for any data work: never edit the raw data directly. Think of it as your “source of truth.”

<br>

### 2. **Standardize text formatting**

If your dataset includes columns like `Flavor`, there might be entries such as `lemon`, ` Lemon`, or `LEMON`. These all refer to the same flavor but are formatted inconsistently.

- Add a new column next to `Flavor`
- Use this formula:  
  `=LOWER(TRIM(B2))`  
  This converts all text to lowercase and removes any extra spaces.

**Why this matters:** Spreadsheets treat different formats as different values. Standardizing helps you group data correctly and avoid misleading summaries.

<br>

### 3. **Fix inconsistent capitalizations**

Check the entire column for inconsistencies like mixed case text (`Lemon`, `LEMON`, `lemon`). Apply lowercase formatting using the formula above.

- Drag down the formula to apply it to the whole column

**Why this matters:** Clean and consistent data improves accuracy in counts, sorting, and pivot tables later.

<br>

### 4. **Flag missing values**

Identify rows where data is missing — especially for key columns like `Cups Sold`.

- Add a helper column:  
  `=IF(ISBLANK(C2), "MISSING", C2)`
- Use conditional formatting to highlight any row where “MISSING” appears

**Why this matters:** Missing data can affect calculations and analysis. Highlighting them lets you decide how to handle those rows — ignore, delete, or fill with assumptions.

<br>

### 5. **Format dates properly**

Spreadsheets often import dates as plain text or numbers. Let’s fix that:

- Select the `Date` column
- Go to `Format > Number > Date`
- Add a new column with:  
  `=TEXT(A2, "dddd")`  
  This shows the day of the week (e.g., Monday, Tuesday)

**Why this matters:** Understanding when sales occur (weekends vs weekdays) can reveal patterns and help guide decisions.

<br>

### 6. **Create a calculated column (Total Revenue)**

Let’s calculate how much money each row earned.

- Use: `=C2 * D2`  
  where `C2` is `Cups` and `D2` is `Price per cup`
- Label the column `TotalRevenue`

**Why this matters:** Calculating revenue lets you explore overall performance, trends, and top-performing days or flavors.

<br>

### 7. **Handle notes and comments**

Sometimes your dataset includes a column like `Notes` with text like “repeat customer” or “forgot to write price”.

- Add filters to this column so you can quickly scan or sort these comments
- Consider creating tags (e.g., “issue”, “loyalty”) in a new column based on keywords

**Why this matters:** These notes might contain valuable business insights or indicate errors in data entry.

<br>

### 8. **Use filters**

Enable filters to explore subsets of the data easily.

- Select the header row (Row 1)
- Go to `Data > Create a filter`
- Use filter icons to show only certain flavors, blank values, or high sales days

**Why this matters:** Filtering helps you analyze smaller pieces of data and test hypotheses — like which flavor sells best.

<br>

### 9. **Use sorting to detect outliers**

Sort the dataset by `Cups Sold` or `Total Revenue`.

- Go to the `Data` menu and use `Sort A-Z` or `Z-A`

**Why this matters:** Sorting helps you find extreme values — which might be true high performers, or just mistakes (e.g., 999 cups in one day?).

<br>

### 10. **Calculate summary statistics**

Use spreadsheet functions to get a sense of your data distribution.

- AVERAGE: `=AVERAGE(E2:E100)`  
- MEDIAN: `=MEDIAN(E2:E100)`  
- MODE: `=MODE(E2:E100)`  
- MIN/MAX: `=MIN(E2:E100)` / `=MAX(E2:E100)`

**Why this matters:** These measures help you understand the central tendency and spread of your data — a key analyst skill.

<br>

### 11. **Highlight potential outliers visually**

Use conditional formatting to color cells that are above or below certain thresholds (e.g., more than 50 cups in a day).

- Go to `Format > Conditional formatting`
- Apply rules like “Cell value > 50” to highlight unusually high numbers

**Why this matters:** Outliers can point to either success stories or data entry problems — both are worth investigating.

<br>

### 12. **Check for skewness**

You can approximate skewness by comparing the average and median.

- Add a column: `=E2 - MEDIAN($E$2:$E$100)`
- See if the results are mostly positive or negative

**Why this matters:** A large difference between average and median suggests skewness — data that's not symmetrical — which can influence your choice of metrics or graphs.

<br>

### 13. **Label rows for quality control**

Create a “Data Quality” column to quickly see if each row passes basic checks.

- Use:  
  `=IF(AND(C2>0, NOT(ISBLANK(D2))), "OK", "REVIEW")`

**Why this matters:** Helps you spot rows that need attention, especially before creating summaries or charts.

<br>

### 14. **Add an Analyst Notes column**

Add a column named `AnalystNotes` where you write your thoughts for each row.

- For example: “Quantity missing”, “Very high sale — check for event?”, “Low price?”

**Why this matters:** Developing the habit of jotting down insights prepares you for real-life reporting and storytelling.

<br>

### 15. **Create pivot tables for summaries**

Pivot tables are powerful tools for summarizing data.

- Go to `Data > Pivot Table`
- Rows: Flavor  
- Values: SUM of Cups, AVERAGE of Revenue

**Why this matters:** This gives you a clear comparison across categories like flavor, day, or customer type.

<br>

### 16. **Create a pie chart (Flavor share of revenue)**

- Select columns `Flavor` and `TotalRevenue`
- Go to `Insert > Chart > Pie chart`

**Why this matters:** Pie charts show proportion. It’s useful for understanding contribution of each flavor to total sales.

<br>

### 17. **Create a bar chart (Flavor vs Cups)**

- Highlight `Flavor` and `Cups`
- Insert > Chart > Bar chart

**Why this matters:** Bar charts are great for comparing quantities across categories.

<br>

### 18. **Create a line chart (Date vs Cups)**

- Select `Date` and `Cups`
- Insert > Line chart  
- Add trendline via `Customize > Series > Trendline`

**Why this matters:** Line charts are best for showing how things change over time.

<br>

### 19. **Add summary metrics**

Create formulas at the bottom of your data for key figures:

- Total Revenue: `=SUM(E2:E)`  
- Total Cups Sold: `=SUM(C2:C)`  
- Average Price: `=AVERAGE(D2:D)`

**Why this matters:** These metrics are often included in reports and dashboards. They give a snapshot of overall performance.

<br>

### 20. **Build a Dashboard Sheet**

A dashboard is like a **control panel** for your dataset. It pulls together the most important numbers, summaries, and charts into one clean, visual space — so that anyone (even someone who hasn’t seen your raw data) can quickly understand what’s going on.

In this step, you'll create a **dedicated dashboard sheet** within your Google Spreadsheet. This sheet won't have raw rows or formulas scattered everywhere. Instead, it's a clean page with clear visuals, summaries, and insights — just like something you'd present to your manager, client, or investor.

<br>

#### 🔧 What to include in your dashboard:

Think of it like a mini data report. Your dashboard should include:

- ✅ **Headline numbers** (called KPIs – Key Performance Indicators)  
  - Total cups sold  
  - Total revenue  
  - Average price per cup  
  - Most popular flavor  

- 📊 **Charts**  
  - A bar chart showing sales by flavor  
  - A line chart showing sales over time  
  - A pie chart showing revenue share by flavor  

- 📌 **Important observations**  
  - Add text boxes or notes like:  
    “Fridays had the highest average revenue.”  
    “Mint flavor underperformed consistently.”

<br>

#### 🪜 How to build it step-by-step:

1. **Create a new sheet**  
   - At the bottom, click the "+" button to add a new sheet  
   - Rename it to something like `Dashboard` or `Summary`

2. **Link key values from your main sheet**  
   - Go to a clean cell and type:  
     `='Sheet1'!E101`  
     (Replace `Sheet1` and `E101` with the actual sheet name and cell you want to pull from)  
   - This pulls over your total revenue, total cups, average price, etc.  
   - Label these numbers clearly (e.g., use Column A for labels like “Total Revenue” and Column B for values)

3. **Insert your charts**  
   - Go back to the sheet where you created your bar, line, and pie charts  
   - Click on the chart, then press `Ctrl+C` (or right-click > Copy)  
   - Go to the `Dashboard` sheet and press `Ctrl+V` to paste them  
   - Resize and arrange them neatly (use white space wisely!)

4. **Format for clarity**  
   - Use bold fonts, borders, or background colors to make sections distinct  
   - Keep it clean — avoid clutter or unnecessary formulas  
   - You can add icons or emojis in labels to make things more visual

5. **Add quick insights (optional)**  
   - Use merged cells to write brief takeaways like:  
     “🟢 Sales peaked on weekends — promote on Friday!”  
     “🔴 Mint sales dropped 3 weeks in a row — consider removing.”

<br>

#### 🧠 Why this matters:

A dashboard helps transform your spreadsheet from a bunch of numbers into a **story**. It answers questions quickly:

- What happened?  
- What’s working well?  
- What should we improve?

This is what decision-makers care about — not rows and rows of data, but **what the data means**.

Even if you're just starting out, learning to build a simple dashboard shows you're thinking like a real analyst: clear, concise, and audience-focused.

> Your dashboard is the bridge between data and decision-making.


<br>

## 🧠 Self-Reflection Prompts

At the end of your analysis, take a few moments to reflect. This builds critical thinking — a core analyst skill.

Answer these questions in your sheet or in a separate journal:

- What did I learn from cleaning this data?
- What surprised me during my exploration?
- Were there any rows that confused me or seemed wrong?
- How would I explain this dataset to someone unfamiliar with it?
- What advice would I give Lina to improve her business?
- Which chart told the clearest story? Why?
- Where might bias or incorrect assumptions exist in the data?
- What would I do differently next time?

<br>
<br>


## 📘 Summary

By completing this spreadsheet mini-project, you’ve gone through an entire data workflow:

- You imported and organized raw data (Extract)
- You cleaned and structured the data (Transform)
- You analyzed and visualized insights (Load and Reflect)

> You’re not just following instructions — you’re building data intuition and learning to think like an analyst.

Next up: Let’s tell your data story using Canva or PowerPoint!  
Turn your spreadsheet into a real portfolio piece. 🎓

