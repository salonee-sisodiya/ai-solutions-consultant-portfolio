# RAG Evaluation Summary

Evaluation of the Flowise multi-document RAG chatbot (NITI Aayog *National Strategy for AI* + *Responsible AI for All* white paper) across three rounds.

Raw data: [Round 1](evaluation/rag-eval-round1.csv) · [Round 2](evaluation/rag-eval-round2.csv) · [Round 3](evaluation/rag-eval-round3.csv) · [Summary table](evaluation/rag-eval-summary.csv)

## Method

- **Question set:** 25 questions: 18 single-document (Q1-10 strategy, Q11-18 white paper), 4 cross-document (Q19-22), 3 out-of-scope controls (Q23-25).
- **Ground truth:** each question has an expected answer and source page numbers taken from the PDFs.
- **Grading:** every answer is marked **correct**, **partial** or **wrong**, plus a separate **Grounded Y/N** (did the bot state anything not supported by the documents?).
- **Fixed settings in all rounds:** chunk size 1500, overlap 300, Groq chat model, Gemini embeddings, in-memory vector store.

## Rounds

| Round | Settings | Questions run | Correct | Partial | Wrong | Partial or better | Grounded | Out-of-scope |
|---|---|---|---|---|---|---|---|---|
| R1 | v1, Top K 10 (some questions graded at Top K 4 or 6), task type RETRIEVAL_DOCUMENT | 25 | 4 | 2 | 19 | 24.0% | 80.0% | 3/3 |
| R2 | v2, Top K 4, task type RETRIEVAL_DOCUMENT | 11 | 3 | 2 | 6 | 45.5% | 90.9% | 2/2 |
| R3 | v3, Top K 4, task type SEMANTIC_SIMILARITY, contents page removed from index | 13 | 3 | 2 | 8 | 38.5% | 38.5% | 2/2 |

### Like-for-like (10 questions run in all rounds: Q1, 2, 10, 11, 14, 18, 19, 22, 23, 24)

| Round | Correct | Partial | Wrong | Partial or better | In-scope only (excl. 2 controls) |
|---|---|---|---|---|---|
| R1 | 3 | 0 | 7 | 3 of 10 | 1 of 8 |
| R2 | 3 | 2 | 5 | 5 of 10 | 3 of 8 |
| R3 | 3 | 0 | 7 | 3 of 10 | 1 of 8 |

### Limitations

- R2 and R3 are partial runs (11 and 13 of 25), so their overall percentages are not directly comparable to R1.
- R1 was not one fixed configuration: Q10-13 and Q16-17 were graded at Top K 4 and Q14-15 at Top K 6.
- R3 changed two things at once (embedding task type and contents page), so its effect cannot be pinned on either.
- One grader (me), one run per question, small sample. Treat the numbers as directional.

## Findings

### 1. Retrieval is the main failure point
For Q2-4 and Q6-10 the answer sits in a specific section (for example healthcare p.24-29, skilling p.64-70, ethics p.85-89, Appendix II p.100-102), but those pages were not retrieved. The bot received introduction or front-matter chunks instead and either said the details were missing or answered generically.

### 2. The white paper was never retrieved
All 8 white-paper questions (Q11-18) failed in R1. R2 and R3 did not retrieve it either (Q11, Q14, Q18). The strategy PDF is larger and dominates the top results. Cross-document questions (Q19-22) fail for the same reason, since only one document reaches the model.

### 3. Misattribution between documents
The bot repeatedly presented strategy content as the white paper's (R1 Q14, Q18; R2 Q14; R3 Q11, Q14, Q18). In R3 it then claimed the white paper has no discussion of equality or non-discrimination, or that agriculture and health are its case studies. Both are false. The white paper's case study is Digi Yatra airport boarding.

### 4. Removing the contents page changed the failure mode
| | R1 / R2 (contents page indexed) | R3 (contents page removed) |
|---|---|---|
| Typical behaviour | Found the section title, said the content was "not in the excerpts" | Answered from the introduction, or said the content does not exist |
| Example | Q7: "does not elaborate on the specific measures" | Q7: "does not outline a concrete skilling curriculum" (the strategy has a 7-page skilling chapter) |
| Grounded rate | 80.0% (R1), 90.9% (R2) | 38.5% |

R3 also returned the same four introduction chunks for most questions, including off-topic ones. This may point to the embedding task-type change rather than the contents page, which this test cannot separate.

### 5. Output formatting problems
| Issue | Where |
|---|---|
| Table answer truncated mid-sentence | R2 Q2, Q4, Q18, Q19; R3 Q5 |
| Raw `sourceDocuments` JSON returned instead of an answer | R2 Q22 |
| Answer was only "**Actions" | R3 Q22 |
| Stray `<br>` tags and `**` markdown | R1 Q6, Q9, Q13 |

### 6. Out-of-scope handling is reliable
The cricket, 2026 budget and biryani questions were never answered from general knowledge, and no figure or recipe was invented. Weaknesses: refusals are terse ("Hmm, I'm not sure.") and don't say the topic is outside the documents, and irrelevant source chips are still shown.

## Recommended next experiments

Change one variable per round and re-run all 25 questions each time.

1. Raise Top K (4 to 8).
2. Separate retriever per document, or document name added as chunk metadata.
3. System prompt: no tables, short bullets, name the source document per point, say explicitly when the topic is outside the documents.
4. Raise the max token limit.
5. Revert the embedding task type to RETRIEVAL_DOCUMENT and test the contents-page removal on its own.
