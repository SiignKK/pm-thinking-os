---
name: pm-thinking-os
description: "Use when the user invokes $pm-thinking-os, starts with “新需求：”, asks to 过一遍需求, requests a PRD for a tracked requirement, starts “复盘：” for an existing decision, or asks to search prior PM decisions in the current Git project. Do not trigger for ordinary chat without requirement or decision-tracking intent."
---

# PM Thinking OS

Guide one real requirement at a time from an observed signal to a confirmed,
locally stored Decision Episode. Preserve uncertainty and the original decision
context; do not optimize for producing an answer quickly.

## Separate chat from artifacts

- Chat assumes an 不懂技术的业务人员: use 日常语言 and 一次只问一件事.
- Generated documents' 默认读者是 AI: use 固定结构, 稳定编号, explicit
  conditions, steps, results, evidence, and links between requirements/tests.

Simple chat must not produce vague artifacts. Translate confirmed business
answers into precise AI-readable contracts without inventing missing rules.

## Locate the project

Before New episode, Generate requirement artifacts, Outcome review, or History
search, run the read-only command below from the current working directory:

```text
git rev-parse --show-toplevel
```

Use the single absolute path returned with exit code zero as `<root>`. Reject
empty output, multiple output paths, a relative path, or a path that is not an
existing directory. If resolution fails, stop and ask the user to open or enter
the target Git project. Do not fall back to the Skill directory, a global Codex
directory, or the PM OS source repository.

Ignore ambient Git discovery overrides when resolving the project. Do not read
secret-bearing environment variables. Do not modify the caller's environment.

Store each active or confirmed requirement under
`<root>/需求/<requirement-name>/`. Keep its New-episode draft, Decision Episode,
PRD, research, and later requirement-specific artifacts in that one direct
child folder. Search history only in direct `决策记录.md` files under
`<root>/需求`.

Never run `git add`, `git commit`, or `git push`.

Before a new requirement or review:

1. Read [decision-protocol.md](references/decision-protocol.md).
2. Read [episode-schema.md](references/episode-schema.md) before saving.
3. Read [storage-protocol.md](references/storage-protocol.md) before any search
   or write.
4. Read [review-protocol.md](references/review-protocol.md) for `复盘：`.
5. Read [draft-protocol.md](references/draft-protocol.md) before any New-episode
   draft read or write.

## Route the request

- For an explicit PRD request for a tracked requirement, run **Generate
  requirement artifacts**.
- For `新需求：...`, `$pm-thinking-os`, or “过一遍需求”, run **New episode**.
- For `复盘：<ID or title>`, run **Outcome review**.
- For a request to find past decisions, run **History search**.
- Otherwise do not take over the conversation.

If a PRD request arrives with a new or incomplete requirement, keep it as a
pending explicit PRD request while running **New episode**. Before every next
question below the gate, write `<95%，继续澄清` as a standalone line and
create no `决策记录.md`, `PRD.md`, or `测试文档.md`; the New-episode draft is
still maintained. After the Decision Card is confirmed and readiness is
`≥95%，可以生成`, run **Generate requirement artifacts** without asking the
user to repeat the request. Confirmation alone never triggers artifacts when
there is no pending request.

## Generate requirement artifacts

1. Resolve the tracked requirement to its confirmed direct folder under
   `<root>/需求`. If it cannot be identified there, stop and ask the user to
   identify the requirement; never write artifacts to another folder.
2. Read the requirement folder's `决策记录.md` and relevant reliable
   requirement materials before judging readiness. Combine those sources with
   the current conversation. Do not ask again for answers already present in
   those sources. Keep the Decision Episode ID and file path available for the
   PRD's source Episode ID/path.
3. Read [artifact-protocol.md](references/artifact-protocol.md).
4. Generate nothing unless the user explicitly asks for PRD output.
5. Judge readiness as exactly `<95%，继续澄清` or `≥95%，可以生成` using
   actor/scenario, start-to-finish flow, normal results, permissions, limits,
   exceptions/failures, success evidence, and regression boundaries.
6. If `决策记录.md` is missing, or the combined sources still leave a gate
   item unclear, write `<95%，继续澄清`, state the most important unclear
   point in everyday language, and ask exactly one simple question. Do not
   create partial PRD or test files.
7. At or above 95%, write both `PRD.md` and `测试文档.md` in the existing
   requirement folder. Refuse to overwrite either existing file unless the
   user explicitly approves updating that exact file. Report both paths.

## New episode

1. Search relevant history under `<root>/需求` using the procedure in
   [storage-protocol.md](references/storage-protocol.md). Summarize only
   genuinely relevant matches. State why each is similar, what is different,
   and whether it is evidence or merely a reminder.
2. As soon as the requirement input yields a readable temporary name, safely
   resolve a new or resumable direct folder and `需求草稿.md` using
   [draft-protocol.md](references/draft-protocol.md). Do not wait for the
   Decision Card. At this stage never create `决策记录.md`, `PRD.md`, or
   `测试文档.md`.
3. Before every next question after a readable temporary name exists, run the
   draft loop in this exact order: read the current draft if present (first
   named turn has none); merge the current answer into in-memory state; compress
   and safely create or write back only if state changed; then derive the next
   highest-information-value question. The first named turn creates one
   populated draft, never an empty draft followed by an update.
   Direct user facts and lossless shorter paraphrases belong in `已明确`;
   agent-added meaning, ambiguity, and inference belong in `待验证`. A later
   correction replaces the old conclusion rather than preserving history.
   Ordinary draft updates are silent and never add a confirmation question.
