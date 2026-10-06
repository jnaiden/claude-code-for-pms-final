# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

Sources: `00-rook/company/notes/handoff-from-priya.docx` (21 Aug 2026) and the
company section of the Rook wiki (rook-wiki connector). Wiki roadmap was last
reviewed 2026-06-30, so treat "status" there as stale.

### Me and my job
I'm the new PM for Rook Dispatch, replacing Priya Raghunathan (left 21 Aug 2026;
no overlap, she was sole Dispatch PM for 14 months). I report to Helen Achebe.

### The company and products
Rook Industries (founded 2014, 241 staff, HQ Site Aleph, offices in Berlin and
Singapore) sells coordination and provisioning software to independent masked
responders and the handlers/quartermasters who support them. Rook does not
employ responders. Revenue is subscription, priced per active responder.
Monthly release train; point releases are 4.x.

- **Rook Dispatch** (mine, flagship, current release 4.2). Incident comes in, Dispatch
  ranks available responders, pings the top one's phone, they take it or not, and it
  moves down the list. Handlers use the web console; responders use a native phone app.
  Routing config ships in the release; handlers can't change it at runtime.
- **Rook Supply**. Gear: requisitions, quartermaster approval, maintenance schedules,
  field failure reports. Reads the Responder Availability Record that Dispatch writes,
  and schedules maintenance into low-callout-load windows, so any change to how Dispatch
  computes availability silently changes Supply scheduling.

**Confidentiality rule:** cover identities are never stored or mappable to legal
identities (Security Policy 4.1). Never design for, or try to work out, who a responder is.

### People (Dispatch)
- **Helen Achebe**, Director of Product, my boss. Owns roadmap and commitments. Gives room.
- **Marcus Oyelaran**, Engineering Manager (Site Aleph). Straight talker; first stop when
  unsure; can pull numbers.
- **Wen Li**, Staff Engineer (Berlin). Built the who-gets-pinged logic; the best source on
  ranking, since no written description exists. Was away 14-24 Aug.
- **Nadia Hoffmann**, Support Lead (Berlin). Owns tickets; hears handler complaints first.
  Priya recommended a standing 15 minutes.
- **Ravi Menon**, Data Analyst (Singapore). Weekly acceptance-rate reporting.
- **Sofia Marino**, Product Designer. Console and phone app; ran the September interviews.

### Vocabulary
- **Responder**: independent, non-employee; a record of capability tags + availability.
  **Handler**: looks after specific responders and is who actually uses the console.
  **Quartermaster**: Supply approver.
- **Callout**: request to attend an incident. **Ping**: a callout offered to one responder.
  Outcomes: **taken**, **turned down**, or **missed** (ping wait expired). Turned down and
  missed both move it on but are recorded separately.
- **Ping wait**: how long a ping sits before counting as missed; one value for everyone.
- **Routing priority**: ranking score. Inputs: proximity (travel-time estimate),
  availability, capability match, recent acceptance history. Declines and misses lower
  future ranking, which is a feedback loop worth remembering.
- **Acceptance rate**: pings taken / pings offered; the headline metric (weekly, aggregate).
  **Time-to-accept**: median seconds ping to taken. **Coverage gap**: no available
  responder had the required tags; that is not a low acceptance rate.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather, aquatic,
  crowd-management, de-escalation. **Mutual aid**: cross-area cover; unsupported, on Q4 list.

### Where things stand (as of early Oct 2026)
- **Releases:** 4.0 (7 Apr: new nav, profile redesign, routing override audit log);
  4.1 (16 Jun: travel-time proximity, bulk callout, push reliability); **4.2 (12 Aug)**:
  proximity weighted up vs. recent acceptance, **ping wait cut 90s to 60s**, persistent
  console filters, three fixes (unnamed). Priya called mobile and console stable, but a
  September handler says persisted filters silently reset twice after updates.
- **The open problem (data, rook-database, 29 Jun-6 Sep):** acceptance held ~75-78% weekly
  through 10 Aug, then fell to 54% (week of 10 Aug), 66%, 67%, 73%. The drop is **missed
  pings: 2.3% of pings before 4.2, 18% after**; turned-down actually fell (21% to 18%). That
  fits the 90s-to-60s ping wait, not seasonality. Callout volume dipped ~10-20% (about
  139/week to 110-127), so some softness is real, but it doesn't explain the misses.
  Tickets ran 5-8/week, then 20-32/week; the "gone before he could answer" and "hardly
  anything for X" tickets are mostly still open.
