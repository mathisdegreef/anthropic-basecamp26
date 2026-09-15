# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A Larkspur disruption-care chat agent on the Claude Messages API: nine given tools plus next_available_day, which we added, driven by one tool loop.
Does: Looks up a stranded customer's booking, checks flight status and policy, and answers "when can I actually fly?" with the earliest open seat, e.g. Austin for K7PQ2M.
Number: 10,330 input tokens per resolved contact on our probe (K7PQ2M, 3 tool calls, n=1), of which 2,419 tokens are tool schemas sent on every turn.
Guardrail: The loop is capped at 8 model turns, and next_available_day is told to take route, date and cabin from the booking, never to guess them.
Next: Move next_available_day behind the Larkspur MCP server (2.2), then prove it with eval cases.
Still broken: Nothing watches tone: an abusive message (R8KD3F) gets the same calm resolution as any other.
Lever: cost

## Priya asked

Costs:
Wrong:
Runs it:
Left out:

## Before 2.2: schema tokens on every turn

2,419 tokens per turn, 10 tools, counted on the wire with `python3 run.py --tool-tax`
before `next_available_day` moved behind the MCP server. It matches what gate 2.1 banked
in `.workshop/build2_tokens.json`.

| tool | if dropped | owner |
|---|---:|---|
| lookup_booking | 191 | given |
| get_flight_status | 190 | given |
| search_alternatives | 113 | given |
| check_policy | 387 | given |
| hold_seat | 113 | given |
| confirm_rebooking | 171 | given |
| issue_voucher | 242 | given |
| escalate_to_human | 195 | given |
| send_confirmation | 104 | given |
| next_available_day | 359 | ours, local (agent.py) |
| **whole list** | **2,419** | 10 tools |

The per-tool column is what dropping that one tool saves with the rest still listed, so it
does not sum to the whole-list figure.