4. Before every next question in a New episode, render this paired block:

   ```text
   当前思考方向
   - <one short sentence naming the current business decision or unknown being clarified>

   当前把握程度
   - 需求理解：<很低|低|中|高|很高> <初始|↑|→|↓> — <one short reason>
   - 决策充分：<很低|低|中|高|很高> <初始|↑|→|↓> — <one short reason>
   ```

   Treat `当前思考方向` and `当前把握程度` as one required unit. Whenever a
   New episode reply shows the confidence block, show the direction block
   immediately before it, including when presenting the final Decision Card.
   If a reply only explains the user's previous question and does not continue
   the New episode or show confidence, render neither block.

   Write the direction as exactly one short bullet sentence in everyday
   language. Name the business decision or unknown currently being clarified,
   keep it tied to the original business need, and update it as the inquiry
   advances. Do not merely repeat the user's latest answer and do not expose
   internal reasoning. Before output, self-check that the direction still
   advances the original business need; if not, revise it.

   Treat the levels as one strict ordered scale: `很低 < 低 < 中 < 高 < 很高`.
   On the first display use `初始`. On every later display, compute each trend
   only by comparing the current level with that dimension's immediately
   preceding level: a higher level requires `↑`, the same level requires
   `→`, and a lower level requires `↓`. The trend is never a separate
   judgment. Before output, self-check each dimension's level against its
   preceding level; if they disagree, correct the trend symbol.

   Then ask exactly one highest-information-value question. That question must
   contain one information request; do not bundle subquestions or request
   multiple independent facts in one sentence.
5. Convert feedback or symptoms into the decision that must be made. Do not
   jump directly to a feature or solution.
6. Maintain a compact working classification: direct evidence, second-hand
   report, assumption, unknown, and organizational constraint.
7. Raise one material challenge by default. Stop when another challenge adds no
   new information.
8. Check whether the result can be observed legally and practically. If not,
   allow a `parked` decision record, set `learning_eligible` to `false`,
   and never call it a completed learning loop.
9. Confirm Decision Owner, evidence, unknowns, real options, decision,
   assumptions, low-confidence prediction if needed, falsification signal, and
   review trigger. Add a review date only when the user has a meaningful
   calendar commitment. For an unscheduled requirement, use an event trigger
   and do not invent a date.
10. Render the complete Decision Card from the schema. The final card must show
   the paired direction and latest confidence snapshot even when it asks no next
   question. Save only after the user explicitly confirms it. Store each latest
   confidence level and reason, but never the direction, a trend, or per-turn
   confidence history.
11. Follow the create procedure in
   [storage-protocol.md](references/storage-protocol.md) to render the confirmed
   card directly as `<root>/需求/<requirement-name>/决策记录.md`. Do not create
   a temporary payload or run an installer. Only after creation and read-back
   verification succeed, report the Episode ID, paths, and transition to the
   formal record; retain the draft but stop automatically reading or writing
   it. If creation fails before a formal file exists, do not claim a transition
   and continue the draft loop. If a created file fails read-back verification,
   report a content conflict and stop automatic operations until it is resolved.
   Save every later artifact for that requirement in the same returned folder.

Never silently convert “I cannot predict” into certainty. A weak baseline
prediction is acceptable only when labeled low confidence with its limitations.

## Outcome review

1. Search `<root>/需求` for the Episode ID or title using
   [storage-protocol.md](references/storage-protocol.md). If no record matches,
   stop. If multiple records match, ask for exactly one identifying detail.
2. Show the original Decision, Assumptions, Prediction, and Falsification Signal
   before asking about the result.
3. Follow [review-protocol.md](references/review-protocol.md), one question at a
   time.
4. Show the complete proposed review and obtain explicit confirmation.
5. Append the confirmed review using the append-only procedure in
   [storage-protocol.md](references/storage-protocol.md). The resulting file
   must begin byte-for-byte with its original content; never alter the original
   Decision, Prediction, or previous reviews.

## History search

Search only `<root>/需求` using
[storage-protocol.md](references/storage-protocol.md). For each useful result,
identify:

- why it is relevant;
- the earlier decision and observed outcome;
- a supported or rejected assumption;
- a material context difference;
- whether the history should change the current decision.

Similarity alone is not proof. Surface staleness and selection bias.

## Due reviews

When the user asks which reviews are due, inspect only direct requirement
records under `<root>/需求/*/决策记录.md`. Include a record only when:

- its status is not `closed`;
- `review_at` is a real calendar date rather than empty; and
- that date is on or before the user-requested as-of date, or today when the
  user did not supply one.

Event-only triggers are not calendar-due. Report their trigger separately only
when useful; do not invent a due date.

## Maintainer validation

For a repair or distribution check, read
[validation-scenarios.md](references/validation-scenarios.md) and exercise only
the relevant scenarios in a disposable temporary Git repository with synthetic
input. Keep the source Skill read-only, make no network request, use no real
credential or private data, and make no global or system change. Normal Skill
use does not run this validation entry.

Do not write personal data, credentials, access tokens, customer identifiers,
or real company-sensitive examples. Do not rewrite a Decision Episode's
original decision or prediction.
