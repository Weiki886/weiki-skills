# Write the Implementation Plan

Break the clarified request into small tasks that can be **verified and committed independently**. Write for an executor with no repository context and questionable taste, including your future self.

## Plan Structure

```markdown
# [Feature] Implementation Plan

**Goal:** <One sentence describing what to build>
**Architecture:** <2–3 sentences describing the approach>
**Stack:** <Key technologies and libraries>
**Requirements:** <Link to the clarified requirement/design/Issue; read it during execution>

## Global Constraints
<Hard constraints shared by the whole project: minimum versions, dependency limits,
naming and copy rules, platform requirements. One verbatim line per constraint.>

---

### Task N: [Component]

**Files**
- Add: `exact/path/to/file.py`
- Change: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Interfaces**
- Consumes: <type/signature produced by an earlier task>
- Produces: <exact names and signatures, including parameters and return types>

- [ ] **Step 1: Write a failing test** (include real test code)
- [ ] **Step 2: Run and confirm failure** (command + expected reason)
- [ ] **Step 3: Write the minimum implementation** (include real code)
- [ ] **Step 4: Run and confirm pass** (command + expected result)
- [ ] **Step 5: Commit** (`git add` + Conventional Commit message)
```

## Split Tasks

- A task is the smallest unit with its own test cycle that is worth a reviewer approving independently.
- Fold setup, scaffolding, and docs into the task that needs them; do not make them independent chores.
- Split at a boundary where a reviewer might reject A but approve B.
- Each step should be one action of roughly 2–5 minutes: write failing test, run failure, implement, run pass, commit.

## Group Files

- Give each file one clear responsibility; focused files are easier to change and review.
- Group things that change together by responsibility, not by technical layer.
- Follow the existing repository structure. Do not reorganize just to make it look cleaner. Plan a split only when the file you are already changing is genuinely bloated.

## No Placeholders

Every step needs the information its executor will need. These make the plan fail:

| Placeholder | Why it fails |
| --- | --- |
| TODO/TBD/“implement later” | The executor cannot act on it. |
| “Handle errors/validation/edge cases appropriately” | No concrete rule is given. |
| “Add tests for the above” without test code | No test has been specified. |
| “Similar to Task N” | Tasks may be read out of order; repeat the actual code. |
| Describe only what to do without how | Code steps need concrete code. |

Define every type/function/method referenced somewhere in this plan; do not refer to symbols that do not exist in it.

## Self-Review the Plan

After drafting, compare it to the requirement with fresh eyes (do this yourself, not via a subagent):

1. **Specification coverage:** Map every requirement/acceptance item to a task; list and fill gaps.
2. **Placeholder scan:** Search TODO, “appropriately,” “similar to Task N,” and other prohibited placeholders; remove each one.
3. **Type alignment:** Confirm later signatures and field names match earlier definitions. A producer named `clearFullLayers()` cannot silently become a consumer of `clearLayers()`.
4. **Interface continuity:** Confirm each consumed interface is produced by an earlier task; there are no dangling dependencies.

Fix problems in place and repeat until all four checks pass; no separate review cycle is needed.

## Execution Handoff

Once the plan is saved, offer one of two execution paths rather than silently starting:

1. **Delegate each task to a clean subagent with task-level review**, suitable for a larger implementation.
2. **Execute sequentially in this session**, following the checklist with a checkpoint after each task.

Both paths must follow the red-green gate in `references/tdd.md`.

## Execution Discipline

### Ledger

Memory can disappear; the ledger remains. During execution, keep a progress ledger in a gitignored temporary file within the repository:

```text
# Execution ledger — plan: <plan name>

- Task 1: complete — `pytest tests/x.py` — 12 passed
- Task 2: in progress
- Task 2: Ruling: plan called the signature clearLayers(), existing code uses clearFullLayers() — keep existing name — consequence if wrong: Task 4 consumer must be renamed
```

- Record each task with the command and actual result; after interruption/context compaction, recover from the ledger and `git log`, not memory.
- Update it alongside normal work/commits rather than repeatedly reopening it as a separate chore.

### Plan Error or Code Error?

When actual output conflicts with the plan, classify it first:

- **Code error:** follow `debugging.md` and find the root cause; do not alter behavior to force the expected output.
- **Plan error** (contradiction, conflict with requirements/existing interface, or impossible command): choose the smallest change that satisfies the requirement and record `Ruling: <discovery> — <decision and reason> — <cost if wrong>`. Later tasks must read the ruling before using that interface.

A departure without a Ruling is an undocumented decision and hidden rework.

### Continue and Stop Conditions

Once sequential execution is selected, complete the plan continuously within the authorized scope; do not ask “continue?” after every task. Pause when necessary authority or information is missing for:

1. An irreversible or destructive operation.
2. A security-sensitive action involving credentials, permissions, or external exposure.
3. An out-of-workspace side effect such as merging, pushing a shared branch, or releasing.
4. A plan so broken that every path requires guessing; return with ruled-out assumptions and evidence.

Do not ask again for clearly authorized work with a clear target. Neither a plan nor a Ruling grants authority. Report other blockers honestly rather than forcing them into these categories. If the requirement or plan resolves an ambiguity, record a Ruling and continue; it is not a stop condition.
