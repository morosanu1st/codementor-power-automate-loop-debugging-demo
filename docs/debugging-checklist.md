# Checklist: "this step only runs once inside my loop"

1. **Run history.** Open the failed or odd run, expand `Apply to each` and use
   the iteration arrows. Note the first iteration where the step is skipped,
   failed, or missing.
2. **Placement.** Is the action really *inside* the loop container? An action
   placed right after the loop runs exactly once.
3. **References.** Inside the loop, use `items('Apply_to_each')?['Field']` (or
   the dynamic content from the loop). Anything like `...?['value'][0]` or
   `first(...)` always points at the first record.
4. **Run after.** If an earlier step in the same iteration fails, the next
   steps with the default "is successful" setting are *Skipped*. Fix the
   failing step, or add a branch that runs after "has failed" to log it.
5. **Concurrency.** With parallel iterations, `Set variable` / `Append to`
   race each other. Set *Concurrency control* to 1, or use `Compose` per
   iteration instead of a global variable.
6. **Conditions and Do until.** A `Condition` or `Do until` whose expression
   is already true after the first pass will stop doing the work. Check
   the evaluated inputs in run history.
7. **Data source limits.** If later records never arrive, check pagination
   or the row limit on the list action (by default many connectors return
   only the first page).
8. **Re-test** with 3 to 5 known rows and confirm each iteration writes
   its own record.
