# n8n Automation — Airtable AI Agent Workflow

## 📌 Project Overview

This project is a fully automated **n8n workflow** that collects team member data through a web form, stores it in **Airtable**, applies intelligent **profession-based rating logic**, and then uses **Google Gemini AI** to generate a personalized two-line poem for each team member — all without writing a single line of backend code.

It demonstrates end-to-end **no-code/low-code automation** using real-world integrations: form handling, database management, conditional branching, and AI content generation.

---

## 🚀 Why This Project Matters (Interview Perspective)

### 1. Real-World Automation Skills
This workflow solves a genuine business problem: **automating repetitive data entry and personalization tasks**. Instead of a human manually logging team members and generating personalized content, the entire pipeline runs automatically the moment a form is submitted.

### 2. Integration Expertise
It showcases the ability to connect multiple platforms — a web form, a cloud database (Airtable), and a Large Language Model (Google Gemini) — within a single cohesive pipeline. This is a core skill for roles in **automation engineering, data engineering, and AI product development**.

### 3. Conditional Logic & Decision-Making
The use of a **Switch node** to route records through different rating paths based on profession demonstrates understanding of **conditional branching and business rule implementation** — a fundamental concept in software engineering and workflow design.

### 4. AI Agent Integration
Embedding an **AI Agent** (powered by Google Gemini) directly inside an automation pipeline highlights practical knowledge of **LLM integration**, prompt engineering, and AI-augmented workflows — highly sought-after skills in the current market.

### 5. Data Lifecycle Management
The workflow covers the **complete data lifecycle**: data collection → storage → enrichment → AI processing → final update. This mirrors how production-grade ETL (Extract, Transform, Load) pipelines are structured.

---

## 🛠️ Technology Stack

| Tool / Platform       | Role in Workflow                              |
|-----------------------|-----------------------------------------------|
| **n8n**               | Workflow orchestration engine                 |
| **n8n Form Trigger**  | Web form for data collection                  |
| **Airtable**          | Cloud database for storing team records       |
| **Switch Node**       | Conditional routing based on profession       |
| **Google Gemini AI**  | Large Language Model for poem generation      |
| **n8n AI Agent Node** | Orchestrates AI prompt and response handling  |

---

## 🔄 Workflow — Step-by-Step Walkthrough

### Step 1 — Form Submission Trigger
**Node:** `On form submission` (Form Trigger)

The workflow begins when a team member fills in a web form titled **"My Team"**. The form captures:
- **Name** — the person's full name
- **Looks** — a brief description of their appearance
- **Profession** — selected from a dropdown: `Video Editor`, `Graphic Designer`, or `Manager`

> **Why it matters:** Using a form trigger makes the workflow self-service. No developer intervention is needed for each new entry. The trigger pattern is the foundation of every event-driven automation system.

---

### Step 2 — Create Record in Airtable
**Node:** `Update in Airtable` (Airtable — Create operation)

As soon as the form is submitted, n8n immediately creates a new record in an Airtable base (`Table 1`) with:
- `Name`, `Looks`, `Profession` — mapped directly from form fields
- `Rating` — set to an initial default value of **2**

> **Why it matters:** This step demonstrates **data persistence** — capturing and storing structured data in a cloud database. The initial rating of 2 acts as a placeholder that will be enriched by the next steps.

---

### Step 3 — Conditional Routing by Profession
**Node:** `Pathways` (Switch Node)

After the record is created, the workflow evaluates the `Profession` field and routes the execution to one of three parallel paths:

| Condition              | Route              |
|------------------------|--------------------|
| Profession = `Video Editor`     | → Video Editor Rating |
| Profession = `Graphic Designer` | → Graphic Designer Rating |
| Profession = `Manager`          | → Manager Rating |

> **Why it matters:** This is the **decision-making core** of the workflow. It mirrors `if/else` or `switch/case` logic in traditional programming and shows the ability to implement business rules declaratively.

---

### Step 4 — Profession-Based Rating Update
**Nodes:** `Video Editor Rating`, `Graphic Designer Rating`, `Manager Rating` (Airtable — Update operation)

Each profession path updates the Airtable record's `Rating` field with a role-specific score:

| Profession       | Assigned Rating |
|------------------|-----------------|
| Video Editor     | ⭐⭐⭐⭐⭐ **5** |
| Manager          | ⭐⭐⭐⭐ **4**   |
| Graphic Designer | ⭐⭐⭐ **3**     |

The correct record is identified using the Airtable record `id` captured from Step 2, ensuring the update targets the exact row just created.

> **Why it matters:** Demonstrates **dynamic data enrichment** — the ability to update records with derived or calculated values based on business logic. This mirrors how real-world systems apply scoring, classification, or tagging to incoming data.

---

### Step 5 — AI-Powered Poem Generation
**Node:** `AI Agent` (n8n LangChain AI Agent + Google Gemini Chat Model)

