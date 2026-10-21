# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

My data comes from the TripAdvisor hotel reviews dataset (jniimi/tripadvisor-review-rating), a public collection on Hugging Face (https://huggingface.co/datasets/jniimi/tripadvisor-review-rating). Only the review text is used. My community is travelers posting hotel reviews on TripAdvisor. My classifier labels each review as positive, negative, or mixed.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `Positive`

**Definition:** 
Review is mostly favorable, with no major complaints.

**Example 1:**
>
Really excellent Hilton\nStayed here on business trips and the hotel is excellent for business trips. Staff is excellent and really helpful. VERY clean hotel. Best choice in San Diego; especially a Hilton.

**Example 2:**
>
Exceptional service and comfort. Spent two nights here for a wedding in Brooklyn. The staff members were all exceptionally friendly and professional, from the doormen and bellhops to the front-desk and concierge. I was especially grateful that Kevin, a bartender in the Ritz's off-lobby bar, was able to tie my bowtie -- I would have looked pretty foolish otherwise. He made it look perfect. Our room was spacious and comfortable, with a large bathroom and king-size bed that provided some of the best sleep I can remember in a hotel. My only disappointment was that the fitness center did not offer a sauna or steam room.

### `Negative`

**Definition:**
Review is mostly unfavorable, with no major praise.

**Example 1:**
>
Room is so small that I was initially claustrophobic. Bathroom is about as small as our toilet at home. Double beds should be outlawed! Who can get a good night's sleep in a bed so tiny? Staff are indifferent, although concierge was very helpful. No help offered with our 5 bags, no tea & coffee making facilities in room & no food services on site. Location is ok for walking to Times Square & Empire State Building, but not a good area for decent restaurants. This is not value for money & we would not stay here again.


**Example 2:**
>
Getting to the room was interesting - we had to walk down an outdoor corridor, with gates, dragging our bags and at night... not really the most convenient access. We had an early flight the next morning to Hawaii, as well as other guests apparently, because we ended up with people sitting on the floor of the shuttle to get to the airport. Oh, and the driver totally didn't know what she was doing - we went to the wrong airline/terminal. The room was not fabulously clean, both in the sleeping area and the bathroom. Plus, our room(s) were on the ground floor and there was a car parked outside our window with lights on until about midnight.

### `Mixed`

**Definition:**
Significant praise and significant complaints.

**Example 1:**
>
Stayed one night. Looks like every holiday inn express. Friendly staff at check-in. Pillows were poor quality. I did got upgraded to suite which offers lots of space. Got free popcorn at check in. Not a lot to do in the evening.

**Example 2:**
>
I know it’s pricey, but as they say location is everything and this is right in the middle of where we wanted to be (Times Square), so that helped our trip be even more enjoyable. The cost here is still comparable to prices of other hotels in the area, but I wish they would include WiFi in the guest rooms (especially since AT&T had two towers go down in the area, and my data connection was at zero almost the entire time of my trip). And I wish the hotel had better pillows and mattresses (like TempurPedic), but what they had was pretty standard for most any hotel anywhere and not what I would expect in a luxury hotel. I highly recommend a Times Square view and a high floor. Ours was on the 35th floor and an excellent (if not the best) view, and it was quiet and not noisy at all. I was told our room could go for $1800 on the night of New Years Eve. Our bathroom had a broken tile on the floor, and the water drained slowly out of the bathtub, but otherwise this was a great room. Another bad side is that the hotel twice lost a discount authorization form I had submitted to them in March, and that created a delay during checkin. They said they have no system for what to do with forms that are submitted more than two weeks in advance of the trip.

### The hardest boundary

**Which two labels:**

mixed vs. positive/negative

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

1. If the reviewer states an explicit, unconditional verdict (recommends it,
   would return, "would avoid", "never again"), label by that verdict, even if
   the review also lists complaints or praise on the other side.
2. If there is no verdict (or it is hedged, e.g. "good only if under $200"),
   label by weight: if one side is only a brief or minor mention, use the
   dominant side.
3. Label 'mixed' only if neither applies: substantial praise and substantial
   criticism, with no clear verdict.
---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
My data comes from the TripAdvisor hotel reviews dataset (jniimi/tripadvisor-review-rating), a public collection on Hugging Face (https://huggingface.co/datasets/jniimi/tripadvisor-review-rating).


**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->

Cold labelled the first 20, the rest was labeled with AI and more or less checked. 
I also asked AI to point out those that were the hardest to label, reviewed the labels carefully and tightened the criteria.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |
| **Total** |  | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**2.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**3.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**

**Settings:** <!-- epochs, learning rate, batch size, seed -->

**Anything I changed from the defaults, and why:**

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:**

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
