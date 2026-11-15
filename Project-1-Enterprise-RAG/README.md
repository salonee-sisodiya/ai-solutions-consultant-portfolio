# Project 1: Enterprise Multi-Document RAG Chatbot

A no-code RAG (Retrieval-Augmented Generation) chatbot built in **Flowise** that answers questions about two Indian government AI policy documents, followed by a **3-round evaluation** that measures where it works and where it fails.

![Workflow overview](screenshot/workflow%20overview.png)

## What it does

Ask a question in plain English and the bot answers from two source PDFs, or says it doesn't know.

| Document | Used for |
|---|---|
| NITI Aayog, *National Strategy for Artificial Intelligence* (2018) | Sector strategy, skilling, research, ethics, government actions |
| NITI Aayog, *Responsible AI for All* white paper | Responsible-AI principles, applied to facial recognition (Digi Yatra) |

## Tech stack

| Component | Choice |
|---|---|
| Platform | Flowise (no-code), Node.js 20 |
| Chain | Conversational Retrieval QA Chain |
| Chat model | Groq |
| Embeddings | Google Gemini embeddings |
| Vector store | In-memory (re-upsert the PDFs after every Flowise restart) |
| Loaders | Two PDF loaders, one per document |
| Chunking | 1500 characters, 300 overlap |
| Top K | 4 (R2, R3) |

## Repository contents

| Path | Description |
|---|---|
| `chatflow/Multi -Doc RAG Chatflow.json` | Exported Flowise chatflow. Import it to reproduce the bot |
| `chatflow/system-prompt.txt` | The system prompt (Response Prompt) used by the bot |
| `evaluation/` | Three evaluation rounds (CSV) and a summary table |
| `evaluation-summary.md` | Written analysis of the evaluation |
| `Flowise case study.pdf` | Two-page case study |
| `screenshot/` | Screenshots of the build (PDF uploader, vector store, workflow, sample query) |

## How to run it

1. Install Node.js 20, then run `npm install -g flowise` and `npx flowise start`.
2. Open Flowise in your browser and go to **Chatflows → Add New → Settings → Load Chatflow**, then select `chatflow/Multi -Doc RAG Chatflow.json`.
3. Add your own **Groq** and **Google Gemini** API credentials to the chat model and embeddings nodes.
4. Upload the two PDFs in the PDF loader nodes and click **Upsert**.
5. Open the chat window and ask a question.

## Evaluation

I tested the bot with a fixed set of **25 questions**: 18 single-document, 4 cross-document and 3 out-of-scope. Each question has an expected answer and source pages, and each bot answer was graded as correct, partial or wrong, plus a separate grounded Y/N check (did it invent anything?).

| Round | What changed | Questions run | Correct | Partial | Wrong | Partial or better | Grounded | Out-of-scope handled |
|---|---|---|---|---|---|---|---|---|
| R1 | Baseline | 25 | 4 | 2 | 19 | 24.0% | 80.0% | 3/3 |
| R2 | Top K 4 | 11 | 3 | 2 | 6 | 45.5% | 90.9% | 2/2 |
| R3 | Embedding task type changed, contents page removed from index | 13 | 3 | 2 | 8 | 38.5% | 38.5% | 2/2 |

> R2 and R3 were partial runs, and R1 was not a single fixed configuration, so the rounds are **indicative, not a controlled comparison**. Details are in the [evaluation summary](evaluation-summary.md).

### Key findings

1. **The main problem is retrieval, not generation.** For most failed questions the answer exists in the PDFs, but the right pages were never retrieved (healthcare, education, skilling, ethics, government actions, appendix countries).
2. **The white paper was never retrieved in any round.** All 8 white-paper questions failed in R1, and R2 and R3 never retrieved it either. The larger strategy document dominates the top results.
3. **The bot sometimes credits the wrong document.** In R3 it presented strategy content under a "white paper" heading and said the white paper has no content on equality, which is false.
4. **Removing the contents page made failures more confident, not better.** In R1 the bot said "the details aren't in the excerpts". In R3 it answered from the introduction or claimed the strategy has no skilling programmes. Grounded rate fell from 90.9% (R2) to 38.5% (R3), although the question sets differ.
5. **Out-of-scope handling works.** The bot never answered the cricket, budget or recipe questions from general knowledge. Its refusals are terse.
6. **Output formatting hurt several answers.** Table-style answers were cut off mid-sentence (R2: Q2, Q4, Q18, Q19) and two cross-document answers returned raw JSON or just "**Actions".

### What I would test next

These are hypotheses, not results:

- Raise Top K (4 to 8 or higher) and re-run the full 25 questions under one fixed configuration.
- Use a separate retriever per document, or add document names as metadata, so the white paper is not crowded out.
- Change the system prompt to ban tables, use short bullets, name the source document for each point, and say when something is outside the documents.
- Raise the max token limit to stop truncation.
- Change one setting at a time. R3 changed two (task type and contents page), so its effect cannot be attributed to either.

Full per-question results: [Round 1](evaluation/rag-eval-round1.csv) · [Round 2](evaluation/rag-eval-round2.csv) · [Round 3](evaluation/rag-eval-round3.csv) · [Summary table](evaluation/rag-eval-summary.csv)

## System prompt

The full Response Prompt used in the Conversational Retrieval QA Chain node is in [`chatflow/system-prompt.txt`](chatflow/system-prompt.txt). Flowise inserts the retrieved chunks where `{context}` appears.

## Author

**Saloni Sisodiya**, AI Solutions Consultant
[LinkedIn](https://linkedin.com/in/salonee-s-201564399) · saloneesisodiya.26@gmail.com
