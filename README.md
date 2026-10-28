
Name: Muhammad Subtain Haqani Awaisi
Reference: MATACS-TH-2026-02
Role: Associate Software Developer
Time actually spent: about 2 hours

Files in this repository

WRITEUP.md - answers to the five questions
PROMPTS.md - the extraction and scoring prompts
ranking.json - my expected ranking of proposals A to E
diagram/flowchart.png - process flow from upload to ranked output

Short summary of the design

AI only reads the proposals, pulls out facts with a quote for each fact, and scores the one fuzzy criterion, team and approach. Everything else is plain code: pass/fail checks, the maths for price, timeline, warranty and experience, weights and ranking. Proposal text is treated as data, never as instructions. A person reviews every flagged and ineligible proposal and makes the final award.

How I used AI tools

I used Claude to help me understand the brief, draft the flow and tidy my writing. I made the final design decisions myself.

Where I disagreed with the AI: the first draft let the AI give all the scores, including price and timeline. I changed this because the AI can give different numbers on different runs, and price and timeline are just maths. I moved them to fixed formulas in code so the ranking is repeatable.

Assumptions

- Experience counts only installs of 100 kW or more, using the number the bidder states. I did not check dates beyond what is written.
- A bidder who only promises a licensed subcontractor later does not meet the licence requirement.
- A missing timeline is flagged for a human, not guessed.
- Scores for flagged proposals are provisional and shown only to help the reviewer.
