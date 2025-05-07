# 🍋 Meet Lina and her Google Spreadsheet

<br>

<img width="400" alt="image" src="https://github.com/user-attachments/assets/5a979833-bf60-48a8-b595-61eb1ea73b1d" />

<br>

Lina runs a small weekend lemonade stand and started tracking her sales in **Google Sheets** — just like many real businesses do in their early stages.

She logs:
- The date of each sale  
- Which **flavor** was sold  
- How many **cups** were sold  
- The **price** per cup  
- Any **customer notes**

Here’s a **sample of her spreadsheet before cleaning**:

| Date       | Flavor   | Cups | Price | Notes            |
|------------|----------|------|-------|------------------|
| 2024-06-01 | Lemon    | 10   | 1.5   | -                |
| 2024-06-01 | lemon    | 5    | 1.5   | Repeat customer  |
| 2024-06-01 | Mint     |      | 1.8   | forgot quantity  |
| 2024-06-02 | GINGER   | 7    | 2     | typo in flavor   |
|            | Lemon    | 10   | 1.5   | Missing date     |
| 2024-06-03 | Lemon    | 10   | 1.5   | -                |
| 2024-06-03 | Lemon    | 10   | 1.5   | - (duplicate row)|
| 2024-06-04 | mint     | 8    | 1.8   | lowercase flavor |

You can view or copy the spreadsheet here:  
🔗 [Sample Google Sheet](https://docs.google.com/spreadsheets/d/1gq9DEMO123/edit?usp=sharing) *(replace this with actual link)*

<br>

## 🧼 What is messy data?

Messy data is any data that:
- Has **inconsistent values** (e.g., "lemon", "Lemon", "LEMON")
- Has **missing cells** (e.g., no quantity, no date)
- Has **duplicates** (same row entered more than once)
- Uses **different formats** (e.g., date in wrong format, prices in text)

Just like a cluttered kitchen slows down cooking, messy data **slows down or misleads analysis**.

<br>

## ✂️ Cleaning steps Lina should take

Here’s how Lina can start cleaning her data — step by step in Google Sheets.

<br>

### 1. Fill in missing values

> "I forgot to write how many cups I sold for mint… and I missed a date entry!"

**How to fix it:**  
- Ask yourself: Is the data truly missing or just a mistake?
- If it's unrecoverable, mark it clearly (e.g., “MISSING” or leave it blank intentionally)
- If it's obvious (e.g., price repeated from earlier), fill it consistently

<br>


### 2. Fix formatting issues

> "Why do I have Lemon, lemon, and LEMON all over the sheet?"

**How to fix it:**
- Use consistent **capitalization** (e.g., convert all to “Lemon”)
- Standardize **price formats** (e.g., no extra spaces or symbols)
- Use **Data Validation** to restrict flavor names to a dropdown list

<br>

### 3. Remove duplicates

> "Looks like I pasted the same row twice on 2024-06-03."

**How to fix it:**
- Use Google Sheets: `Data > Data cleanup > Remove duplicates`
- Manually scan rows if data is small

---

### 4. Keep consistent categories

> "Sometimes I wrote 'Ginger', sometimes 'GINGER'. Which one is correct?"

**How to fix it:**
- Make a controlled list (e.g., Lemon, Mint, Ginger) and stick to it
- Fix typos, align categories

<br>


### 5. Organize with filters

> "My sheet is getting big — hard to find things!"

**How to fix it:**
- Use `Data > Create a filter` in Google Sheets
- Sort by date, filter by flavor, check for blanks

<br>

## 📝 What Lina’s cleaned sheet might look like

| Date       | Flavor | Cups | Price | Notes            |
|------------|--------|------|-------|------------------|
| 2024-06-01 | Lemon  | 10   | 1.5   | -                |
| 2024-06-01 | Lemon  | 5    | 1.5   | Repeat customer  |
| 2024-06-01 | Mint   | 6    | 1.8   | Estimated value  |
| 2024-06-02 | Ginger | 7    | 2     | Corrected flavor |
| 2024-06-03 | Lemon  | 10   | 1.5   | -                |
| 2024-06-04 | Mint   | 8    | 1.8   | -                |

Now Lina’s spreadsheet is:
- Easy to filter  
- Ready to explore  
- Safe to analyze

<br>

## 📄 Why clean data = reliable insights

Cleaning may feel boring — but it’s the **foundation of every analysis**.

Without cleaning:
- “Lemon” and “lemon” are seen as two different products
- Missing values lead to wrong totals or averages
- Duplicates inflate results

With clean data, Lina can confidently:
- Calculate how many cups of each flavor she sold  
- Compare prices  
- Spot daily or weekly trends

<br>


## ✅ Self-check questions

<details>
<summary>🧠 What are 3 common problems in messy data?</summary>
- Missing values  
- Inconsistent formats (e.g., Lemon vs lemon)  
- Duplicate rows  
</details>

<details>
<summary>✏️ Why should categories like flavor be standardized?</summary>
So that we don’t count the same thing multiple times under different names.
</details>

<details>
<summary>📊 How does cleaning help us later?</summary>
It ensures our analysis is accurate and makes trends visible.
</details>

<details>
<summary>📅 What should you do if a date is missing?</summary>
Try to recover it. If not, mark it clearly instead of guessing.
</details>

<br>


## 🏁 What’s next?

Now that Lina’s data is clean, she’s curious:
- Which flavors are most popular?  
- Do sales increase on weekends?  
- Does her pricing affect demand?

In the next chapter, we’ll help Lina explore her data — and uncover the story it tells.
