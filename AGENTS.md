# Rook Industries: course working file

## Session scope, Product School lab

This directory is coursework for Product School's "Codex for PMs"
certification. Everything in it is a fictional teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.codex-for-pms/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it); and, only if the student once said no to trusting this folder, change that one trust setting for this folder in Codex's settings file, keeping a copy of the file first.
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

### Role, company and products
- You are the incoming PM for Rook Dispatch, succeeding Priya Raghunathan, who left 21 August 2026. Rook sells subscriptions per active responder; responders are independent customers, not employees. Handlers are the main users.
- **Dispatch** is the flagship: handlers enter incidents in a web console; Dispatch ranks available responders and pings their native phone apps one at a time. Taking a ping assigns the incident and marks the responder engaged; turning it down or missing it sends the offer onward.
- **Supply** handles equipment requisitions, quartermaster approvals, fulfillment, maintenance and field failure reports. Dispatch writes the shared **Responder Availability Record**; Supply reads it to schedule maintenance around expected callout load. Availability changes can affect Supply maintenance.
- Releases run monthly; routing configuration ships with releases, rather than being adjustable by handlers at runtime.

### People to work with
| Person | Role and why to involve them |
| --- | --- |
| Helen Achebe | Director of Product; your director; owns roadmap and commitments. Confirm what remains committed after 4.2. |
| Marcus Oyelaran | Dispatch Engineering Manager; first contact for engineering, feasibility and help getting numbers. |
| Wen Li | Dispatch Staff Engineer; built responder ranking; key source for understanding routing. |
| Ravi Menon | Data Analyst; owns numbers and weekly ping acceptance reporting. |
| Nadia Hoffmann | Support Lead; owns tickets and handler feedback; schedule regular contact. |
| Sofia Marino | Product Designer; owns console and phone app; ran September interviews. |
| Priya Raghunathan | Former Dispatch PM; left a 21 August handover. |

### Vocabulary and measures
- **Callout:** incident requesting a responder. **Ping:** one offer to one responder. **Taken:** accepted; **turned down:** explicitly declined; **missed:** no answer before the **ping wait** expires. Keep declines and misses separate.
- **Acceptance rate:** pings taken / all pings (taken, turned down or missed); headline metric, reported weekly in aggregate. **Time-to-accept:** median seconds from ping sent to taken.
- **Coverage gap:** no available responder has the required capability tags; distinct from an offer not being accepted.
- **Routing priority:** ranking using travel-time proximity, availability, capability match and recent acceptance history. Turning down or missing pings lowers the recent-acceptance component and later ranking. **Capability tags** describe competencies required by incidents.
- **Handler:** manages responder readiness. **Quartermaster:** owns stock/approvals. **Requisition:** equipment request. **Service interval:** maintenance frequency. **Field failure report:** gear failure, potentially bringing maintenance forward.
- **Mutual aid/shared cover:** responders covering other areas; not supported in the documented product, Q4 exploration only.
- **Cover identity:** responder persona. Rook stores no mapping to legal identity. Do not infer identities or design features assuming such a mapping; consult Security Policy 4.1 before designing changes to responder records (policy not yet read).

### Where things stand — source snapshot, not live status
- **4.0 (7 April 2026):** console navigation, responder profile redesign, routing-override audit log. **4.1 (16 June):** travel-time proximity, bulk callout, phone push reliability. Priya describes console and mobile as stable since 4.1.
- **4.2 (12 August):** proximity weighted more heavily relative to acceptance history; ping wait reduced from 90 to 60 seconds; console filters persist; three defect fixes. Proximity addressed complaints about nearby responders being skipped for distant ones with stronger acceptance histories.
- Priya reports fewer pings taken and more handler complaints after 4.2. She believes August seasonality is the main cause and argues against reverting. Treat this as a hypothesis: ranking and timeout changed together, and causality is unresolved. 
- Q3 roadmap was last reviewed **30 June** and still lists Priya as owner. It marks routing changes, timeout tuning and **Availability Confidence** (score beside stated availability) committed to 4.2. The first two shipped; Availability Confidence is absent from release notes. Confirm its status with Helen; Priya mentions multiple unnamed deferrals.
- **Requisition approval chains** (second approval for large Supply requests) is committed to 4.3. **Handler phone app** and **shared cover** are Q4 explorations. Numbered-release commitments require changes through Product. Reconcile the stale roadmap with Helen before promising dates.
- First priorities: investigate acceptance and complaints with Ravi, Nadia, Marcus and Wen; document how ranking actually works; clarify deferred commitments with Helen. Priya calls filter-persistence complaints cosmetic, but that prioritization is her judgment and needs evidence.

### Sources and working material
- Wiki: Company → About Rook, Rook Dispatch, Rook Supply, Glossary, Team directory, Releases and Q3 roadmap (including roadmap item pages). Directory updated 2 September 2026.
- Handover: `00-rook/company/notes/handoff-from-priya.docx`. Local routing code: `00-rook/code/dispatch-routing/`. Further evidence: wiki Research/customer interviews; read-only database tables: `callouts`, `pings`, `handlers`, `responders`, `support_tickets` (not yet analyzed).

