## Week 1 — 23/09/2026

Goal: Brainstorm a solid finance assistant concept, generate safe mock transaction data, and complete the design phase (Steps 1 to 4: Problem, I/O, Manual Math, and Pseudocode) before jumping into any Python code.

AI prompt used:

"Act as a senior Python mentor. I am a university student working on a finance assistant project. Help me draft the Business Problem, Inputs/Outputs, and Pseudocode for a CommBank-style 'Budget Breaker' app. Also, generate 30 rows of mock bank statement data in CSV format for a student living in Perth. Finally, give me a good structure for the README file."

What it returned:
The AI generated a highly realistic 30-row CSV dataset featuring local Perth spots. It also provided a clear breakdown for Steps 1-4 and a very professional, comprehensive README template including a features checklist and tech stack.


What I kept: I kept the mock CSV data entirely because it accurately reflects my real spending habits as a Curtin student while protecting my actual banking privacy. I also kept the core structure of the Business Problem and Inputs/Outputs as it perfectly framed my idea.

What I changed: I modified the AI's README template to match my actual repository. For example, I changed the suggested filename from finance_assistant.ipynb to ISYS2001_A2.ipynb and updated the feature status since I haven't coded them yet.
<img width="1020" height="899" alt="image" src="https://github.com/user-attachments/assets/57711674-e884-44d0-97ed-dabec5b86b2d" />


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

## 07/10/2026

---

### 1. Keeping the code inside what we were taught

**What I asked the AI:**
> I gave it the unit's module list (Modules 2-5 basics, Module 7 pandas with
> `read_csv` and `.sum()`, Module 8 API calls, Module 9 Gradio, Module 10-11 testing)
> and asked it to check whether my code went beyond that scope.

<img width="574" height="887" alt="image" src="https://github.com/user-attachments/assets/7234c15f-25f2-4775-adca-4c2828fd215f" />


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

<img width="1203" height="260" alt="image" src="https://github.com/user-attachments/assets/507fbd16-796b-4f3c-89af-ffb77cb9a92b" />

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

<img width="243" height="67" alt="image" src="https://github.com/user-attachments/assets/c3c4b71a-a661-4bd0-81e4-33f2aa3939ca" />

**Problem 4 — was it my key?**
Next I called the models endpoint to check the key itself:

```python
url = f"https://generativelanguage.googleapis.com/v1beta/models?key={GEMINI_API_KEY}"
r = requests.get(url, timeout=30)
```

This returned `200` and a full list of available models. So the key works, the network
works, and the problem was specific to the `generateContent` endpoint.

<img width="344" height="703" alt="image" src="https://github.com/user-attachments/assets/371bc519-e6f8-419e-aad7-e7c174efa2d8" />

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

## 11 October 2026

**Goal:** Get the Gemini assistant responding reliably after days of 503, timeout
and 429 errors.

**What fixed it:** switching the model from `gemini-2.5-flash` to `gemini-3.8-flash`.
One line changed:

```python
GEMINI_URL = f"https://generativelanguage.googleapis.com/v1beta/models/gemini-3.8-flash:generateContent?key={GEMINI_API_KEY}"
```

**Where the idea came from:** a classmate mentioned they had the same problem and that
switching to a newer model worked for them. I did not take it on trust — I already had
the list of models my key can reach, from the diagnostic I ran during the earlier
debugging session:

```python
url = f"https://generativelanguage.googleapis.com/v1beta/models?key={GEMINI_API_KEY}"
r = requests.get(url, timeout=30)
```

`gemini-3.8-flash` was in that list, so I knew it was a real model available to my key
rather than a name someone half-remembered. That is why I was willing to try it.

<img width="1544" height="313" alt="image" src="https://github.com/user-attachments/assets/4ccdd35b-5c12-42f3-b1be-6ca9b986af47" />


**Why I think it worked:** the free tier appears to count quota per model, so the newer
model had headroom the one I had been hammering did not. My last error before this was
429 (rate limit), not a code error — so the fix was never going to be in my own logic.

**The sequence of models I went through, and why each time:**

