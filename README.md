# Budget Breaker 💸

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
2023-10-01,18.50,Rent,Weekly share house rent portion
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
        'Rent': 92.5,
        'Coffee': 27.8,
        'Groceries': 269.95,
        'Transport': 24.0,
        'Eating Out': 98.7,
        'Subscriptions': 60.0,
        'Entertainment': 52.0
    },
    'category_percentages': {
        'Rent': 6.17,
        'Coffee': 1.85,
        'Groceries': 17.99,
        'Transport': 1.6,
        'Eating Out': 6.58,
        'Subscriptions': 4.0,
        'Entertainment': 3.47
    },
    'total_spent': 624.95,
    'remaining_balance': 875.05,
    'goal_achieved': True
}
```

If the file is missing, a required column is missing, the file is empty, or the input numbers don't make sense, the function prints a clear error message and returns `None` instead of crashing.

## How to run

1. Open `ISYS2001_A2.ipynb` in Google Colab.
2. Run all cells top to bottom (`Runtime > Run all`).
3. When prompted, enter your free Gemini API key from [Google AI Studio](https://aistudio.google.com/) — the notebook uses `getpass` so it's never saved into the notebook or repository.
4. Upload your own `transactions.csv` (or use the sample in `data/`) into the Gradio interface, set your income and savings goal, and click "Analyse my budget".
5. Type a question in the chat box and click "Ask" — the assistant's replies are grounded in your uploaded data.

## Tech stack

- **pandas** — loading and analysing CSV transaction data
- **Gemini API** (`gemini-flash-latest`) — the conversational finance assistant
- **Gradio** — the user interface
- Python `assert`-based tests for validation

## Repository structure

```
├── ISYS2001_A2.ipynb      # Main notebook (six-step method, code, chatbot, Gradio, tests)
├── data/
│   └── transactions.csv   # Sample transaction data
├── DIARY.md                # Developer's Diary
└── README.md               # You are here
```

## Author

Nguyen Hoang Nguyen (Azit) — Business Information Systems, Curtin University