- Onboarding plan: align priorities and inherited commitments with Helen, trace real callouts with Wen/Marcus, and combine Ravi's analysis with Nadia's tickets and Sofia's interviews before selecting a change.
- Measure both ping outcomes and complete callout journeys: eventual assignment, offers needed and elapsed time to assignment where data permits. Lower ping acceptance alone does not establish worse assignment outcomes.
- Available callout/ping data spans late June to early September 2026; it cannot establish recurring August seasonality without older reporting. No operational analysis was performed this session.
- Open customer questions: who pays, signs and renews each subscription; how responder and handler needs differ; whether filter persistence causes workflow harm rather than merely cosmetic complaints.

- Reviewed all four September handler interviews and all 147 support tickets. The ticket comparison uses June 29–August 11 (40) and August 13–September 7 (107), with none filed August 12.
- After 4.2, 30 tickets reported fewer or no offers and 15 reported offers moving on before responders could answer, versus zero in either category before. Counts include repeat reports, and quiet-ping tickets name four responders.
- The interviews corroborate both symptoms. Kip alone described one responder overloaded while another was quiet. Three interviewees wanted clearer alerts, while two tickets asked for more distinctive console sounds.
- Filter feedback shifted from asking for persistence to reporting silent resets and other problems using saved filters. Ambrose reported watching the wrong view after a reset.
- Trace reported cases with Ravi and Wen to compare who was eligible and got the first offer, when offers expired, and whether assignment rates or time to assignment changed. The effect of either 4.2 routing change remains open.

- Module 3 queried ping records through September 6. Acceptance was 831/1,085 (76.6%) before August 12 and 380/586 (64.8%) from August 13 onward, down 11.7 percentage points; release day is separate (11/25). All 188 responder-period rows are in `03-rewind/ping-responder-weeks.md`.
- Correction to the earlier ticket summary: #3041 was filed August 12. The 107 tickets previously labeled August 13 onward include that ticket; the strict post-day count is 106, including 14 fast-expiry complaints, plus one on release day.
- Misses rose from 3/186 pings during August 5–11 to 7/25 on August 12 and 12/25 on August 13. Fast-expiry complaints began August 12; quiet-stretch reports began August 17 for Farlight and August 20 for Undertow and Ashgrove.
- Farlight's weekly pings/taken from June 29 were 12/9, 12/9, 11/8, 12/9, 13/10, 12/8, 10/4, 3/0, 1/0, 0/0. Release week mixed pre/post dates; misses rose before offer volume collapsed. Eligibility and ranking evidence is still needed to explain the sequence.
- Next measure assignment share across all callouts, including those with no pings, plus time and offers to assignment. No callout-outcome analysis or experiment was performed. Independently testing ranking and wait settings was discussed as a proposal, not an implemented test.

- Module 4 read all dispatch-routing files. `offer.py` stops at the first acceptance; available responders further down the list can receive no offer. `history.py` subtracts 0.12 for both misses and explicit declines, adds 0.08 only for acceptance, and has no time-based recovery. An unknown responder starts at 0.5; scores are bounded at 0 and 1.
- The recovery path requires availability, an offer reaching the responder, and acceptance within 60 seconds. Waiting alone does not restore the stored score. Farlight's weekly data supports the proposed misses-to-lower-ranking sequence, but historical eligibility and ranking records are needed to confirm causation.
- Marcus's August 14 release-page comment asks whether the new ranking weights were intended to apply to responders who turn jobs down. Code applies the weights to everyone; proximity rises from 0.45 to 0.60 and acceptance history falls from 0.40 to 0.25. Whether this scope was deliberate remains undocumented in the sources read.
- Code also scores partial capability matches without excluding them and checks availability only when building the list. Confirm mandatory-capability checks, readiness rechecks, and late-answer handling in the full system; phone and availability connections in this folder are placeholders.
- Discussed separating miss penalties, recovery rules, treatment of existing low scores, handler explanations, support escalation, and monitoring quiet responders. These require agreed policy and verifiable criteria as well as implementation; sample code was illustrative only and no routing files were changed.

- Module 5 delivered `05-super-speed/brief.md` for Helen, centered on Farlight, with source details, proposed recovery policy, acceptance criteria, owners and pilot evaluation. It grew beyond the requested one-pager during refinement; thresholds, response times and policy are recommendations, not approved Rook rules.
- The brief proposes reviewing historical miss penalties, preserving explicit declines, correcting only supported history with responder confirmation, and protecting new misses during a monitored pilot. Historical ranking evidence, durable scoring and phone confirmation capability still need verification; one acceptance does not establish sustained recovery.
- Fresh Farlight ticket queries returned different identifiers than earlier sessions. Brief sources use current verified records: #3068 August 17 availability checked, #3118 August 29 disappearing offer, and #3128 September 1 responder quote. Farlight's weekly ping totals still matched the Module 3 extract.
- `05-super-speed/prototype.html` is a standalone local interactive prototype. It includes a 16-responder dispatcher overview with search and status filters, Farlight's coverage card, Support review and approvals, responder confirmation, correction retry, sample offers and escalation. Her phone reflects current request, correction, recovery and withdrawal states.
- Prototype availability, scores, evidence and live events are explicitly illustrative. Interaction checks passed using a simulated document environment; browser appearance was not visually verified. No production integration, routing change or real correction was made.

- Module 6 created `.agents/skills/review-checklist/SKILL.md`, a project-local skill checking ownership, measurable success, consistent scope, and problem-before-fix. It reports evidence and minimal improvements without editing unless asked.
- The skill was saved to GitHub. Its file was manually checked; automated validation could not run because the `python` command was unavailable. The skill has not yet been run against the brief.
- A Monday 9 a.m. review automation was created and then deleted at the student's request. No recurring review schedule remains from this session.