| Model | Why I moved on |
| --- | --- |
| `gemini-flash-latest` | The alias the unit spec suggests. Returned 503 and timeouts repeatedly |
| `gemini-2.5-flash` | A specific stable model, to rule out the alias pointing somewhere busy. Still hit 429 |
| `gemini-3.8-flash` | Confirmed available to my key via the models endpoint. Works |

**A trade-off I am accepting:** the spec recommends the `gemini-flash-latest` alias
because it keeps you on the current free Flash model as Google updates it. I have pinned
a specific model instead, which means it will not follow those updates. I decided a
model that responds today is worth more than one that auto-updates but does not answer.
The retry loop stays in regardless, since a working model can still have a bad minute.

**What I would do differently:** spend less time assuming the fault was mine. I rewrote
the retry logic several times before running the diagnostic that showed the key and
network were fine. Checking what was actually reachable, earlier, would have saved a
session.

## Week 4 — 12 October 2026

**Goal:** Review the whole notebook for hidden bugs, fix the Gradio crash, and make
the Gemini assistant reliable enough that it does not depend on one model staying up.

Everything that broke today was outside my budget logic. The calculations and tests
were fine; the problems came from the Colab environment, the Gradio library, and
depending on an external API.

---

### 1. Code review: bugs the tests never caught

**What I asked the AI:**
"Here is my core break_down_budget function and my current test suite. Please act as a QA tester and review my code one last time. Are there any edge cases, weird inputs, or user errors that my code would crash on, which my current tests have missed?"

**What it returned:** Five problems, none of which my tests had caught:

| Problem | Why it matters |
|---|---|
| `gr.Number` returns `None` when the box is cleared, and `None <= 0` raises a `TypeError` | The app crashes on an input a real user could easily give |
| The Gradio error always said "check it has Amount and Category columns", even when the real cause was income of 0 | Misleading message sends the user to fix the wrong thing |
| `time.sleep()` ran after the **last** retry attempt as well | 30 seconds of waiting for nothing before giving up |
| The API key sat in the URL, so a failed request could print it in the error | A key leaked into a traceback is a leaked key |
| Markdown headings written `##3.` with no space | They render as plain text, not headings |

**What I kept:** all five. I had no counter-argument to any of them — each one was a
case I simply had not thought to test.

**What I learned:** passing tests does not mean the code is correct. My tests only
covered the inputs I had imagined. The `None` income bug is the clearest example:
every assert passed, and the app still crashes if someone clears a box.

---

### 2. The Gradio crash: `module 'gradio' has no attribute 'Request'`

**The problem:** the Gradio cell crashed inside the library itself, not in my code.
The traceback ran through `gradio/flagging.py` while Gradio was setting up its Flag
button.

<img width="601" height="161" alt="image" src="https://github.com/user-attachments/assets/e6f6fd9f-c4c9-4c28-8f06-96e8d7dbe5ce" />


**How I read it:** every line in the traceback was inside the library. That is the
signal that the problem is the environment, not my logic — a version loaded in memory
that did not match the one installed on disk.

**What I did:** restarted the runtime, and added `flagging_mode="never"` to
`gr.Interface` since my app has no use for a Flag button. I also added `debug=True`
to `launch()` so errors raised inside the app are actually shown rather than
swallowed.

**What I learned:** read where the traceback *stops being my code*. If none of it is
mine, stop editing my code.

---

### 3. The Gemini timeout, and why the app still worked

**The problem:** the quick test at the bottom of the Gemini cell failed with
`Read timed out (read timeout=60)`. The retry loop then waited and tried again, so I
stopped the cell manually and got a `KeyboardInterrupt`.

**What confused me:** the Gemini cell failed, but the assistant still worked inside
Gradio. That made no sense to me at first.

**What I worked out:** Python runs a cell top to bottom. `ask_assistant` was already
*defined* before the last line — the test call — got stuck. So the function existed
in memory regardless of whether the test call succeeded. The timeout was a temporary
problem on Google's side, not a broken function.

**The arithmetic that made the delay obvious:** 3 attempts × 60s timeout, plus waits
of 10 + 20 + 30 seconds, is about 4 minutes in the worst case. That is why the app
had been appearing to hang.

---

### 4. The assistant's reply showed raw Markdown

**The problem:** replies appeared in the app as `**$222 over budget**` — asterisks and
all.

