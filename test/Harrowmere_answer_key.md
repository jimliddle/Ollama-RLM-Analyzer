# Answer key: Harrowmere ITT test document

Fictional 6-page tender (about 2,400 words, 3 chunks at the default 6,000-character chunk size).

## Main question

> List every mandatory requirement a tenderer must meet to submit a compliant tender and to deliver the contract. Include every deadline, amount, percentage, accreditation, named site, and every field the tenderer has to fill in.

## Second question (narrow, tests precision)

> What insurance and financial security must the tenderer provide, and by when?

Expected: R16, R17, R18, R19 only (bid bond, performance bond, three insurance levels, parent company guarantee). Most of the document is irrelevant to this question, so most chunks should return few or no facts. The "bank issuing the bid bond" form field on page 6 (R34) is a fair extra.

## Requirements (30)

Score one point for each that appears in the final report with the right figure.

| ID | Page | A correct answer says | Verbatim text to look for |
|---|---|---|---|
| R1 | 1 | All correspondence must quote the tender reference HBC/FM/2026/014 in the subject line | must quote the tender reference HBC/FM/2026/014 |
| R2 | 1 | Clarification questions must be submitted via the portal by 17:00 on 6 November 2026 | by 17:00 on 6 November 2026 |
| R3 | 1 | Attendance at the site visit on 3 November 2026 at 10:00 (Civic Centre reception) is mandatory | Attendance at the site visit is mandatory |
| R4 | 2 | No more than three representatives at the site visit; names registered by 30 October 2026 | must register their names by 30 October 2026 |
| R5 | 2 | Tenders must be received through the portal by 12:00 noon on 27 November 2026 | no later than 12:00 noon on 27 November 2026 |
| R6 | 2 | Tenders must remain valid/open for acceptance for 120 days from the deadline | open for acceptance for 120 days |
| R7 | 2 | Annual turnover of at least £2,400,000 in each of the last three financial years (audited accounts) | annual turnover of at least £2,400,000 |
| R8 | 2 | Must hold ALL SEVEN accreditations: ISO 9001, ISO 14001, ISO 45001, SafeContractor, Gas Safe, NICEIC Approved Contractor, BICSc corporate membership | ISO 9001, ISO 14001, ISO 45001, SafeContractor, Gas Safe registration, NICEIC Approved Contractor status and BICSc corporate membership |
| R9 | 2 | No more than two subcontractors, each named in the tender | No more than two subcontractors |
| R10 | 2 | Three references from similar contracts in the last five years, at least one public sector | three references from contracts of a similar size |
| R11 | 3 | Single PDF file not exceeding 40 MB | single PDF file that does not exceed 40 MB |
| R12 | 3 | Method statement page limit: 30 pages in section 4.3, RAISED TO 35 by Clarification Note 2 (correct answer is 35, or 30 with the amendment noted) | The method statement shall not exceed 30 pages |
| R13 | 3 | Arial, minimum 11 point, written in English | Arial at no less than 11 point |
| R14 | 3 | Prices in pounds sterling, excluding VAT | pounds sterling and shall exclude VAT |
| R15 | 3 | Prices fixed/firm for the first 24 months | held firm for the first 24 months |
| R16 | 3 | Bid bond of £25,000 from a PRA-authorised bank, submitted with the tender | bid bond of £25,000 |
| R17 | 3 | Performance bond of 10% of annual contract value within 14 days of the award letter | performance bond equal to 10% of the annual contract value |
| R18 | 3 | Insurance: public liability £10,000,000; employer's liability £5,000,000; professional indemnity £2,000,000 | public liability insurance of not less than £10,000,000 |
| R19 | 3 | Parent company guarantee required where the tenderer is a subsidiary | parent company guarantee |
| R20 | 4 | Enhanced DBS certificate (issued within the previous three years) for staff in libraries, leisure centres and community buildings | enhanced DBS certificate |
| R21 | 4 | Staff paid at least the Real Living Wage from day one | at least the Real Living Wage |
| R22 | 4 | Full-time contract manager with at least five years of comparable experience | minimum of five years of experience |
| R23 | 4 | At least 4 apprenticeships during the initial term, each at least twelve months | At least 4 apprenticeships |
| R24 | 4 | Response times: emergency 2 hours, urgent 24 hours, routine 5 working days | within 2 hours of the call being logged |
| R25 | 4 | Helpdesk staffed 24 hours a day, every day of the year | helpdesk that answers calls 24 hours a day |
| R26 | 4 | Monthly performance report by the fifth working day of each month | by the fifth working day of each month |
| R27 | 4 | Cleaning audit score of at least 98% at every site every month | score of at least 98% |
| R28 | 4 | Cleaning chemicals must carry the EU Ecolabel or equivalent | EU Ecolabel |
| R29 | 5 | Minimum quality threshold: tenders scoring under 50% of quality marks are excluded | less than 50% of the available quality marks |
| R30 | 6 | Fourteen sites, all named: Harrowmere Civic Centre, Eastgate Library, Quarry Lane Depot, Millbrook Leisure Centre, St Aldhelm's Community Hall, Northfield Library, Old Corn Exchange, Wetherby Road Depot, Castle Park Pavilion, Fenwick House, Registry Office, Lowmoor Leisure Centre, Bridge Street Customer Hub, Ashdown Cemetery Lodge | Ashdown Cemetery Lodge |

## Form fields (6)

All on page 6, Annex B. The extraction prompt tells the model to treat blank fields as requirements.

| ID | Field |
|---|---|
| R31 | Name of tenderer: |
| R32 | Company registration number: |
| R33 | Registered office address: |
| R34 | The bank issuing the bid bond shall be: |
| R35 | Total annual price in pounds sterling, excluding VAT: |
| R36 | Signed by an authorised signatory: |

## Traps

**The amendment.** Section 4.3 on page 3 sets a 30-page limit. Clarification Note 2 on page 6 raises it to 35. The two statements sit in different chunks, so only the consolidation step can reconcile them.

- Best: the report says 35 pages and mentions the change.
- Acceptable: it reports both figures and the amendment.
- Wrong: it says 30 pages with no mention of the change, or lists 30 and 35 as two separate requirements.

**Long lists.** R8 has seven accreditations and R30 has fourteen sites. Count them in the final report; a shortened list is a miss.

**Things that are NOT mandatory.** Each of these appearing as a requirement is a false positive.

| Page | Statement | Why it is not a requirement |
|---|---|---|
| 1 | The incumbents employ 46 staff; spend was about £1,850,000 | Background, "for information only" |
| 2 | Online briefing on 22 October 2026 | Optional, "encouraged to join" |
| 3 | One variant tender | Permitted, not required |
| 3 | Insurance cover at the time of submission | Explicitly not needed until the start date |
| 4 | Recruiting within the borough, working with local colleges | "Would welcome", explicitly not a requirement |
| 4 | Electric vehicles | No date set; Clarification Note 3 confirms not mandatory |
| 5 | Weighting 60% quality / 40% price | A fact about evaluation, not something the tenderer must do |

## What to record for each run

1. **Recall:** requirements found out of 30, form fields out of 6.
2. **False positives:** how many of the seven traps were reported as mandatory.
3. **Amendment:** best, acceptable or wrong.
4. **Badges:** every text in the "verbatim" column is in the PDF exactly, so a fact about one of them marked UNMATCHED means the model reworded its quote. An INFERRED badge on a trap item is the label working as intended.
5. **Pages:** spot-check five facts against the Page column.
6. **Warnings:** note any truncation or cut-off warnings in the final result.
