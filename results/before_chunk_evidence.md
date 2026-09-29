# Supplemental before evidence

Measured on 2026-09-28 with the unchanged corpus, chunker, model, top-k and cutoff. Used CPU execution for this inspection because the local accelerator could not initialize. No answer-model calls. Retrieval: `store.py::search`; chunk production: `chunker.py::split_documents`. This supplements the September 23 generation log, rather than replacing it.

## How many times can I change my meal plan tier?

Answer-bearing retrieved chunk: admin_meal_plan_changes.txt#0; distance 0.2475
```
On the meal plan changes

You can change your meal plan tier once, in the first ten days of the semester. After that it's locked. Downgrading refunds the difference to your student account; upgrading bills you immediately.
```

## When is the withdrawl dealine?

Answer-bearing retrieved chunk: admin_withdrawal_deadline.txt#0; distance 0.4679
```
On the withdrawal deadline

Withdrawal is a different thing from dropping and has a different date. Dropping ends at week six. Withdrawal runs to week ten, requires an adviser signature, and puts a W on the transcript that doesn't affect GPA.
```

## How many hours do I have to commit to CS 340 Databases class?

Answer-bearing retrieved chunk: course_cs_340.txt#1; distance 0.2835
```
CS 340 Databases

Lightly curved, usually two or three points. Expect 6 hours a week early, 15 in the last three weeks when the project lands. The one piece of advice: start the term project in week three, not week eight; everyone learns this the hard way.
```

Answer-bearing retrieved chunk: course_cs_340_workload.txt#0; distance 0.3125
```
Workload for CS 340 Databases

People keep asking so: 6 hours a week early, 15 in the last three weeks when the project lands. That's real time, not optimistic time. It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.
```

## How much money do I get to print per semester.

Answer-bearing retrieved chunk: admin_printing_quota.txt#0; distance 0.2876
```
On the printing quota

Every student gets $30 of printing per semester, which is roughly 600 black-and-white pages. It does not roll over. Colour costs eight times as much per page, which people discover after printing one poster.
```

## How many hours can I work a week with campus work 

Answer-bearing retrieved chunk: money_jobs.txt#1; distance 0.2302
```
On-campus work

Dining: no. Maximum is 20 hours a week during term. Most people find 10 to 12 is the point where it stops affecting coursework.
```

## Chunk sentence check

Checked 141 chunks: 141/141 have at most three body sentences; maximum 3. Titles are headings, excluded from body sentence counts. Sentence boundaries are ending punctuation followed by whitespace, not decimal points in prices. This verifies the numerical limit, not whether every detail is necessary.

### Actual sample from chunker.py::split_documents
```
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```
