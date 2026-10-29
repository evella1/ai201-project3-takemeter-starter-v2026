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

Examples:
-  Praises location and staff but says "stay at one of the other hotels" → 'negative' (verdict wins)
- "Rude staff and sirens", but "Perfect city centre hotel!!!" → 'positive' (verdict wins)
- "Let's say it's ho-hum, good only below $200"→ 'mixed' (hedged verdict, balanced)
- "Great location and staff, but very little sleep due to sirens", no verdict → 'mixed'
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
| positive | 134 | 67% |
| negative | 33 | 16.5% |
| mixed | 33 | 16.5% |
| **Total** | 200 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
*The post:* "So, where to start - I always ask for a bathtub as I love a long soak. There is a bathtub but its half size so no chance of a soak. This could easily have been pointed out when booking but wasn't. On arrival, when asking for a full size tub was told it was $200 a day more for room with full size tub. Also, I'm pretty sure we're in a standard/superior room when we booked & paid for a deluxe. Our room & bed are tiny, friends staying here have much larger room & bed & we booked the same. Fortunately, its such a great city so you don't spend too much time in your room. The bed is small but very comfortable, Frette sheets delightful (& robes & Aveda toiletries). Asked for late check out only to be informed that from 12 noon to 6 pm would cost $249! How?? Lastly, the free wine & breakfast are a really nice treat but I have never been to a hotel that has a room service menu that doesn't have any breakfast items on it. Maybe its an NYC thing not to want to have breakfast in your room.... Location wise, the hotel wins, & the staff gave us great bar recommendations. When returning to my room after 5pm yesterday house-keeping hadn't managed to get to my room. Surely not 4 star service, turn-down/house-keeping happened when I was out for dinner but no choccies tonight!! On the other hand, we did recieve a bottle of wine when we mentioned it was our anniversary. As I say very mixed feelings. Can't say I'd reccomend this place..........."

*Could have been:* mixed or negative

*I chose, because:* negative. The reviewer calls it "mixed", but "Can't say I'd recommend" is a clear verdict, and the verdict rule comes first.

**2.**
*The post:* "I recently stayed at the Westin Copley in Boston, MA while visiting family for Thanksgiving. I stayed in a Junior Suite (Room 1001). Things got off to a negative start at check-in as when I made my reservation, I was told I would have a room on a high floor. The 10th floor is not a high floor, in my opinion. Upon entering the room, I was definitely impressed with the size of the room and the view, despite not being higher up. Let me start with the good before I finish with the bad. The view was spectacular, the room was neat, and it may have had the most comfortable bed that I have ever slept on. Also, the staff was very friendly. Unfortunately, I have more bad comments. The desk in the room was so wobbly that it was un-useable. The bathroom is tiny, outdated, and I find it hard to believe that a hotel charging this much a night still has a shower/tub combo with a shower curtain. It did have a nice shower head but it was useless since the water pressure was so weak. The door in the bathroom is worn, weathered , and generally not in great shape. I had an issue with the TV reception the entire stay. I arrived on a Wednesday and noticed it but did not notify the front desk. Thursday morning it was so bad that I couldn't even watch it so I called the service number. A gentleman came up , did a few things behind the tv and said he had to reset the satellite. The reception never got any better. The noise wasn't a problem here but it is a tad annoying that the doors slam when closing, so you hear that in the hall. I expect that at an economy hotel when I travel on business but did not expect that here. One other troubling note was that there is no security with regards to the elevators to the guest rooms. Anyone can walk in and go to any floor without a room key. One final issue was the morning of check out , it took forever to get a guest services rep on the phone to answer a question that I had. I don't think all of these negatives are a big deal , individually, but add them all up over a 3 day/2 night stay and it impacts your overall experience. Add in the hefty rates, and I would say that the experience was average at best. I am not likely to recommend this hotel to anyone unless they had a super-cheap deal."

*Could have been:* mixed or negative

*I chose, because:* negative. The verdict is hedged ("unless… super-cheap deal"), so I went by weight. The reviewer says they have "more bad comments", and the complaints clearly dominate.

