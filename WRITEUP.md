
Flow in one paragraph

Admin sets criteria per category and each change is saved as a new version. The reviewer uploads proposals and picks a category. Code checks the files, extracts the text and locks the current criteria version for this run. A scan looks for instruction-like text inside the proposals. AI extracts facts as JSON with a source quote for each fact. Code checks the JSON and the quotes, runs the mandatory pass/fail checks, calculates scores and ranks. AI scores only team and approach and writes a one-line reason. A human reviews flagged items and the final list, then decides the award.

1. AI vs rules

AI is used for:
- Reading messy proposal text and extracting facts into a fixed JSON format.
- Scoring team and approach against a rubric, because it needs judgement.
- Writing the one-line reason in plain language.

Plain code is used for:
- File checks, text extraction and loading the criteria.
- Mandatory pass/fail checks (licence, WSIB, insurance) and the budget and 16-week limits.
- Scores for experience, price, timeline and warranty, weights, ranking and tie-breaks.

I drew the line here because AI is good at reading language but bad at being exactly repeatable. Anything that is a number or a yes/no rule should be code, so it can be tested and trusted.

2. Consistency and explainability

- Prices, weeks and years go through fixed formulas, so the same input always gives the same score.
- AI runs at temperature 0 with a fixed prompt version and strict JSON output. The only AI-made score is team and approach, using a 4-item rubric worth 25 points each.
- If the same proposals are ranked twice, the result should be the same. The extracted facts are saved after the first run and reused, so a re-rank does not call the AI again unless the reviewer asks for a new run.
- The reviewer sees a table for each proposal: every criterion, the extracted fact, the quote from the proposal, the points and the weight. Nothing is a black box.

3. Human in the loop

- Checkpoint 1: the reviewer looks at every flagged and ineligible proposal and can confirm or override. A reason is required for any override.
- Checkpoint 2: the reviewer reads the ranked list with the reasons and evidence quotes, can change it, and makes the final award. The tool never awards anything itself.
- The audit log saves who ran it, the time, criteria version, prompt version, model name, file hashes, extracted JSON, scores, flags, and every override with its reason.

4. Changing criteria

Criteria are versioned. When the admin edits them, a new version is created and old scores stay linked to the old version, so history is not rewritten. Old results are marked as scored under version N. The reviewer can re-run under the new version, which creates a new run. Both runs are kept and can be compared. Because the facts are already extracted, a re-score with new weights is quick and costs almost nothing, since only the code step runs again. If a mandatory criterion changes or a new one is added, the extraction is run again for the new fields.

5. Risks and safeguards

Wrong extraction: every fact needs a source quote, and code checks that the quote exists in the text. Bad output is retried once, then goes to a human.

Made-up facts: "not stated" is the required value for missing info, and the prompt forbids guessing.

Prompt injection (a proposal tries to tell the AI how to score it): proposal text is treated as data only. A scan flags instruction-like text, the AI is told to ignore it, and the proposal is flagged for a human. Scores come from code, so the AI cannot hand out full marks.

Missing or vague info (like no timeline): flagged, not guessed. The reviewer decides.

Claims that are not proof (like a licensed subcontractor to be confirmed later): the mandatory check needs the licence to be held by the bidder and stated. Otherwise the proposal is ineligible.

Bias: company names are not used for scoring, only the admin criteria. Reviewers can see the reason for every score.

Scanned or broken PDFs: there is an OCR step and a warning if the text quality is low.

Cost: facts are extracted once per proposal and cached, and only one small AI call is used for scoring. Re-scoring is code only.

Reviewer trusts the tool too much: the tool shows evidence and flags, and a human must confirm before the result is final.

How the sample set comes out

- B is ineligible. The licence is only promised through a subcontractor later, so it fails a mandatory check, even though it is the cheapest and fastest.
- C has hidden text trying to force first place. It is ignored, flagged and not rewarded. The real content is scored normally.
- D has no timeline, so it is flagged and the reviewer must ask for one.
- A and E are clean and complete. A ranks first on experience and speed.
