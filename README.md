# AI-Assisted-Support-Ticket
Smart Support Ticket Sorting: Used the Claude API and Pandas to automatically categorize customer support requests into groups like billing, login issues, data, and integrations.

Auto-Drafted Replies: Generated instant initial responses for each ticket to speed up support agent workflows.

Accuracy Checking: Built a testing step comparing AI tags against human-labeled tickets to make sure the model was reliable before use.

Human-in-the-Loop: Flagged low-confidence predictions for manual review, saving time on easy tickets while keeping humans in control of tough ones.


### What Is This Project?

Imagine a company receiving hundreds of support emails every day. Human agents usually have to read every single email, figure out who should handle it (e.g., the billing team vs. the tech team), write a reply from scratch, and send it out. This takes a lot of time.

This project automates that entire process using Python and AI (Claude API):

1. It automatically **categorizes** incoming tickets into standard buckets (Auth, Billing, Integration, Data).
2. It **drafts a response** that a human agent can quickly edit and send.
3. It **checks its own accuracy** against real historical data so you know how trustworthy it is.
4. It **flags uncertain tickets** for human review so the AI never sends a wrong answer to a customer.

---

### Step-by-Step Breakdown of How It Works

#### 1. Loading and Managing Data (Pandas)

* **What you do:** Use the Python library **Pandas** to load customer tickets stored in a CSV or Excel file.
* **How it works:** Pandas turns raw text data into an organized table (a DataFrame) with columns like `Ticket_ID`, `Customer_Message`, `Actual_Category`, and `Predicted_Category`. This makes it easy to pass batches of text to the AI and save the results.

#### 2. Ticket Classification & Response Generation (Claude API)

* **What you do:** Send the text of each support ticket to the Claude API with a structured system prompt.
* **How it works:**
* You instruct Claude: *"Classify this ticket into one of four categories: Auth, Billing, Integration, or Data. Also give a confidence score from 0 to 100%, and write a polite first draft response."*
* Claude reads the ticket (e.g., *"I can't log in with my password"*), identifies it as `Auth`, generates a friendly response like *"Hi! Try resetting your password using this link..."*, and returns a confidence score (e.g., `95%`).



#### 3. Accuracy Validation Step

* **What you do:** Compare the AI's predictions against a set of manually labeled "ground truth" tickets.
* **How it works:**
* You calculate accuracy metrics: $\text{Accuracy} = \frac{\text{Correct AI Predictions}}{\text{Total Tickets}}$.
* This ensures the AI meets a minimum quality bar (e.g., 90%+ accuracy) before deploying it to handle actual customer inquiries.



#### 4. Human-in-the-Loop Safeguard

* **What you do:** Set up conditional logic based on confidence levels.
* **How it works:**
* **High Confidence (e.g., >85%):** The draft response is auto-filled into the agent's screen for quick 1-click approval.
* **Low Confidence (e.g., <85%):** The ticket is tagged for manual triage by an agent. This prevents AI hallucinations or miscategorizations from reaching customers.



---

### Project Summary Table

| Step | Tool / Library | Purpose | What Happens |
| --- | --- | --- | --- |
| **Data Handling** | `pandas` | Read & format incoming ticket data | Imports ticket text from CSV files into structured dataframes. |
| **Categorization** | `anthropic` (Claude API) | Classify issue types | Reads customer complaints and assigns tags (`auth`, `billing`, etc.). |
| **Auto-Drafting** | `anthropic` (Claude API) | Write response suggestions | Creates initial reply drafts for support agents to review. |
| **Validation** | Python logic / `scikit-learn` | Measure model precision | Compares AI output vs. human tags to calculate overall accuracy. |
| **Confidence Routing** | Python `if/else` logic | Safety net (Human-in-the-Loop) | Routes ambiguous or complex tickets directly to human agents. |