- **Starvation:** Farlight, Meteor Mite, The Undertow and Vesper went from ~10-13
  pings/week to ~1-4 by late Aug, while busier responders (Nightwell, The Gale) got
  more. Cause not proven; consistent with proximity weighting plus decline penalties
  (0.12 vs 0.08 credit) that never decay (TODO(2019) in history.py), and missed pings
  count as declines. Needs Wen Li to confirm. Priya's "mostly seasonal, back in September"
  is not supported by this data, but data stops 6 Sep, so check later weeks. Don't frame it
  as "revert 4.2": the ranking change was long requested. Splitting the wait cut from
  the weight change is the open question.
- **Roadmap vs. reality:** committed for 4.2 were pinged-change, ping timeout tuning, and
  **Availability Confidence**. Availability Confidence is not in the 4.2 notes; Priya says
  a couple of items were squeezed out. Which are still Q3 commitments has not been
  discussed with Helen and needs to be. Requisition approval chains (Supply) are
  committed for 4.3. Handler phone app and shared cover between responders are Q4
  "exploring".
- **Filter persistence:** Priya called it cosmetic noise. A handler values it but wants a
  warning when it resets. Still low priority next to routing; don't let it eat the first month.
- **Gaps I'm expected to fill:** write the missing description of how pings are decided
  (with Wen Li).

### How to help me
Be plain about what's established versus hypothesis. Check numbers with Ravi's data before
asserting causes. Treat the wiki roadmap and Priya's handoff as inputs, not truth.

- Before 4.2 (29 Jun-11 Aug) nothing was broken: acceptance steady at 75-78% a week, misses
  about 2%, no starved responders, 5-8 tickets a week. The break is sharp at 12 Aug.
- Where the evidence lives: routing code in `00-rook/code/dispatch-routing/` (config.py
  holds the 4.2 weights and 60s wait); database tables callouts, pings, responders,
  handlers, support_tickets (29 Jun-6 Sep); interviews and briefs in the wiki's Research
  and Product briefs pages. `00-rook/feedback/` is empty.
- My working recommendation: restore the 90s ping wait on its own first, then fix the
  scoring loop (let scores drift to neutral, stop treating a miss as a full decline); keep
  the proximity change. Cause is a hypothesis until the two changes are tested separately.
- Open: database stops at 6 Sep so recovery is untested; Wen Li to confirm the loop; Marcus
  to say if the wait can ship alone; Helen conversation on 4.2 commitments still pending;
  Nadia to answer the open "quiet" and "gone before he could answer" tickets.

- Two problems, not one (pings table, 12 Aug-6 Sep): a broad one, where the 12 other
  responders' miss rate rose from ~2% to ~14% (what the 90s wait should fix), and a deep
  one, where Vesper, The Undertow, Farlight and Meteor Mite miss ~59% and are 30% of all
  110 misses. Their last "taken" was 14-19 Aug, and all 20 pings since went unanswered
  (16 missed, 4 turned down). Halfmoon and Ashgrove are only mildly down.
- Tickets (support_tickets, 147, 83 open, all open ones after 12 Aug): 30 "quiet" tickets
  come from only 4 handlers; 15 "gone" tickets come from 11. Filters are 14 tickets, bigger
  than the interviews suggest. Dot, Kip and Halloran filed none, so tickets undercount.
- Interviews (4 handlers, 2-5 Sep): "gone" 3 of 4, alerts 3 of 4, uneven workload 2 of 4.
  Only the interviews show handlers wanting to know a callout is live (supports the Q4
  handler phone app). Halloran's Supply points (requisitions, failure reports, catalog
  search) also appear in tickets from others and pre-date 4.2.
- Why the wait went 90s to 60s is not recorded in the code, changelog, wiki or handoff;
  ask Priya via Marcus before shipping 90s. Don't blanket-reset scores: recompute them
  without 4.2-era misses, stop counting a miss as a full decline, add decay.
- `00-rook/feedback/tickets/` doesn't exist; tickets live in the database. Keep household
  detail from interviews (e.g. Dot's) out of shared docs, per the confidentiality rule.
