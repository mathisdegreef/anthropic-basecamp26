# Overnight review: Larkspur disruption-care agent

**To:** mathisdegreef__anthropic-basecamp26  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-09-15 12:59

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Overnight, Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. PITCH.md's cost figure comes from a single probe run, n=1, on one PNR.**

The Number line states '10,330 input tokens per resolved contact on our probe (K7PQ2M, 3 tool calls, n=1)'. There is no readout-trace.json in the repository and no evals/cases.json, so this figure has never been reproduced or averaged across booking shapes. A single sample cannot separate a fixed schema tax from a variable-by-scenario cost.

Run python3 run.py --all --trace and compare the per-PNR token totals against the 10,330 figure in PITCH.md.

**2. next_available_day's tool description is 494 characters against search_alternatives at 159.**

The added tool's description spells out when to call it, what it returns versus search_alternatives, and that origin, dest, date and cabin must come from lookup_booking, 'never guessed.' search_alternatives, the given tool it depends on for the actual bookable options, still reads 'Search for alternative Larkspur flights available for rebooking after a disruption.' The schema-tax table in PITCH.md prices next_available_day at 359 tokens per turn against search_alternatives at 113.

Run python3 run.py --tool-tax and confirm whether the 359 versus 113 token split still holds after the MCP move.

**3. PITCH.md names tone as the unmeasured gap, but TONE_ADDENDUM is 92 characters and untested against it.**

The Still broken line reads 'Nothing watches tone: an abusive message (R8KD3F) gets the same calm resolution as any other.' TONE_ADDENDUM was changed from empty to 'Be concise and direct. No filler phrases. Get to the point immediately. Use short sentences.', which governs brevity, not how the agent responds to an abusive customer. No eval case or trace in the repository runs R8KD3F through the agent to check what it actually does.

Run python3 run.py R8KD3F --trace and paste the resulting transcript.

**4. The diff moves response.content into the message history but run_agent still returns text_of(response) only after the loop ends.**

The change from messages.append({'role':'assistant','content':text_of(response)}) to messages.append({'role':'assistant','content':response.content}) preserves tool_use blocks in history, which the prior version dropped. The final return also moved from the stale answer variable to text_of(response), computed after the loop exits. Neither change touches MAX_TOOL_CALLS, still 8, or what happens to a conversation that hits that cap mid-escalation.

Run python3 verify.py 1.2 to confirm the message-history fix passes the workshop's own check for that step.

**5. EXTRA_TOOLS holds one tool and LOCAL_TOOLS wires one executor; the other nine tools route through support.execute_tool with no case in this repo exercising.**

tool_list() returns build_tools() plus EXTRA_TOOLS, and next_available_day is the only entry in either. The confirmation_token gate on confirm_rebooking and the queue/summary_for_human fields on escalate_to_human are schema constraints nobody has driven a transcript through, since no eval cases exist in the repository and no trace file survived the push.

Run python3 eval_harness.py and paste the totals for how many cases exercise confirm_rebooking and escalate_to_human.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (251 lines)`
- `PITCH.md`
- `TEAM.md (unchanged template)`

Reviewer: `claude-sonnet-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
