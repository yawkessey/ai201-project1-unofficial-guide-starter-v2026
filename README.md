# The Unofficial Guide

<!-- Replace this line with your name and which corpus you picked. -->

Yaw Kessey-Ankomah — `campus_life`

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

I selected the `campus_life` corpus of short campus posts. This command-line guide answers questions covered by those posts, including courses, campus work hours, and meal-plan details. It retrieves relevant chunks and uses them to generate an answer with a source document. If no chunk is close enough to the question, the relevance gate refuses to answer.

## Chunking Strategy

**Chunk size:** Up to three complete body sentences, with the document title repeated for context.

**Overlap:** Zero repeated body sentences; the title is retained in each chunk.

Campus posts are short but can cover several topics, so I chose smaller groups of complete sentences to keep the retrieved material focused. The original 800-character windows kept all 88 posts whole; the new strategy produces 141 chunks. I start with no body-sentence overlap to avoid repeating the same facts, while retaining the title to identify the topic. This limits retrieved text, not the length of the final answer. The simple sentence splitter preserves decimal prices but may split abbreviations incorrectly, and sentence groups can still depend on context in another chunk.

<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `admin_add_drop_deadline.txt#0` — produced by: `chunker.py::split_documents`

```
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** — source: `course_cs_340.txt#0` — produced by: `chunker.py::split_documents`

```
CS 340 Databases

I'm a junior and I've done this twice now. Format is lecture twice a week plus a project that runs the whole term. Assessment: one midterm and a final, both open-book.
```

**Chunk 3** — source: `dining_halden_hall.txt#0` — produced by: `chunker.py::split_documents`

```
Halden Hall

I lived here my sophomore year. Wait times: rarely more than 8 minutes, even at noon. The thing worth going for is soup rotation, and the bread is baked on site.
```

**Chunk 4** — source: `health_center.txt#0` — produced by: `chunker.py::split_documents`

```
The health centre

Walk-in hours are 8am to 11am; everything after that is by appointment and appointments run about a week out. If something is urgent, go at 8am and wait rather than booking. Counselling is separate, in the same building, and has its own intake process with a shorter wait than people expect — usually three or four days for a first session.
```

**Chunk 5** — source: `housing_morrow_house_laundry.txt#0` — produced by: `chunker.py::split_documents`

```
Laundry in Morrow House

Machines take $1.50 wash, $1.25 dry, coin or card. There are eight washers and six dryers for the building, which is the wrong ratio and means the dryers back up on Sunday evenings. Best time to do laundry here is Tuesday or Wednesday morning.
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:** How many times can I change my meal plan tier?

**Answer:**

```
You can change your meal plan tier once.

Source: admin_meal_plan_changes.txt
```

**My relevance cutoff:** `0.6`

With the rebuilt sentence-based index, the five in-corpus best distances range from 0.2302 to 0.3938, while the five out-of-scope distances range from 0.8236 to 0.8859. The existing cutoff of 0.6 falls in the gap, so I kept it: all five in-corpus questions pass the gate and all five out-of-scope questions are refused. These measurements support the cutoff for these ten questions; they do not guarantee the same result for every possible question.

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question | In corpus? | Best distance |
|---|---|---|
| How many times can I change my meal plan tier? | Yes | 0.2475 |
| When is the withdrawl dealine? | Yes | 0.3938 |
| How many hours do I have to commit to CS 340 Databases class? | Yes | 0.2835 |
| How much money do I get to print per semester. | Yes | 0.2876 |
| How many hours can I work a week with campus work | Yes | 0.2302 |
| What is the capital of Mongolia? | No | 0.8236 |
| How do I change the oil in a diesel engine? | No | 0.8493 |
| Who won the 1994 World Cup? | No | 0.8859 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.8442 |
| How do I write a for loop in Rust? | No | 0.8714 |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.**
I asked Codex to help me write `split_documents()` using my three-sentence chunk limit, without editing the file for me. It suggested grouping up to three body sentences, repeating the document title for context, and using zero body-sentence overlap. I pasted the code myself, but my saved version had missing lines and indentation errors. With Codex's debugging feedback, I corrected the indentation, added the missing import, document loop, and chunk-list initialization, and returned the new chunks instead of calling the fallback. Codex then ran the preview successfully, producing 141 chunks instead of the starter's 88.

**2.**
I ran retrieval for my meal-plan and withdrawal questions, then asked Codex to run the remaining three campus questions and the five out-of-scope questions and record the distances in my README. The combined results showed in-corpus best distances of 0.2302–0.3938 and out-of-scope distances of 0.8236–0.8859. Codex recommended keeping the existing 0.6 cutoff because it fell between those groups and filled the blank table with the measured results. I left that recommendation unchanged. I then asked Codex to run the meal-plan question and add the actual answer and source to the Sample Answer section.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunks contain at most three body sentences | Maximum 3 sentences | Max 3 (141/141) | Max 3 (141/141) | Max 3 (141/141) | MET |
| 5. Every answer ends with a follow-up question | 5 of 5 | 0/5 | 0/5 | 0/5 | MISSED |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

Baseline generation evidence: [saved evaluation](results/run_2026-09-23_2039.md), produced by `run_eval.py::main` on September 23 with three runs per question and caching off. Criteria 2 and 5 are counted from the five in-corpus answers in each run. Every answer names a source; none ends with a follow-up question. The scorer's expected-phrase passes are not substitutes for these criterion checks.

[Supplemental chunk evidence](results/before_chunk_evidence.md) records a September 28 inspection using `store.py::search` and `chunker.py::split_documents`, with no system changes or new generated answers. The retrieved source sets and best distances match the saved log. All five questions retrieve a chunk containing the answer. Retrieval and chunking are deterministic, so these measurements are repeated across the three columns; they are not three new generation runs. Criterion 4 measures the numerical three-sentence limit across all 141 chunks, treating titles as headings rather than body sentences. It does not establish that every detail is necessary. No original criteria were changed.

**Criterion 1 — actual retrieved chunk:** `admin_withdrawal_deadline.txt#0`, returned by `store.py::search` at distance 0.4679 (the second result). The supplemental evidence includes answer-bearing chunks for all five questions.

