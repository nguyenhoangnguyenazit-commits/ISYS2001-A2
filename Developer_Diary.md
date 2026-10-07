## Week 1 — 23/09/2026

Goal: Brainstorm a solid finance assistant concept, generate safe mock transaction data, and complete the design phase (Steps 1 to 4: Problem, I/O, Manual Math, and Pseudocode) before jumping into any Python code.

AI prompt used:

"Act as a senior Python mentor. I am a university student working on a finance assistant project. Help me draft the Business Problem, Inputs/Outputs, and Pseudocode for a CommBank-style 'Budget Breaker' app. Also, generate 30 rows of mock bank statement data in CSV format for a student living in Perth. Finally, give me a good structure for the README file."

What it returned:
The AI generated a highly realistic 30-row CSV dataset featuring local Perth spots (Coles Karawara, Transperth, Curtin Guild Cafe). It also provided a clear breakdown for Steps 1-4 and a very professional, comprehensive README template including a features checklist and tech stack.


What I kept: I kept the mock CSV data entirely because it accurately reflects my real spending habits as a Curtin student while protecting my actual banking privacy. I also kept the core structure of the Business Problem and Inputs/Outputs as it perfectly framed my idea.

What I changed: I modified the AI's README template to match my actual repository. For example, I changed the suggested filename from ISYS2001_A2.ipynb to finance_assistant.ipynb and updated the feature status since I haven't coded them yet.

What I rejected: I completely rejected the AI's attempt to give me the actual Python code this week. My strict goal was to finish the pseudocode and design logic first. I will tackle the actual coding and Pandas implementation in Week 2 so I can fully understand how to build the tool myself.


## Week 2 — 03/10/2026

Goal: Translate the pseudocode into actual Python code using pandas, run automated tests with the mock CSV file, and finalize the README document for the repository.

AI prompt used:

"Write the Python code for the break_down_budget function based on my pseudocode from Step 4. Include data validation, try-except blocks, and assert tests for both normal cases and edge cases (like a missing file or bad data)."

What it returned:
The AI provided the full Python function utilizing pandas.read_csv() and groupby(), along with a robust testing block checking for missing files, empty data, and incorrect data types.

What I kept: I kept the core pandas logic for the budget calculations, especially the to_dict() conversion and the dictionary comprehensions, because they were incredibly clean and efficient for calculating the category percentages.

What I changed: I updated the test values to accurately reflect the new Rent ($290.00) and total spent ($343.60) from my revised manual calculations. More importantly, I had to troubleshoot and fix several runtime errors myself: I hit a NameError because I hadn't run the import cell first, an AssertionError because I needed to properly upload my transactions.csv to Colab's /content/ folder, and a SyntaxError caused by overlapping code. Documenting and fixing these taught me a lot about how the Colab environment actually works.

What I rejected: I deliberately rejected the AI's provided code for the Gemini API and Gradio interface for now. My strategy is to hold off on those features until Weeks 3 and 4 so I can focus purely on mastering the backend data analysis and ensuring my GitHub commit history shows steady, logical progression.


## Week 3 — 06/10/2026

Goal: Integrate the Google Gemini API and Gradio interface into the project while ensuring all code is beginner-friendly, explicable, and the API key is securely hidden from GitHub.

AI prompt used:

"Help me simplify the Gemini API call and Gradio interface code. Remove any complex 'black box' syntax like inline if-statements or 1-line JSON extractions. Also, how do I securely store my API key in Google Colab without hardcoding it, so it doesn't get stolen when I push my code to GitHub?"

What it returned:
The AI provided a much simpler, step-by-step approach to extracting the Gemini JSON response. It replaced the inline if-statements in the Gradio block with standard if-else conditionals. Most importantly, it introduced google.colab.userdata to securely retrieve the API key from Colab's Secrets manager, and added a for loop with try-except to handle server timeouts by retrying up to 3 times.


What I kept: I kept the core requests.post() structure and the Gradio UI layout because they align perfectly with Modules 8 and 9. The use of Colab Secrets (userdata.get()) is brilliant and exactly what I needed to keep my repository secure.

What I changed: I completely rewrote the JSON parsing section. Instead of a massive 1-line extraction (r.json()["candidates"][0]...), I broke it down variable by variable (candidates_list, first_candidate, content_dict, etc.). This makes the code slightly longer but guarantees I can explain every single line during my Assessment 3 oral defense. I also implemented the 3-attempt retry loop to handle network issues gracefully.

What I rejected: I strictly rejected any advanced Python shortcuts or "black box" techniques suggested earlier by the AI. If I couldn't confidently explain the logic using the core concepts taught in ISYS2001 (variables, loops, conditionals, basic dicts), I threw it out.

## Week 3 07/10/2026

---

### 1. Keeping the code inside what we were taught

**What I asked the AI:**
> I gave it the unit's module list (Modules 2-5 basics, Module 7 pandas with
> `read_csv` and `.sum()`, Module 8 API calls, Module 9 Gradio, Module 10-11 testing)
> and asked it to check whether my code went beyond that scope.

[SCREENSHOT: <img width="574" height="887" alt="image" src="https://github.com/user-attachments/assets/7234c15f-25f2-4775-adca-4c2828fd215f" />]