**The cause:** Gemini writes in Markdown, but `gr.Textbox` only displays plain text.

**What I did:** changed that output to `gr.Markdown`, so bold text and bullet points
render properly.

---

### 5. Rewriting the assistant around model fallback

 I split the Gemini code into two functions:

- `call_gemini(model, payload)` — sends one request to one model, returns the reply
  text, or `None` if it fails
- `ask_assistant(message, budget_summary)` — builds the request, then loops through
  `GEMINI_MODELS` until one works. If every model fails, it returns `offline_advice()`
  built from the user's real numbers

I also moved the API key out of the URL and into the request headers, so it cannot
appear in an error message.

**What I deliberately kept simple:** I still unwrap the JSON one layer at a time, and
the fallback is a plain `for` loop with `if`/`else` and `try`/`except`. A cleverer
version would have been shorter, but I need to explain every line in the oral defence.

---

### 6. Errors 404 and 429, and why my first backup list was useless

My first attempt at a backup list still failed:

- **404 (Not Found)** — I had *guessed* model names like `gemini-1.5-flash-8b`. Guessed
  names are not real models.
- **429 (Too Many Requests)** — `gemini-3.8-flash` had hit the free tier limit, because
  I had run the cell many times while testing.

**The thing I actually found interesting:** `gemini-flash-latest` returned 429 at the
same moment as `gemini-3.8-flash`. A `latest` name is only an alias pointing at a real
model, so it draws on the *same quota*. As a backup it is worthless — it fails at
exactly the same time as the model it points to.

**What I did instead of guessing:** asked the API which models my key can actually
reach, and picked five stable flash models from that list, each with its own quota:

```python
GEMINI_MODELS = [
    "gemini-3.8-flash",
    "gemini-3.5-flash-lite",
    "gemini-3.7-flash",
    "gemini-3.1-flash-lite",
    "gemini-3.6-flash"
]
```

I left out preview, pro, tts, image and alias models — unstable, lower free limits, or
they do not return text at all.

---

### 7. The result

In the final test, `gemini-3.8-flash` still returned 429, and the app switched to
`gemini-3.5-flash-lite` on its own and replied in under a second.

I asked it how long it would take me to buy a $3000 car. It used my real figure —
$778.15 left over per month — and answered about 4 months (3000 ÷ 778.15 ≈ 3.86).
That is the fallback design and the data grounding both working in one answer.

<img width="1140" height="641" alt="image" src="https://github.com/user-attachments/assets/07741540-8d22-48c3-8ed0-ea2938d0cecf" />


### 8. The bug that only existed on a fresh run

When I rewrote the Gemini cell around `call_gemini()`, I accidentally deleted
`PERSONA` and `offline_advice()`. The app still worked for me, because both were
still in my Colab session's memory from the earlier version of the cell. On a
fresh runtime, it crashed with `NameError: name 'PERSONA' is not defined` as soon
as a question was asked.

This was the same thing I noticed earlier today (section 3), where a function kept
working after its cell failed, but this time it hid a real bug instead of saving me.

**What I changed:** I now test with Runtime → Disconnect and delete runtime →
Run all. Restarting is not enough, because it does not clear the old variables.
This is the only way to see the notebook the way my marker will.

### 9. Removing the test call from the Gemini cell

I had a quick off-topic test at the bottom of the Gemini cell:
`print(ask_assistant("Where is Perth city?"))`. I removed it, because it
fired a real API request every time the notebook ran, which slowed down "Run all"
and used up free-tier quota before the app was even open.

I still checked the off-topic behaviour once, through the Gradio interface. The
assistant answered briefly and steered the conversation back to budgeting, which
is what the persona instruction asks for.

<img width="1044" height="460" alt="image" src="https://github.com/user-attachments/assets/2c79d4ec-0366-4cbc-ae2f-c451002e10bc" />

---

### Reflection

A program that depends on an external API can never be completely reliable, because I
do not control the server. What I control is how my program behaves when the API
fails. With a list of backup models and an offline fallback, the user always gets a
useful answer and the app never crashes.

The other lesson is to read error codes as information rather than noise. 404 means
the thing does not exist — stop guessing names. 429 means too many requests — stop
hammering it. Both told me exactly what to do, once I stopped treating every failure
as the same failure.

---