**3.**
*The post:* "This is a well managed establishment. Staff very friendly and efficient at check-in. Problem with my desk lamp was fixed 3o minutes after calling in the problem and someone called to follow up with me to make sure all was well. Laundry promptly picked up after I called.
Rooms are clean, spacious and quiet even though near main throughfare. Very limited movie choice compared to other hotels. Strong internet WIFI signal. 
On a Monday night at 9 PM, they had closed their signature sefood and steak main dining room because ""it was slow"" and patrons had to eat in the bar instead which I felt was a bit strange. Service from the bar was quick and friendly though.
Gym is well equipped with TV screens on most machines. 
Decor in rooms and common areas a bit pretentious and stuffy for my taste so I probbaly won't be back, but that is a subjective matter.
I spent three nights here and have no complaints."

*Could have been:* positive or mixed

*I chose, because:* positive. "Probably won't be back" is hedged and the reviewer calls it a matter of taste, so it isn't a firm verdict. By weight, the review is clearly positive.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, seed 42, max length 128 tokens.
Trained on CPU (torch 2.14.1+cpu).

**Anything I changed from the defaults, and why:**
No changes for the first run. For the improvement run I changed epochs from 3 to 10 (see The Improvement).

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

| Label | Train | Val | Test |
|---|---|---|---|
| positive | 93 | 21 | 20 |
| negative | 23 | 5 | 5 |
| mixed | 23 | 5 | 5 |
| **Total** | 139 | 31 | 30 |

'negative' and 'mixed' have only 5 examples each in the test split (under 8), so their F1 will move a lot between seeds.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I asked AI to read the labelling rules in this README, label reviews 21-200 as positive, negative or mixed, list the cases it found hard, and fill in the label counts.
- *What came back:* Labels for all 180 reviews and the counts. It also found two problems in my cold-labelled rows: a typo in a label and one review I had left unlabelled.
- *What I changed:* I fixed the typo and labelled the missing row as positive. I skimmed the reviews to verify the labels. The hard cases showed me that my verdict rule doesn't say how strong a verdict must be to override the overall weight of the review.

**Moment 2**

- *What I asked for:* I asked AI to point out the hard cases it encountered while labelling.
- *What came back:* Ten hard cases.
- *What I changed:* I carefully re-checked the labels for all ten and changed the label for one of them. I then selected the three that best represent the decision rules and updated the hard-cases section of the README.


**Pre-labelling disclosure:**

I cold-labelled the first 20 reviews myself. Reviews 21-200 were pre-labelled by Claude using my taxonomy and decision rule. I then checked them by skimming, except for the ten hard cases, which I verified closely.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.767 | 0.667 | −0.100 |
| Macro F1 | 0.613 | 0.267 | −0.346 |
| F1 — `positive` | 0.905 | 0.800 | −0.105 |
| F1 — `negative` | 0.571 | 0.000 | −0.571 |
| F1 — `mixed` | 0.364 | 0.000 | −0.364 |

Difference = trained minus baseline.

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

I predicted the model would skew positive, because positive reviews are 67% of my data.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->

My fine-tuned model did worse than a general model that was never trained on my
data, on every measure. The baseline finds some negative and mixed reviews (F1 0.57
and 0.36); the trained model finds none (F1 0.00 for both). So fine-tuning has not
yet taught the model to recognise my two smaller labels.



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Macro-F1 | ≥ 0.70 on all seeds | 0.267 | 0.267 | 0.267 | MISSED |
| 2. Per-label F1 | positive & negative ≥ 0.80; mixed ≥ 0.50 | pos 0.80 / neg 0.00 / mix 0.00 | pos 0.80 / neg 0.00 / mix 0.00 | pos 0.80 / neg 0.00 / mix 0.00 | MISSED |
| 3. Label balance | no label < 10% or > 80% | pos 67% / neg 16.5% / mix 16.5% | same (whole dataset) | same (whole dataset) | MET |
| 4. Self-agreement | ≥ 0.60 on 50 re-labelled reviews | — | — | — | NOT MEASURED (re-labelling not done yet) |
| 5. Confidence | most-confident third ≥ 90% accurate (revised) | 0.90 | 1.00 | 0.80 | MISSED (seed 2024) |

**Raw spread table (notebook section 6):**

