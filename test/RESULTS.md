# Test results

## Setup

| | |
|---|---|
| Date | 11 October 2026 |
| Model | qwen3.6:35b, running in Ollama on a Mac |
| App build | 2026-10-11.4 |
| Context window | 16384 tokens |
| Chunk size / overlap | 6000 / 500 characters (3 chunks) |
| Thinking | Model default (on) |
| Document | `Harrowmere_ITT_test.pdf` |
| Answer key | `Harrowmere_answer_key.md` |
| Report produced | `RLM_Report_qwen3.6-35b.pdf` |

The document is a fictional six page invitation to tender, about 2,400 words, written for this test. It contains:

- 30 mandatory requirements, each with a specific date, amount, name or list
- 6 blank fields on a Form of Tender
- 7 statements that look like requirements and are not (optional, background or explicitly not mandatory)
- 1 amendment: a 30 page limit on page 3 is raised to 35 by a clarification note on page 6

Objective given to the app:

> List every mandatory requirement a tenderer must meet to submit a compliant tender and to deliver the contract. Include every deadline, amount, percentage, accreditation, named site, and every field the tenderer has to fill in.

The Objective Refiner suggested a rewrite. It was skipped and the objective above was used as written.

## Score

| Measure | Result |
|---|---|
| Requirements reported with the right figure | 30 of 30 |
| Form fields reported | 6 of 6 |
| Amendment (30 to 35 pages) | Both figures reported, with the change noted |
| Non-requirements reported as mandatory | 0 of 7 |
| Facts extracted | 41 |
| Quotes found in the document | 41 of 41 |
| Chunks cut off or lost | 0 of 3 |
| Warnings | None |

## What the run shows

- The audit step did real work. The first draft left out the performance bond (10% of annual contract value within 14 days). The audit caught it and the merge put it back in the right section. Without that step the score would have been 29 of 30.
- Both long lists came through complete: seven accreditations and fourteen named sites.
- The one statement about insurance that is easy to misread was handled correctly. The report says cover is not needed at submission but must be confirmed in the Form of Tender.
- The model rated its confidence HIGH, which the score supports.

## Context window and thinking

The same document and objective were first run with a context window of 8192. qwen3.6 thinks before it answers, and thinking counts against the output limit. In two of the three chunks it used more than 5,000 of about 5,800 available output tokens on thinking, and the answers were cut off part way through. That run scored 14 of 30. Every missed requirement was in one of the two chunks that were cut off, and the app reported both in its warnings.

Raising the context window to 16384 was the only change needed to get from 14 to 30.

## Flaws in the 30 of 30 run

- One fact appears twice (F30 and F32, the cleaning audit score). Two chunks overlapped and the model worded it slightly differently each time, so the duplicate check did not catch it.
- The amendment is reported as two bullets, 30 pages and then "increased to 35", where one statement of the current limit would be clearer.
- Two statements are firmer than the source. The report says abnormally low tenders "will be rejected" where the document says "may be rejected", and "must complete every cell" where the document says "should". Both quotes are marked verified, because the check confirms that the quote is in the document. It does not confirm that the statement matches the strength of the quote.
- The PDF export shows only the first two lines of a long objective.

## Limits of this test

This is one model and one short document. The document was written for the test, so its requirements are phrased cleanly and are easy to quote. Real tenders are longer and messier.

Not yet tested:

- The previous version of the app on the same document, for a before and after comparison
- Thinking turned off, which should be several times faster
- A document long enough to need more than one consolidation batch, which is where the report is drafted in parts
- Other models