```text
On the withdrawal deadline

Withdrawal is a different thing from dropping and has a different date. Dropping ends at week six. Withdrawal runs to week ten, requires an adviser signature, and puts a W on the transcript that doesn't affect GPA.
```

**Criteria 2 and 5 — actual answer, meal-plan question, run 1:** generated by `generate.py::answer_from_chunks` and recorded by `run_eval.py::main` in the saved evaluation. It names a source but contains no follow-up question.

```text
You can change your meal plan tier once.

Source: admin_meal_plan_changes.txt
```

**Criterion 3 — actual gate results:** produced by `run_eval.py::check_out_of_scope` in the saved evaluation, cutoff 0.6. One deterministic pass supplies the same 5/5 for each column.

| Out-of-scope question | Best distance | Gate |
|---|---|---|
| What is the capital of Mongolia? | 0.824 | refused |
| How do I change the oil in a diesel engine? | 0.849 | refused |
| Who won the 1994 World Cup? | 0.886 | refused |
| What is the recommended dosage of ibuprofen for a headache? | 0.844 | refused |
| How do I write a for loop in Rust? | 0.871 | refused |

**Criterion 4 — actual chunk:** `admin_add_drop_deadline.txt#0`, produced by `chunker.py::split_documents`. This has three body sentences; the full inspection found a maximum of three across 141 chunks.

```text
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunks contain the answer | MET | All five questions retrieved an answer-bearing chunk, this exceeds target of 4/5 |
| 2 | Every answer names a source | MET | All five answers named a source in each run|
| 3 | Gate stops out-of-corpus questions | MET | All 5 out-of-scope questions refused |
| 4 | Maximum three sentences per chunk | MET | All chunks contained at most three body sentences |
| 5 | Every response ends with a follow-up question | MISSED | None of the 5 answers contained the followup question in any of the runs  |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

### Criterion 5 — missing follow-up questions

**Stage: generation.** None of the five in-corpus answers ended with a follow-up question in any of the three runs (0/5 each run, or 0/15 answers overall). For example, the meal-plan answer gave the limit of one change and cited `admin_meal_plan_changes.txt`, then stopped.

**Mechanism:** In `generate.py`, `answer_from_chunks()` passes `GROUNDING_INSTRUCTION` to the model. That instruction asks for a brief answer based only on the documents and a source filename, but never asks for a follow-up question. `build_prompt()` also asks only for an answer and the file used. The follow-up requirement exists in `criteria.md`, but that file is not included in this answer-generation prompt. The system therefore does not communicate the required behavior to the model. This explains the missing instruction; an explicit instruction still needs testing to see whether the model follows it consistently.

**Pattern:** The same omission occurred across all five topics and all three runs, even though answer-bearing chunks were retrieved. This points to a shared generation instruction rather than a topic-specific retrieval failure. Criteria 1–4 met their measured targets, so criterion 5 is the only baseline miss to diagnose.

**Improvement to test in Milestone 4:** Add an explicit instruction to end in-corpus answers with one relevant follow-up question about information supported by the retrieved documents, while keeping the answer grounded and retaining its source citation. Then rerun the full evaluation to measure whether the follow-up rate improves without losing the other targets.



## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
