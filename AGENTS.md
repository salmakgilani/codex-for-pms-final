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
