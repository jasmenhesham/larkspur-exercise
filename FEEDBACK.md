# Client review: Larkspur disruption-care agent

**To:** Room 20  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-17 11:21

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

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py run_agent(): your diff moves answer = text_of(response) out of the while loop to after it.**

You also changed the assistant append to response.content, so thinking blocks go back to the API intact. The old version assigned answer inside the loop and could return text from a mid-loop turn. Yours returns only the final response text. If the loop exits at MAX_TOOL_CALLS = 8 while stop_reason is still tool_use, the returned answer is whatever text that last tool-calling turn held, possibly an empty string.

Run python3 run.py --all --trace and paste the totals footer, including any booking that reaches 8 turns.

**2. readout-trace.json shows 3 API turns and 2 tool calls, lookup_booking then get_flight_status, and nothing after.**

The run used 8649 tokens in and 688 out. No search_alternatives, check_policy or hold_seat call appears in it. Your diff touched the search_alternatives description and the get_flight_status date format, and the committed trace contains no call to the first of those.

Run python3 run.py --all --trace and paste the per-booking tool sequence for every booking shape in the run.

**3. readout-trace.json records 0 cache tokens read, 0 written, and agent.py contains no cache_control anywhere.**

Every turn resends runtime_preamble() + SYSTEM_PROMPT + TONE_ADDENDUM plus all nine tool schemas. That is 8649 input tokens for a 3-turn run that touched two tools. A larger model changes the price per one of those tokens and leaves the 0 cache hits as they are.

Run python3 run.py --tool-tax and paste its output next to the 8649 input-token figure from readout-trace.json.

**4. There is no evals/cases.json in this repository, and readout.html banks gates 1.2, 1.3 and 1.4 only.**

Nothing here tests the behaviour you changed in run_agent() or the new search_alternatives text. Gates 2.1, 2.2, 3.1 and 4.1 are not banked. No bench.py before and after pair is referenced anywhere, so no figure here compares one configuration to another.

Run python3 bench.py --label before --stage 1 --runs 3, then the same with --label after, and paste the python3 bench.py --compare before after output.

**5. agent.py search_alternatives takes only pnr, and your new description says it returns 'flight number, date, and timing information'.**

The input_schema has no field for date, origin or time window, so the description is the only guidance the model gets about what to pass. The text lists return fields and says nothing about when to call it versus check_policy or hold_seat. TONE_ADDENDUM is 0 characters and EXTRA_TOOLS is 0, so nothing else steers the tool order.

Run python3 verify.py 1.3 and paste the output, then python3 run.py --show-tools and paste the search_alternatives entry.

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

- `agent.py (226 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