<!-- Paste the table printed by the "The numbers, as a table" cell here,
     followed by the stability note line it prints underneath. -->

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.667 | 0.667 | 0.667 | 0.000 |
| Macro F1 | 0.267 | 0.267 | 0.267 | 0.000 |
| F1 — `positive` | 0.800 | 0.800 | 0.800 | 0.000 |
| F1 — `negative` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `mixed` | 0.000 | 0.000 | 0.000 | 0.000 |

Accuracy moved 0.000 across seeds. Stable enough to trust.

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | positive | negative | mixed |
|---|---|---|---|
| **positive** | 20 | 0 | 0 |
| **negative** | 5 | 0 | 0 |
| **mixed** | 5 | 0 | 0 |


**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

All 5 real negative reviews and all 5 real mixed reviews were called positive, and
nothing ever went the other way. The model never learned the positive/negative or
positive/mixed boundary at all; it collapsed to always guessing the majority label.
This happened identically on all three seeds (accuracy 0.667 = exactly the positive
share of the test set; macro-F1 0.267).

---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Macro-F1 | ≥ 0.70 on all three seeds | MISSED | 0.267 on every seed, far below 0.70. It has to hold on all three; it holds on none. |
| 2 | Per-label F1 | positive & negative ≥ 0.80; mixed ≥ 0.50 | MISSED | Positive reaches 0.80, but negative and mixed are 0.00 on every seed. The model never predicts either. |
| 3 | Label balance | no label < 10% or > 80% | MET | positive 67%, negative 16.5%, mixed 16.5% of 200 rows. A dataset property, so the same for every seed. |
| 4 | Self-agreement | ≥ 0.60 on 50 re-labelled reviews | NOT MEASURED | The second round of labelling hasn't been done yet, so there's nothing to compare. |
| 5 | Confidence | most-confident third ≥ 90% accurate (revised) | MISSED | Revised because the original 0.80 cutoff couldn't be measured (see criteria.md). The most-confident third was 0.90 and 1.00 accurate, but 0.80 on seed 2024. |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

Criteria 1 and 2 (macro-F1 and per-label F1) were missed because the model labels every review positive.

Cause: too few negative and mixed examples (23 each in training, against 93 positive) and too little training (27 steps). Guessing positive every time was the easiest way for the model to lower its loss.

How I know: validation accuracy stayed at 0.677 (the positive share of the validation set) for every epoch on every seed, even as the loss went down. Validation loss was still falling when training stopped (0.869, 0.818, 0.806 over the three epochs), so the model had not finished learning. In the confusion matrix every review is predicted positive, and the test accuracy of 0.667 is just the positive share of the test set (20 of 30).

Labelling or model problem: I agreed with the staff labels on 26 of 30 posts (87%), and all four disagreements were the same kind of mistake, not random ones. So I apply a written rule consistently. Inconsistent labels would also make the model mix labels up, but this model never predicts negative or mixed at all. And the baseline, using my definitions, does tell the labels apart. This points to the model and the amount of data, not my labels.


---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 26 / 30 = 87%


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
     
| Post | Staff said | I said | My call, and why |
|---|---|---|---|
| Everyone blames the hill at 18 miles. I split the field into people w… | `analysis` | `hot_take` |  |
| Carbon plates aren't the whole story. My times dropped 2% in the shoe… | `analysis` | `hot_take` |  |
| Checked the entry lists. 41% of this year's field have never run the … | `analysis` | `hot_take` |  |
| Three of us ran the same session on the same day, different shoes, wi… | `analysis` | `hot_take` |  |

**1.**
*The post:* "Everyone blames the hill at 18 miles. I split the field into people who ran the first half under 1:45 and over, and the slowdown after 18 was almost identical for both groups. If the hill were the problem the fast starters would fall apart harder."

*Staff said / I said:* Staff said `analysis` / I said `hot_take`

*My call, and why:* Staff is right. The split by first-half pace is the whole argument without it nothing is left. I marked it hot_take because the conclusion might be coincidental, but Rule 1 only asks whether the fact carries the argument, not whether it proves it.

*Which pile:* (a)

**2.**
*The post:* "Carbon plates aren't the whole story. My times dropped 2% in the shoes and another 2% the following year in the same pair, when the only thing that changed was going from 35 to 50 miles a week."

*Staff said / I said:* Staff said `analysis` / I said `hot_take`

*My call, and why:* Staff is right. The 2% figures are the reason given, so they carry the argument. I judged the evidence too thin to prove the cause, which is a stricter test than Rule 1 sets.