Once the rating is applied, all three profession paths converge at the **AI Agent** node. The agent sends a carefully crafted prompt to **Google Gemini**, instructing it to:

- Act as the best poem creator
- Write a **two-line poem only** (simple and easy to understand)
- Incorporate the person's **Name**, **Looks**, and **Profession**

Example prompt structure:
```
I want you to act like the best poem creator and create a two line poem based on the details below.
The poem should be only two lines and not more than that.
Also the poem should be extremely simple to understand.

I want you to include these things in the poem:
Name: [Name]
Looks: [Looks]
Profession: [Profession]
```

> **Why it matters:** This step showcases **prompt engineering** and **AI agent orchestration**. Embedding an LLM inside an automation pipeline to produce personalized, dynamic content is a cutting-edge capability that demonstrates practical AI integration skills.

---

### Step 6 — Save AI Output Back to Airtable
**Node:** `Update record` (Airtable — Update operation)

The AI-generated poem is written back to the Airtable record in a dedicated `Poem` field, completing the data lifecycle. The correct record is again targeted using its unique `id`.

> **Why it matters:** Closing the loop by persisting AI-generated output into the database turns the workflow into a **complete, auditable pipeline** — every team member's record contains their data, a rating, and a unique AI-authored poem.

---

## 🗺️ Full Workflow Diagram

```
[Form Submission]
       ↓
[Create Record in Airtable]  ← initial Rating = 2
       ↓
[Switch: Pathways by Profession]
   ↓           ↓           ↓
[Video Editor] [Graphic Designer] [Manager]
Rating = 5     Rating = 3         Rating = 4
   ↓           ↓           ↓
        [AI Agent — Google Gemini]
        Generates personalized 2-line poem
               ↓
        [Update Airtable Record]
        Saves poem to 'Poem' field
```

---

## 🎯 Key Concepts Demonstrated

| Concept                        | Where It Appears                              |
|-------------------------------|-----------------------------------------------|
| Event-driven triggers          | Form Trigger node                             |
| CRUD operations on a database  | Airtable Create & Update nodes               |
| Conditional branching          | Switch (Pathways) node                       |
| Data mapping & transformation  | Column mappings across all Airtable nodes    |
| Prompt engineering             | AI Agent node prompt design                  |
| LLM API integration            | Google Gemini Chat Model node                |
| End-to-end data lifecycle      | Form → Airtable → Rating → AI → Airtable     |
| No-code automation design      | Entire workflow built in n8n                 |

---

## 💼 Use Cases & Business Value

- **HR & Onboarding:** Automatically log new team members, assign role-based scores, and create personalized welcome messages.
- **Creative Agencies:** Collect team profiles and generate fun, personalized content for internal newsletters or team pages.
- **Talent Management:** Maintain a dynamic database of team members with automated enrichment and AI-generated content.
- **Portfolio Demonstration:** Showcases end-to-end automation design, AI integration, and data engineering skills to potential employers.

---

## ⚙️ Setup & Configuration

### Prerequisites
- [n8n](https://n8n.io/) instance (cloud or self-hosted)
- [Airtable](https://airtable.com/) account with a base containing: `Name`, `Looks`, `Profession`, `Rating`, `Poem` fields
- [Google Gemini API](https://ai.google.dev/) key (via Google PaLM API credentials in n8n)

### Import the Workflow
1. Open your n8n instance.
2. Go to **Workflows** → **Import from file**.
3. Select `My workflow.json` from this repository.
4. Update the Airtable credentials with your own **Personal Access Token**.
5. Update the Google Gemini credentials with your own **API key**.
6. Activate the workflow.

---

## 🧠 Interview Talking Points

1. **"How does your workflow handle different data paths?"**
   > The Switch node evaluates the `Profession` field and routes execution to the appropriate rating node, just like a `switch/case` statement in code.

2. **"How do you ensure the correct record is updated after creation?"**
   > The Airtable record `id` returned by the Create operation is captured and referenced in all subsequent Update operations using n8n's expression syntax: `$('Update in Airtable').item.json.id`.

3. **"How is the AI integrated into the pipeline?"**
   > An n8n AI Agent node is connected to the Google Gemini Chat Model via a LangChain integration. The agent receives structured field values from upstream nodes and uses a prompt template to generate contextual output.

4. **"What would you improve in this workflow?"**
   > Potential improvements include: adding error handling nodes, sending the poem via email or Slack, adding input validation on the form, or extending profession types dynamically.

5. **"What does this project demonstrate about your skills?"**
   > It demonstrates the ability to design, build, and connect multi-step automated pipelines integrating databases, conditional logic, and AI — skills directly applicable to automation engineering, data engineering, and AI product development roles.

---

## 📂 Repository Structure

```
n8n-automation-airtable-agent/
├── My workflow.json   # n8n workflow export (importable directly into n8n)
└── README.md          # Project documentation
```

---

## 📄 License

This project is open-source and available for learning and portfolio purposes.