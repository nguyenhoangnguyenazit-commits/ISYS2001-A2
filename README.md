# Budget Breaker 

A small finance assistant built for **ISYS2001 – Introduction to Business Programming** (Assessment 2, Curtin University).

## The problem

As a university student living in Perth, it's easy to lose track of minor daily expenses like coffee, transport, and eating out. By the end of the month I often don't know where my money went, or whether I hit my savings target.

**Budget Breaker** processes a month of transaction data, categorises spending, works out what percentage of income each category eats up, and tells the user whether they hit their savings goal.

**Target user:** University students who want a quick, no-fuss way to track living expenses and savings without manually going through every receipt.

## Features

| Feature | Status |
|---|---|
| Custom budget breakdown tool (`break_down_budget()`) | ✅ Done |
| Data validation & error handling (bad file, missing columns, empty data, invalid input) | ✅ Done |
| Analysis grounded in the user's own uploaded data | ✅ Done |
| Conversational assistant powered by Gemini (finance persona) | ✅ Done |
| Gradio interface | ✅ Done |
| Automated tests (assert-based, happy path + edge cases) | ✅ Done |

## Sample input

`data/transactions.csv`:

```csv
Date,Amount,Category,Description
2023-10-01,290.00,Rent,Weekly share house rent portion
2023-10-01,5.20,Coffee,Flat white at uni cafe
2023-10-02,42.30,Groceries,Woolworths weekly shop
...
```

Called with:

```python
result = break_down_budget("data/transactions.csv", monthly_income=1500, saving_goal=300)
```

## Sample output

```python
{
    'category_totals': {
        'Rent': 1160.0,
        'Coffee': 33.9,
        'Groceries': 269.95,
        'Transport': 28.8,
        'Eating Out': 117.2,
        'Subscriptions': 60.0,
        'Entertainment': 52.0
    },
    'category_percentages': {   # rounded to 2 decimal places here
        'Rent': 77.33,
        'Coffee': 2.26,
        'Groceries': 18.0,
        'Transport': 1.92,
        'Eating Out': 7.81,
        'Subscriptions': 4.0,
        'Entertainment': 3.47
    },
    'total_spent': 1721.85,
    'remaining_balance': -221.85,
    'goal_achieved': False
}
```

If the file is missing, a required column is missing, the file is empty, or the input numbers don't make sense, the function prints a clear error message and returns `None` instead of crashing.


## How to run

1. Open `ISYS2001_A2.ipynb` in Google Colab.
2. Add your Gemini API key to Colab Secrets before running: click the key icon (🔑) in the left sidebar, add a secret named exactly `GEMINI_API_KEY`, paste in your free key from [Google AI Studio](https://aistudio.google.com/), and switch on "Notebook access". The key is read with `userdata.get()`, so it never appears in the notebook or the repository.
3. Run all cells top to bottom (`Runtime > Run all`).
4. In the Gradio interface at the bottom, upload a transactions CSV (or use the sample in `data/`), set your monthly income and savings goal, optionally type a question for the assistant, then click **Submit**.
5. You get two results side by side: the budget breakdown, and the assistant's reply — grounded in the numbers from the file you just uploaded.

**Note:** the app does not need the API key to run. If no key is set, or if Gemini is unreachable, the assistant falls back to basic advice calculated from your own data rather than failing.
Tech stack
markdown
## Tech stack

- **pandas** — loading the CSV (`pd.read_csv`); the category analysis itself is written with plain dictionaries and loops
- **Gemini API** (`gemini-3.8-flash`, called directly with `requests`) — the conversational finance assistant
- **Gradio** — the user interface
- Python `assert`-based tests for validation
Repository structure
markdown
## Repository structure

```
├── ISYS2001_A2.ipynb      # Main notebook (six-step method, code, chatbot, Gradio, tests)
├── data/
│   └── transactions.csv   # Sample transaction data
├── DIARY.md               # Developer's Diary
└── README.md              # You are here
```

## Author

Nguyen Hoang Nguyen (Azit)