*Which pile:* (a)

**3.**
*The post:* "Checked the entry lists. 41% of this year's field have never run the distance before, against 26% five years ago. That's why the finish-line photos look different, not because standards dropped."

*Staff said / I said:* Staff said `analysis` / I said `hot_take`

*My call, and why:* Staff is right. The 41% vs 26% comparison is the argument, much like the rule's own `analysis` example (times down 3% since 2017). That example is a correlation too, so "might be coincidental" doesn't make it a hot_take.

*Which pile:* (a)

**4.**
*The post:* "Three of us ran the same session on the same day, different shoes, within 10 seconds of each other over 5 miles. I'm not saying shoes do nothing, but whatever they do was smaller than the noise between three people."

*Staff said / I said:* Staff said `analysis` / I said `hot_take`

*My call, and why:* Staff is right. The 10-second spread across three runners is what the claim rests on, and the writer even hedges it. I discounted it as a small sample, but Rule 1 doesn't ask about sample size.

*Which pile:* (a)

**What the pattern in my disagreements tells me:**

All four go the same way: staff `analysis`, me `hot_take`. I consistently judged whether the conclusion actually follows from the data, while Rule 1 only asks whether the argument rests on a checkable fact. So this is a systematic stricter reading on my side, not unclear rules.



---

## The Improvement

**What I changed:**

Epochs from 3 to 10. Nothing else changed.

**Which diagnosis pointed at it:**

The diagnosis named two causes, too few examples and too little training. I picked training because validation loss was still falling at the last epoch and the predictions never moved off positive, so the model was not yet using the 23 negative and 23 mixed examples it already had. Adding more examples would not fix that. I ruled out relabelling because my agreement check showed one consistent reading of the rule, not a rule applied two ways.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Macro-F1 | ≥ 0.70 on all seeds | 0.798 | 0.510 | 0.474 | MISSED (only seed 42) |
| 2. Per-label F1 | positive & negative ≥ 0.80; mixed ≥ 0.50 | pos 0.93 / neg 0.80 / mix 0.67 | pos 0.86 / neg 0.67 / mix 0.00 | pos 0.85 / neg 0.57 / mix 0.00 | MISSED (positive meets it on all seeds; negative and mixed only on seed 42) |
| 3. Label balance | no label < 10% or > 80% | pos 67% / neg 16.5% / mix 16.5% | same (whole dataset) | same (whole dataset) | MET |
| 4. Self-agreement | ≥ 0.60 on 50 re-labelled reviews | — | — | — | NOT MEASURED |
| 5. Confidence | most-confident third ≥ 90% accurate (revised) | 1.00 | 1.00 | 1.00 | MET |

Accuracy across seeds: 0.867 / 0.733 / 0.733 (spread 0.133, more than 10 points).


**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->

Yes, partly. The model no longer guesses positive for everything. Macro-F1 went from 0.267 on every seed to 0.798, 0.510 and 0.474, and negative F1 went from 0.00 to 0.80, 0.67 and 0.57. On seed 42 it now beats the baseline (macro-F1 0.798 vs 0.613, same 30 reviews).

But no new criterion is met on all three seeds. Mixed is still the problem: on seeds 7 and 2024 the model got none of the 5 mixed reviews right, and most of them were called positive (4 of 5 and 5 of 5). Accuracy also moved 0.133 between seeds, so the result depends a lot on which reviews land in the test set. With only 5 mixed and 5 negative reviews in each test set, one review changes their F1 a lot.

So more training fixed the collapse, which confirms that part of the diagnosis. What is left points at the other cause: too few mixed examples.


---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->

Criteria 1 and 2: mixed is only learned on one seed out of three. Next I would add about 30 more mixed reviews, labelled carefully, since mixed has the worst F1. More data would also make the test set bigger and the scores more stable. I stopped at one change because the brief asks for one.

Criterion 4: not measured. It needs a second round of labelling on 50 reviews, which I haven't done.

Criterion 5: revised because the original couldn't be measured. Met after the improvement.


**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->

I meant mixed to capture reviews with significant praise and significant complaints. The model mostly reads them as positive: on seeds 7 and 2024, 9 of the 10 mixed reviews in the test sets were called positive.


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
