# AI Solutions Consultant Portfolio

Enterprise AI | Generative AI | RAG | AI Agents | Workflow Automation | Responsible AI

---

## About Me

Computer Science graduate focused on practical AI solutions: RAG assistants, AI agents and responsible AI workflow design.

I solve business problems with modern AI platforms and low-code orchestration tools, and I test my work. My RAG project includes a 3-round evaluation that shows where it fails, not just where it works.

---

## Core Skills

- Generative AI and Prompt Engineering
- Retrieval-Augmented Generation (RAG)
- AI Agents and Tool Use (n8n)
- LLM Evaluation and Test Design
- Flowise, n8n, WhatsApp Cloud API
- Google Gemini Embeddings and Vector Search
- Responsible AI and Human-in-the-loop Design
- Business Analysis, BRD and Solution Design
- Technical Documentation

---

# Portfolio Projects

| # | Project | Type | Status |
|---|---|---|---|
| 1 | Multi-Document RAG Assistant + 3-round evaluation | Working build | Demo, with published evaluation |
| 2 | AI Ticket Triage | Design case study | Design only |
| 3 | AI-assisted Examination Evaluation | Design case study | Design only |
| 4 | Amrit Water WhatsApp AI Sales & Order Agent | Working build | Proof of concept, demo rates |

## Project 1: Multi-Document RAG Assistant with 3-Round Evaluation

A no-code chatbot built in Flowise that answers questions from two Indian government AI policy documents (NITI Aayog *National Strategy for AI* and the *Responsible AI for All* white paper), or says it doesn't know.

- Stack: Flowise, Groq chat model, Google Gemini embeddings, in-memory vector store
- Evaluated on a 25-question set (single-document, cross-document, out-of-scope), graded for correctness and grounding
- Main finding: retrieval, not generation, was the failure point. The white paper was never retrieved in any round
- Out-of-scope questions were never answered from general knowledge
- The rounds are indicative, not a controlled comparison (see the evaluation summary)

Repository: [Project-1-Enterprise-RAG/](Project-1-Enterprise-RAG/)

---

## Project 2: AI Ticket Triage (design case study)

How AI can classify and route support tickets with business rules, confidence handling and human review.

Repository: [Project-2-AI-Ticket-Triage/](Project-2-AI-Ticket-Triage/)

---

## Project 3: AI-assisted Examination Evaluation (design case study)

A responsible AI-assisted evaluation workflow for large-scale examinations, combining OCR, semantic comparison and rubric-guided scoring with human approval.

Repository: [Project-3-AI-Evaluation-System/](Project-3-AI-Evaluation-System/)

---

## Project 4: Amrit Water WhatsApp AI Sales & Order Agent

A tool-using AI agent that answers price questions, takes and confirms wholesale water orders on WhatsApp, and escalates complaints, discounts and cancellations to the owner.

> **Proof of concept.** Demo rates and test data only. Not a live deployment.

### Business Problem

Customers rarely give all order details in one message, quotes depend on quantity slabs and delivery rules, and every order needs a record. Handling this by hand is slow and error-prone.

### Solution

An n8n AI agent (Google Gemini, per-customer memory) that chooses between six tools: price list, calculator, order lookup, save order, update order and owner alert. Prices come only from a rate table and totals only from a calculator. Orders are confirmed only after an explicit yes.

### Key Capabilities

- WhatsApp Cloud API integration
- English and Hinglish replies
- Collects order details one question at a time
- Updates drafts when the customer changes their mind
- Escalates complaints, discounts, out-of-area delivery and cancellations to the owner
- Never cancels a confirmed order itself
- Reply check that blocks malformed model output

### Testing

14 scenario tests run through real WhatsApp conversations. 7 defects were found and fixed, including wrong totals from model arithmetic, leaked model reasoning and invented delivery status.

### Architecture

Customer (WhatsApp) → Meta Cloud API → n8n webhook → AI Agent (tools + memory) → reply check → WhatsApp reply. Owner alerts go to a separate WhatsApp number.

**Core principle:** The LLM interprets customer intent; the workflow and tools control business execution.

Repository: [Project-4-Amrit-Water-WhatsApp-AI-Assistant/](Project-4-Amrit-Water-WhatsApp-AI-Assistant/)

## Architecture

See the [Architecture-Diagrams](Architecture-Diagrams/) folder.

---

## Resume

[View / Download Resume (PDF)](Resume/Salonee_Sisodiya_Resume.pdf)

---

## Demo

### Amrit Water: WhatsApp AI Agent

[Watch the Amrit Water WhatsApp AI demo](https://youtu.be/-EyeYV5iyWg)

### Multi-Document RAG Assistant

[Watch the RAG / Flowise demo](https://youtu.be/DfR6QR0GL3s)

---

## Tech Stack

Flowise · n8n · WhatsApp Cloud API · Google Gemini · Groq · n8n Data Tables · Vector Store · Prompt Engineering · RAG · GitHub · Markdown

---

## Connect

[LinkedIn](https://www.linkedin.com/in/salonee-s-201564399)

[GitHub](https://github.com/salonee-sisodiya/ai-solutions-consultant-portfolio)

saloneesisodiya.26@gmail.com