**What it returned:** It flagged four things — `raise ValueError(...)`, `df.groupby()`,
`pd.to_numeric(errors="coerce")` with `.isna().any()`, and a dictionary comprehension.

**What I did with it:**
I kept all four changes, because I agreed with the reasoning. The unit spec lists
"error handling with try and except" but never mentions `raise`, so I replaced every
`raise` with a plain `if` check that prints a message and returns `None`. That also
made my error handling consistent across the whole function instead of mixing two styles.

For `groupby()`, the spec says "pandas or your own dictionary and loop logic are both
fine", so I rewrote the category totals using a dictionary and a `for` loop that adds
each amount to the right key. It is more lines, but I can walk through every one of them.

**What I rejected:**
At one point I was given a version that used `df.iterrows()` to validate the Amount
column. I rejected it, because swapping one unfamiliar pandas method for another is not
actually simplifying anything — and it reintroduced `raise`, which I had just removed.
I went with converting the column to a plain Python list and looping over it with
`float()` inside a `try/except` instead.

---

### 2. The API key — getting it out of my code

**What I asked the AI:**
> How to handle the Gemini API key safely, since the spec says to keep it out of the
> notebook and the repository.

**What I did with it:**
I started with `getpass`, which asks for the key each run. That works, but it is annoying
to retype every time the runtime restarts. I switched to Colab Secrets
(`from google.colab import userdata`), which stores the key in my Google account rather
than in the file.

**My reasoning:** hardcoding the key into the notebook would be the easy option, but
bots scan public GitHub repos for exposed keys within seconds, and Google revokes any key
it detects as leaked. If that happened, my marker would clone my repo, run it, and get a
connection error — through no fault of the code itself.

**Trade-off I accepted:** `userdata` only exists inside Colab, so this notebook will not
run as-is on a local machine. Given the unit expects Colab, I decided that was fine.

---

### 3. Debugging the assistant (the long one)

This took most of the session. The assistant kept failing, and the error changed each time.

**Problem 1 — timeouts.**
First error was `Read timed out (read timeout=15)`. I asked the AI why this happens so
often. It suggested the free tier can take longer than 15 seconds to respond, especially
on the first call. I raised the timeout to 30 and added a retry loop:
`for attempt in range(3)` with `time.sleep(2)` between attempts.

[SCREENSHOT: <img width="1203" height="260" alt="image" src="https://github.com/user-attachments/assets/507fbd16-796b-4f3c-89af-ffb77cb9a92b" />]

**Problem 2 — 503 Service Unavailable.**
Then I started getting `503`, which is a server-side error, not a code error. Added a
separate branch for it so the message tells me what actually happened instead of showing
a raw exception.

**Problem 3 — was it even my network?**
Instead of guessing, I tested the connection directly:

```python
r = requests.get("https://generativelanguage.googleapis.com", timeout=10)
print("Reached Google, status:", r.status_code)
```

This returned `404`, which confirmed my network reaches Google fine — a 404 just means
that bare domain has no page. So the problem was not my internet or a firewall.

[SCREENSHOT: <img width="243" height="67" alt="image" src="https://github.com/user-attachments/assets/c3c4b71a-a661-4bd0-81e4-33f2aa3939ca" />]

**Problem 4 — was it my key?**
Next I called the models endpoint to check the key itself:

```python
url = f"https://generativelanguage.googleapis.com/v1beta/models?key={GEMINI_API_KEY}"
r = requests.get(url, timeout=30)
```

This returned `200` and a full list of available models. So the key works, the network
works, and the problem was specific to the `generateContent` endpoint.

[SCREENSHOT: <img width="344" height="703" alt="image" src="https://github.com/user-attachments/assets/371bc519-e6f8-419e-aad7-e7c174efa2d8" />]

**Problem 5 — the actual cause.**
I switched from the `gemini-flash-latest` alias to `gemini-2.5-flash` to rule out a
problem with that one model. The next run came back `429 Too Many Requests` — which
told me something useful: the endpoint was alive the whole time. I had simply been
hammering it. Every debugging run fired three to five requests, and I had run it many
times in a row, so I had hit the free tier's rate limit. Once rate-limited, some requests
hang instead of returning immediately, which is where the timeouts were coming from.

**What I learned:** my retry loop was partly making things worse. Three automatic retries
per click burns three times the quota. The code change was adding a longer wait for `429`
specifically; the real fix was behavioural — stop testing in rapid bursts, and space out
my clicks when demonstrating the app.

---

### 4. Making the JSON parsing explainable

**What I asked the AI:**
> I told it the line `r.json()["candidates"][0]["content"]["parts"][0]["text"]` was a
> black box to me and I could not defend it in the viva.

**What I did with it:**
I broke it into one variable per layer:

```python
data = r.json()
candidates_list = data["candidates"]
first_candidate = candidates_list[0]
content_dict = first_candidate["content"]
parts_list = content_dict["parts"]
final_text = parts_list[0]["text"]
```

Same result, six readable steps. Gemini returns a dictionary; `candidates` holds a list
of possible replies; `[0]` takes the first; inside that is `content`, which holds `parts`,
another list; the first part has the key `text`, which is the actual answer.

I also ran `print(data)` once to see the raw structure on screen rather than trusting a
description of it. That was worth doing — reading the real nesting made the indexes
obvious in a way the explanation alone did not.
