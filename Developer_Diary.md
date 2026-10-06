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
