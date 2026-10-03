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

What I kept / changed / rejected:

What I kept: I kept the core pandas logic for the budget calculations, especially the to_dict() conversion and the dictionary comprehensions, because they were incredibly clean and efficient for calculating the category percentages.

What I changed: I updated the test values to accurately reflect the new Rent ($290.00) and total spent ($343.60) from my revised manual calculations. More importantly, I had to troubleshoot and fix several runtime errors myself: I hit a NameError because I hadn't run the import cell first, an AssertionError because I needed to properly upload my transactions.csv to Colab's /content/ folder, and a SyntaxError caused by overlapping code. Documenting and fixing these taught me a lot about how the Colab environment actually works.

What I rejected: I deliberately rejected the AI's provided code for the Gemini API and Gradio interface for now. My strategy is to hold off on those features until Weeks 3 and 4 so I can focus purely on mastering the backend data analysis and ensuring my GitHub commit history shows steady, logical progression.
