# Rook Dispatch 4.2: what changed, and the one number

**To:** Helen Achebe  **From:** Dispatch PM  **Data:** pings, callouts, responders and support tickets, 29 Jun to 6 Sep 2026

## The number

**Missed pings went from 2.3% to 18.0% of pings after 4.2 shipped on 12 Aug.**

This accounts for the whole fall in acceptance rate, from 76.6% to 64.0%. Turned-down pings went down (21.1% to 18.0%), so responders are not saying no more often. They are not answering in time.

| | Before (29 Jun to 11 Aug) | After (12 Aug to 6 Sep) | Change |
|---|---|---|---|
| Pings | 1,085 | 611 | |
| Taken (acceptance rate) | 76.6% | 64.0% | -12.6 pts |
| Turned down | 21.1% | 18.0% | -3.1 pts |
| **Missed** | **2.3%** | **18.0%** | **+15.7 pts** |

I'm reporting this one number because it does not assume a cause, it can be reproduced from the `pings` table, and Ravi can track it weekly. It is also how we will know a fix worked: the target is a return to about 2-3%. Acceptance had held at 75-78% every week for six weeks, fell to 54% in the week 4.2 shipped, and had recovered only to 73% by the week of 31 Aug.

## Where the misses sit

The 18.0% blends three groups that behave very differently. "In area" means the callout was in the responder's usual area, which is a proxy for the proximity input, not the travel-time score itself.

| Pings after 4.2 | Pings | Missed | Missed rate | Before 4.2 |
|---|---|---|---|---|
| 12 responders, callout in their area | 336 | 20 | 6.0% | 0.6% |
| 12 responders, callout outside their area | 219 | 57 | 26.0% | 8.8% |
| Four responders (Farlight, Meteor Mite, The Undertow, Vesper) | 56 | 33 | 58.9% | 2.9% |
| **All** | **611** | **110** | **18.0%** | **2.3%** |

- **Only 20 of the 110 misses (18%) are ordinary responders missing a ping in their own area.** That is the case a shorter ping wait explains most directly, and it is still a tenfold rise.
- **Pings outside the responder's area went from 17% to 39% of all pings, and they miss at about 30%.** That is 72 of the 110 misses.
- **Most of that rise is cover for three regions.** Harborside, Old Town and Uptown each have one home responder, and each is one of the four. Out-of-region pings into those regions rose from 44 to 133, and **86 were taken, against none anywhere before 4.2**. About four-fifths of the 22-point rise in out-of-region share (18 points) is these three regions. Cover from other regions is listed as unsupported in our own notes (mutual aid, Q4), so it is happening without a feature behind it.
- **Out-of-region pings everywhere else are failing.** There were 108, 52 were missed (48%), and none were taken. 20 of the 72 out-of-region misses are in the three covered regions, and 52 are in the rest.
- **The four miss 53% of pings even in their own area**, against 6% for everyone else, so distance does not explain them. Their home regions are now pinged locally very rarely (6 to 10 in-region pings each since 12 Aug).
- **94 of the 110 misses (85%) are on the first ping**, the top-ranked responder.

### A possible early-warning signal

In the first week after 4.2 (12-18 Aug), the four missed 38% of the pings for callouts in their own area (8 of 21), against 5% for the other 12 (4 of 86). Three of the four were already missing own-area pings on 13 Aug, the day after release. Their ping volume only collapsed the following week, and the first handler ticket about it arrived on 17 Aug. Misses on callouts outside a responder's area are not a useful signal, because everyone misses those more often (34% for the other 12 in the same week).

This is a lead, not a rule. It rests on 36 pings across four responders, and one of the four (Vesper) kept their own-area pings clean for six days, so it would not have flagged all four on day one. It is cheap to test, though: if Ravi reports each responder's own-area missed rate weekly, we can see whether it separates the at-risk responders early.

## What it means

- **Slower callouts.** The 90th-percentile time for a callout to reach the responder who took it went from 28s to 62s. Time spent waiting on misses went from about 5 minutes a week to about 24.
- **Callouts going unanswered.** 27 callouts since 12 Aug had a missed ping and no responder took them, against 3 before (6.1% vs 0.3% of callouts). About two-thirds were in four areas, three of them covered by a single responder. 16 of the 27 involved infrastructure or transport. Most of the 27 had only one ping, so the callout stopped after the miss (see open questions).
- **Handler load.** Support tickets went from about 6 a week to about 29, and 83 are open.

## What I know and what I don't

**Established from the data:** the drop is missed pings, not declines. The break is sharp at 12 Aug. The misses sit in the three groups above, and the four responders' rates are far outside everyone else's.

**Hypotheses, not yet tested.** Three things changed with 4.2, and the data cannot separate them:
1. **The 60s ping wait** (cut from 90s). It fits the rise for responders in their own area. But every turned-down ping was answered within 40 seconds before and after 4.2, so no one was visibly answering in the 60-90 second window. I can't see how long responders took to accept, so this is untested.
2. **The ranking change.** Pings outside a responder's area more than doubled in share. About four-fifths of that is cover for the three regions whose home responder is starved, so it follows from problem 3 more than it stands alone. The rest is pings leaving other regions, which miss 48% and are never taken. Wen Li needs to say why.
3. **The four.** The scoring loop (declines carry a penalty that never decays, and a miss counts as a decline) could starve them, but they also miss half of their own-area pings, so a responder-side cause (phones, notifications, availability) has to be ruled out first.

**Open questions:**
- Why was the wait cut from 90s to 60s? It is not recorded anywhere I can find.
- Why do some callouts end after a single missed ping instead of moving to the next responder?
- Is cover of Harborside, Old Town and Uptown by responders from other regions intended? The 86 takes are all there. I cannot see whether those callouts are handled more slowly or worse, because the data does not record what happens after a callout is taken.
- The data stops at 6 Sep, so recovery is untested. The 31 Aug week looked better (acceptance 73%, missed 12.7%), but that is one week.

Callout volume also dipped about 15% (about 139 to about 119 a week), but it does not explain the misses.

## What I'd propose

I am not proposing to revert 4.2. The ranking change was long requested.

1. **Test the 90s wait on its own**, if Marcus confirms it can ship alone and Priya (via Marcus) explains why it was cut. My earlier view was that this should be the first fix. The breakdown now says it is likely to recover only part of the misses, so I would not promise Helen a full recovery from it.
2. **Ask Wen Li about out-of-area routing and the scoring loop.** This is where most of the misses are, including the cover of three regions by responders from elsewhere.
3. **Rule out responder-side causes for the four** by asking their handlers, who are the right channel. I would not contact responders directly.
4. **Then fix the scoring loop.** Recompute scores without 4.2-era misses rather than a blanket reset, stop counting a miss as a full decline, and add decay.
5. **Test the changes separately**, so we learn which one did what.

## Decisions and input I need

- **From you:** which 4.2 commitments still stand for Q3 (Availability Confidence did not ship), and whether I should talk to handlers before we align.
- **From Marcus:** can the wait ship alone?
- **From Wen Li:** why are out-of-area pings up, and does the scoring loop explain the four?
- **From Ravi:** weekly pings, taken, turned down and missed after 6 Sep, per responder, plus accept times if the app logs them, and each responder's missed rate on own-area pings so we can test the early-warning signal.
