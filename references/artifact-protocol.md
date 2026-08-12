# Requirement artifact protocol

Read this protocol only when the user explicitly requests PRD generation for a
tracked requirement. The documents are instructions for another AI, so make
every rule observable and every reference unambiguous.

## Generation gate

Use exactly one readiness result: `<95%，继续澄清` or `≥95%，可以生成`.
Do not invent a more precise percentage.
In every user-facing generation decision, write the readiness result as a standalone line
before the one question or the two file paths.

Use `≥95%，可以生成` only when all of these are clear and no remaining unknown
could change the main flow or an expected test result:

- actor and usage situation;
- start point, ordered actions, and end result;
- normal result after each material action;
- who may and may not act;
- limits, exceptions, and failure results;
- observable success and existing behavior that must not regress.

Below the gate, create no PRD or test file. State the one most important unclear
point in everyday language and ask exactly one question. At or above the gate,
generate both `PRD.md` and `测试文档.md` in the confirmed requirement folder and
report both paths.

## PRD contract

Write these sections in order:

1. Document information: version, source Episode ID/path, generation date.
2. Goal and observable success criteria.
3. Actors and usage situations.
4. In scope and out of scope.
5. End-to-end business flow.
6. Requirements.
7. Business rules and permissions.
8. Exceptions and failure results.
9. Existing behavior that must not regress.
10. Non-blocking limitations.

Give every requirement a stable ID in the form `REQ-001`. Use this example as
the required field shape, adapting its business content to the confirmed need:

```markdown
### REQ-001 — Store manager pauses online orders

- Actor: store manager with order-management permission
- Trigger / starting condition: the store is accepting online orders
- Required behavior: after the manager confirms a pause, the store stops
  accepting new online orders while existing orders remain available
- Observable result: the manager sees “Online orders paused” and a customer
  cannot place a new order for that store
- Exceptions or limits: a manager without permission cannot confirm the pause
- Related requirements: REQ-002
```

Write requirements so a test AI can decide pass or fail from an observed
result. Replace vague phrases such as “works normally”, “user friendly”, or
“handle appropriately” with a specific result.

## AI test document contract

Use the same document version as the paired PRD. Write these sections in order:

1. Source PRD path/version, test goal, scope, and exclusions.
2. Required environment, accounts, identities, and data.
3. Coverage table mapping every `REQ-*` to one or more `TC-*`.
4. End-to-end flow tests.
5. Normal, disallowed, boundary, interruption/re-entry, repeated-action,
   permission, and regression tests that apply to this requirement.
6. Issue record format.
7. Execution summary.

Give every test a stable ID in the form `TC-001`. Use this example as the
required field shape, adapting its business content to the confirmed need:

```markdown
### TC-001 — Authorized manager pauses online orders

- Covers: REQ-001
- Priority: high
- Purpose: prove the complete pause flow works and blocks new orders
- Preconditions: the test store is accepting online orders
- Identity and data: authorized store-manager account and one test customer
- Steps:
  1. Sign in as the store manager and confirm “Pause online orders”.
     - Expected: the manager sees “Online orders paused”.
     - Evidence: screenshot containing the store name and paused status.
  2. Sign in as the test customer and try to place a new order for the store.
     - Expected: checkout is unavailable and explains that the store is paused.
     - Evidence: screenshot of the unavailable checkout result.
- Cleanup: resume online orders for the test store
- Result: 未执行
```

Every `REQ-*` must appear in the coverage table. Every test step must pair an
action with an observable expected result. Request only evidence that the
execution AI can legally and practically retrieve.

## Execution results

Use only these results:

- `通过`: every expected result was observed and required evidence was saved.
- `失败`: an actual result differed; record the failed step, expected result,
  actual result, and evidence.
- `暂时无法测试`: environment, account, identity, data, or another prerequisite
  was missing; record exactly what is missing.
- `未执行`: the test was not run in this execution.

The execution AI 不得自行补全 missing conditions, infer success from the
absence of an error, or mark later steps passed after an earlier blocking step.
For a discovered problem, record a stable issue ID, related `TC-*` and `REQ-*`,
impact, reproducible steps, expected result, actual result, and evidence.

Never claim the product has no bugs. Say “本次覆盖范围内未发现问题” only when
every agreed test in the current scope is `通过` and no test is `失败`, `暂时无法测试`, or `未执行`.
Otherwise, do not use that summary: report every result status and the specific gap
for each non-passing test, including the failed observation, missing prerequisite,
or reason it was not run.

## Common mistakes

- Generating documents without an explicit PRD request.
- Generating `PRD.md` without the paired `测试文档.md`.
- Creating partial files below the readiness gate.
- Leaving a `REQ-*` without a `TC-*` mapping.
- Writing steps without observable expected results or evidence.
- Guessing missing test conditions or calling an unexecuted test passed.
