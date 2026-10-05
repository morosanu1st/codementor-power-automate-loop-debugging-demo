# Power Automate loop debugging demo

A small, hands-on walkthrough of the most common reason a Power Automate
data-entry flow runs one step **only once** even though the loop keeps going,
and how to find and fix it in a live 1:1 session.

## What's inside

| File | What it shows |
| --- | --- |
| `flows/buggy-flow.json` | A simplified flow definition (Logic Apps / Power Automate schema) with three classic loop bugs |
| `flows/fixed-flow.json` | The same flow with each bug fixed |
| `docs/debugging-checklist.md` | The step-by-step checklist I use to debug a misbehaving loop in run history |

## The bugs reproduced in `buggy-flow.json`

1. **Action sits after the loop instead of inside it.** `Add_row_to_table`
   is a sibling of `Apply_to_each`, so it runs once, after all iterations.
2. **Hard-coded first item.** The row uses
   `body('List_rows')?['value'][0]` instead of `items('Apply_to_each')`,
   so every iteration writes the same record.
3. **Variable + parallel loop.** `Set_variable` runs inside an
   `Apply to each` with concurrency 20, so iterations overwrite each other
   and the value you see is just whichever iteration finished last.

There's a fourth case in the checklist: an action with the default
**Run after = is successful** gets *skipped* in every iteration after one
earlier step fails, so it looks as if it ran only once.

## How I'd help in a session

1. Open your flow's **run history** together and expand the loop to step
   through iterations 1, 2, 3... to see exactly where the step stops running.
2. Check where the action sits, what it references, concurrency, and
   *Configure run after*.
3. Fix it live, re-run with test data, and confirm every iteration writes
   its row.
4. Leave you with a short checklist so you can debug the next flow yourself.

Most loop problems like this are solved within one session (under an hour).
