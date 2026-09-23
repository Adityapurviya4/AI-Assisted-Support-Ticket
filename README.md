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






















<concurrent_tool_execution>
The **AI-Assisted Support Ticket Classifier** transforms raw customer inquiries into categorized, actionable data while maintaining a strict safety net for edge cases. By combining deterministic data processing with generative AI inference, the system drastically reduces manual triage time without sacrificing response quality.

---

## Technical Project Canvas

| Architecture Pillar | Implementation Details |
| --- | --- |
| **Data Orchestration** | **Pandas** handles data ingestion, batching, text cleaning, and the validation logic (comparing Ground Truth manual labels against AI Output). |
| **Inference Engine** | **Claude API** performs zero-shot classification, extracting the user's intent and generating contextual draft responses. |
| **Categorization Scope** | Four primary routing queues: `auth`, `billing`, `integration`, and `data`. |
| **Safety Mechanism** | **Human-in-the-Loop (HITL):** Low-confidence inferences are flagged for manual review, preventing AI hallucinations from reaching end users. |

---

## The Classification Pipeline

---

## System Architecture: How It Works

1. **Data Ingestion & Cleaning:** Powered by Pandas.
The pipeline begins by loading historical support tickets into a Pandas DataFrame. The text data is preprocessed to remove PII (Personally Identifiable Information), strip out automated email footers, and isolate the core user inquiry.


2. **Context Assembly & API Execution:** Powered by Claude API.
The cleaned text is formatted into a structured prompt and sent to the Claude API. The system prompt is engineered to output a strict JSON object rather than conversational text.

The AI evaluates the text against the four core categories and returns three fields:

* **Category:** The assigned issue type (`auth`, `billing`, `integration`, or `data`).
* **Confidence Score:** A generated metric (e.g., 0.92) indicating how certain the model is about its classification based on the provided context.
* **Draft Response:** A context-aware reply solving the issue or requesting the necessary follow-up information.


3. **The Validation Step:** Measuring Accuracy.
Before deploying the automation, the model is tested against a manually labeled dataset. A script compares the `AI_Category` column against the `Manual_Label` column in the DataFrame. If the AI misclassifies complex edge cases (e.g., a ticket mentioning both a "billing error" and a "data sync issue"), the system logs the discrepancy. This generates hard metrics (Accuracy, Precision, Recall) to prove the model's reliability before production.


4. **Routing & Quality Control:** The Human-in-the-Loop.
Once deployed, the pipeline relies on the generated confidence score to route the ticket.

* **High Confidence:** The ticket is automatically tagged in the database and the draft response is queued.
* **Low Confidence:** The ticket is routed to a human agent's dashboard. The agent sees the AI's best guess and draft, but must manually approve, edit, or rewrite it before sending. This drastically cuts down typing time while ensuring quality control for ambiguous or frustrated user emails.


/
/
//

//
/
//
/
/
/
/

/
/
/
/
/
/
/
/
/
/

/

/
/
---

## Technical Whiteboard Architecture Canvas

The system architecture transforms raw, unstructured customer emails into structured database entities, confidence-scored predictions, and agent-ready response drafts through a deterministic, four-stage pipeline:

```
[ Inbound Support Ticket (CSV/Email) ]
                 │
                 ▼
 ┌───────────────────────────────────────────────┐
 │ Node 1: Pandas Data Ingestion & Preprocessing │
 │  - Remove PII & Email Footers                 │
 │  - Construct Structured DataFrame             │
 └───────────────────────┬───────────────────────┘
                         │
                         ▼
 ┌───────────────────────────────────────────────┐
 │ Node 2: Claude API Zero-Shot Inference        │
 │  - Prompt Engineering (System Prompt)         │
 │  - Enforce Structured JSON Schema Output      │
 └───────────────────────┬───────────────────────┘
                         │
        ┌────────────────┴────────────────┐
        ▼                                 ▼
 ┌───────────────┐               ┌─────────────────┐
 │ Validation    │               │ Confidence      │
 │ Matrix        │               │ Router          │
 └───────┬───────┘               └────────┬────────┘
         │                                │
         ▼                                ▼
[ Precision & Recall ]         ┌──────────────────┐
[ Accuracy Checking  ]         │ Score Check      │
                               └┬────────────────┬┘
                       >= 85%   │                │  < 85%
                                ▼                ▼
                      ┌──────────────────┐  ┌──────────────────┐
                      │ Agent Auto-Draft │  │ Human Triage Queue│
                      │ 1-Click Send     │  │ Manual Review    │
                      └──────────────────┘  └──────────────────┘

```

