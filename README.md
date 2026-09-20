# Marketing Campaign Scoper

Locks the objective and constraints of a video ad campaign before any creative work starts, by interviewing the requester one question at a time and freezing the result into a brief everything downstream reads.

It runs the intake as an interview rather than a form: one question per turn, each with options derived from the landing page rather than from a generic list, and each answer restated so it can be corrected before it hardens. A campaign with two primary KPIs is two campaigns, and it says so instead of averaging them.

Everything it locks traces to one of three places: the landing page copy you supplied, an explicit answer, or a flagged assumption. It never invents a price, a claim, a feature or an audience, because every later stage builds on this document and an invented line here is repeated by six agents without anyone rechecking it.

It reads nothing by itself. It has no browser and no disk: the landing page arrives as pasted copy, the brief travels in and out as text, and the questions go to whoever is calling rather than to the buyer. A caller who answers on the buyer's behalf produces an invented brief with an interview's authority.

## What this is, precisely

A FindAgent **`mcp-tool`** agent. Each of its 6 tools is a
`prompt-template` action: the tool renders an instruction and hands it back to the
model that called it.

Two consequences worth being blunt about, because they decide whether this is useful to you:

- **It calls no model and reaches no network.** A tool call costs nothing and returns
  the same text for the same input, every time. There is no API key, no credential
  slot and no egress.
- **It observes nothing.** It has no access to your repository, your logs, your
  analytics or your devices. Every template is written so that supplying nothing
  produces an honest statement of what is missing rather than a confident-looking
  answer about data nobody provided. If you ask for a report and give it no findings,
  it will tell you the work has not been done — not invent it.

## Tools

| Tool | What it returns | Required input |
|---|---|---|
| `read_landing_page` | Extract the campaign-relevant facts from pasted landing page copy — offer, claims, proof, CTA, ICP signals, friction — separating what the page states from what it only implies. | `page_copy` |
| `ask_next_brief_question` | Produce the single next intake question, with options derived from the landing page rather than from a generic list, and the decision it closes. | `brief_so_far` |
| `record_brief_answer` | Record an intake answer into the brief, mark whether it closed the decision or created an assumption, and reissue the brief in full. | `brief_so_far`, `question`, `answer` |
| `check_brief_conflicts` | Run the pre-lock conflict check — a KPI the page cannot produce, a claim with no support, a funnel layer mismatched to the offer — and name what must be resolved before freezing. | `brief_so_far` |
| `freeze_brief` | Freeze the campaign brief for handoff: the full agreed text, every unanswered question, and every assumption standing in for one, each with what it costs if wrong. | `brief_so_far` |
| `check_stage_gate` | Check whether the artifact the next pipeline stage requires was actually supplied, and refuse the handoff by name when only an identifier arrived. | `stage`, `artifacts_supplied` |

Optional inputs render as empty when omitted. Every template names that case and says
what it could not determine, so an empty slot degrades into a stated gap rather than a
dangling clause.

## Part of a department

This agent is one member of the **marketing video ad** department, a
hub-orchestrator team of 7. The hub is `marketing-campaign-scoper`, which locks the brief every later
stage reads; the other members are
reached through it or called directly as `<alias>__<tool>`.

| Agent | Stage in the pipeline |
|---|---|
| `marketing-campaign-scoper` | 1 — interviews for the brief and freezes it (department hub) |
| `marketing-ad-researcher` | 2 — competitor harvest plan, longevity ranking, customer voice, coverage |
| `marketing-angle-strategist` | 3 — scored angle map with auditable arithmetic |
| `marketing-hook-writer` | 4 — the modular creative bank, built on verbatim customer language |
| `marketing-ad-scripter` | 5 — modules, continuity kits, prompts, assembly map, QA protocol |
| `marketing-production-planner` | 6 — blockers, tracks, cost estimate, shoot briefs, release gates |
| `marketing-ad-tester` | 7 — clip QA, test design, readout, and the feedback loop back to 3, 4 and 5 |

Each member is published independently and works on its own.

## Provenance

`source/scoper.md` is the markdown skill this agent was converted from. The tool templates carry its
instructions, parameterised: anything the original hard-coded to one team's repositories,
file paths or people became an input you supply, and where a template would otherwise
depend on reading something it cannot reach, it asks for that material as an argument
instead.

## Licence and use

Published by Kata Team on FindAgent. Free to connect.
