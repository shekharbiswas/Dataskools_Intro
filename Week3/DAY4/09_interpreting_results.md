# 🧾 What does “statistically significant” mean?

Let’s say you run an A/B test:
- Version A = 5 signups out of 100 people
- Version B = 8 signups out of 100 people

It looks like **B is better**… but is the difference **real** — or could it be **just random**?

That’s where **p-values**, **significance**, and **confidence** come in.

<br>

## 🍋 Lina’s real question

Lina tries a new recipe:
- Week 1 (old flavor): sold 60 cups  
- Week 2 (new flavor): sold 65 cups  

She asks:
> “Is this new recipe truly better? Or was it just a lucky week?”

The answer depends on:
- **How big the difference is**
- **How consistent the pattern is**
- **How much randomness is involved**

<br>

## 🧠 Key concepts (no scary math)

| Concept                 | What it means                                           | Example                                             |
|--------------------------|--------------------------------------------------------|-----------------------------------------------------|
| **P-value**             | The chance the result was just luck                    | p = 0.03 means only 3% chance result was random     |
| **Statistical significance** | The result is unlikely due to chance (usually p < 0.05) | Lina’s 5-cup difference was *probably* real         |
| **False positive**      | Thinking something works when it actually doesn’t       | Lina promotes mint, but sales drop later            |
| **Confidence level**    | How sure we are in the result (e.g., 95%)              | “I’m 95% sure this result is real”                  |

<br>

## 📊 When is something significant?

Usually, a **p-value under 0.05** (5%) means the result is **statistically significant** —  
That is, it’s probably **not just random chance**.

But remember:
- Significance doesn’t mean **it’s useful**
- A result can be real but **too small to matter**
- A/B tests can give **false positives** if repeated too much

<br>

## 🧪 Lina's test example

She tries two signboard messages:

| Version | Customers | Sales |
|---------|-----------|-------|
| A       | 100       | 10    |
| B       | 100       | 13    |

That’s a 30% increase — looks great!

But…
- Were those **different days** with different weather?
- Was that **just one weekend** or **multiple weeks**?
- If we repeated this, would we see it again?

To **trust** the result:
- The test needs to be **fair**
- The difference needs to be **large enough**
- We need **enough data** to be confident

<br>

## 🎯 Key lesson

> Not every change that “works once” is a winner.  
> Test again. Look at patterns. Be confident before scaling.

This is why data people say:
> “Is it statistically significant?”  
> It’s not nitpicking — it’s avoiding false promises.

<br>

## 💬 Reflection prompts

- Have you ever made a change based on a single outcome?
- Why is “real” different from “lucky” in experiments?
- How much confidence do you usually need before taking action?

<br>

## ✅ Self-check questions

<details>
<summary>🧾 What is a p-value?</summary>
It’s the probability that your result happened just by chance.
</details>

<details>
<summary>🧠 What is statistical significance?</summary>
It means your result is strong enough that it’s probably not due to randomness — often p < 0.05.
</details>

<details>
<summary>📉 What’s a false positive?</summary>
It’s when you think a change worked, but it actually didn’t — it was just luck or a mistake.
</details>

<details>
<summary>📈 What does “confidence” mean in A/B testing?</summary>
It’s how sure you are that the result is real — usually 90%, 95%, or 99%.
</details>

<br>

## 🏁 Summary

When running tests:
- Check if the difference is **real**
- Avoid acting on **random patterns**
- Learn to trust **patterns over time**, not just one win

📅 This brings us to the **end of Week 3** — now you’ve explored:
- Probability  
- Sampling and bias  
- A/B testing  
- How to know if your result is worth trusting

<br>

## 🔄 What’s next?

Week 4 starts with a reflection:

> “Now that you’ve seen how data works…  
> How do *you* think like an analyst?”

We’ll guide you through:
- Practicing interpretation and ethics  
- Building your own data story