---

---

## Detailed System Design Canvas Specifications

### Node 1: Data Ingestion & Preprocessing Canvas (`pandas`)

This node normalizes unstructured support requests into structured DataFrames ready for API batch processing.

* **Ingestion Schema:** Reads raw CSV/Excel data into a Pandas DataFrame with core schema: `Ticket_ID`, `Timestamp`, `Customer_Email`, `Customer_Message`, and `Ground_Truth_Category`.
* **Text Preprocessing:**
* Strips out regex signatures (e.g., `"Sent from my iPhone"`, email headers, legal disclaimers).
* Masks Personally Identifiable Information (PII) like credit card numbers, phone numbers, and IP addresses.


* **Batching Strategy:** Groups clean messages into configurable batches (e.g., 50 tickets per API call loop) to manage throughput and API rate limits.

---

### Node 2: Inference & JSON Schema Canvas (`anthropic` API)

This node passes the cleaned text to Claude with a system prompt enforcing a strict JSON return payload.

* **Prompt Construction:** Uses zero-shot classification guidelines specifying four immutable target categories (`Auth`, `Billing`, `Integration`, `Data`).
* **JSON Response Constraint:** Forces the model to return raw JSON matching this schema:

```json
{
  "predicted_category": "Auth",
  "confidence_score": 0.94,
  "reasoning_brief": "User mentions password reset loop and 403 authorization error.",
  "draft_response": "Hi there,\n\nIt looks like you're experiencing a login loop. Please clear your browser cache or reset your password using the account recovery link...\n\nBest regards,\nSupport Team"
}

```

---

### Node 3: Validation & Accuracy Checking Canvas

Before deploying to live support workflows, the model's accuracy is calculated against historical manually labeled tickets (`Ground_Truth_Category`).

| Metric | Formula / Implementation | Goal Threshold | Purpose |
| --- | --- | --- | --- |
| **Accuracy** | $\frac{\text{Correct AI Predictions}}{\text{Total Validated Tickets}}$ | $\ge 90\%$ | Verifies overall system reliability across all ticket types. |
| **Category Precision** | $\frac{\text{True Auth Positives}}{\text{True Auth Positives} + \text{False Auth Positives}}$ | $\ge 88\%$ | Ensures miscategorized tickets don't leak into the wrong support queues. |
| **Recall** | $\frac{\text{True Auth Positives}}{\text{True Auth Positives} + \text{False Auth Negatives}}$ | $\ge 92\%$ | Guarantees critical customer issues aren't missed or dropped. |

---

### Node 4: Confidence-Based Routing Canvas (Human-in-the-Loop)

A safety gate acts as a fallback to ensure uncertain AI predictions never harm customer relations.

* **High Confidence Path ($\text{Score} \ge 85\%$):**
* Ticket automatically tags as `Auth` / `Billing` / `Integration` / `Data`.
* Draft response auto-populates directly inside the support agent's dashboard editor.
* Agent performs a 1-click review and send, reducing ticket handle time by up to 70%.


* **Low Confidence Path ($\text{Score} < 85\%$):**
* Ticket routes directly to the **Manual Triage Queue**.
* AI prediction and draft are hidden or marked with a warning flag.
* Human agent categorizes and writes/edits response manually.
* The manual correction is logged back into the training/validation set for continuous model optimization.
