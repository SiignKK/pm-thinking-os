# Decision protocol

Use this protocol for a new requirement. Keep it conversational: one question
per turn when needed, not a visible questionnaire or an endless question loop.
For requirement design and its continuation after a formal record, also read
[clarification-protocol.md](clarification-protocol.md); it selects whether to
ask, offer documents, deliver, or wait.

## 1. Reframe the input

First label the input:

- **Direct evidence**: an observable event or source the user can locate.
- **Second-hand report**: another person summarized what users allegedly said.
- **Assumption**: a proposition accepted temporarily without enough evidence.
- **Unknown**: information that could change the decision.
- **Organizational constraint**: authority, access, legal, staffing, or timing
  limits that restrict possible actions.

An emotion is still evidence of an experience, but it is not proof of its
suspected cause. A report such as “users cannot find support” is not yet the
decision; reframe it as “whether to change the support entry now, investigate
first, or observe.”

## 2. Ask for the decision

Understand the background and desired improvement first. Clarify the following
only as needed for the current business choice or a requested Decision Card:

- what choice must be made;
- what outcome should change;
- who owns the decision;
- when the choice is needed.

Do not assume the product manager is the Decision Owner.

## 3. Find the pivotal unknown

Ask the one question most likely to change the choice. Prefer source,
frequency, scenario, impact, existing behavior data, or authority over cosmetic
details. First identify a blocking unknown under
[clarification-protocol.md](clarification-protocol.md). If there is none, move
to flow readback/document choice or the requested output, rather than searching
for a related detail to ask. Missing background metrics do not automatically
block a clear personal-use requirement.

## 4. Maintain only the current requirement state

During New episode, follow [draft-protocol.md](draft-protocol.md). Treat a
user's direct statement, or a lossless shorter paraphrase that adds no meaning,
as explicit state. Keep agent-added meaning, ambiguity, and inference awaiting
verification. Only pivotal unknowns require the one-question flow; a local
design suggestion can remain explicitly awaiting review without becoming a
prerequisite question or a confirmed requirement.
When later evidence corrects an item, replace it; do not retain the rejected
conclusion or conversation history in working state.

## 5. Judge confidence before the next question

In a New episode, judge these two dimensions independently from all current
information before every next question. Improvement in one dimension must not
move the other unless its own evidence changed.

### 需求理解

| 等级 | 判断条件 |
|---|---|
| 很低 | 只有功能名称或解决方案，不知道真实问题、用户和场景 |
| 低 | 知道部分背景，但问题对象或核心含义仍然模糊 |
| 中 | 问题、对象和主要场景清楚，但边界或例外还不完整 |
| 高 | 问题、场景、范围、规则和主要例外基本明确，可以整理成完整需求 |
| 很高 | 能写出没有关键歧义的业务规则和验收标准，只剩研发实现细节 |

### 决策充分

| 等级 | 判断条件 |
|---|---|
| 很低 | 没有可追溯依据，也不知道必须做出什么选择 |
| 低 | 有一些现象或转述，但关键事实、负责人或影响仍不清楚 |
| 中 | 证据有限，但方案、主要风险和负责人明确，可支持用户明确选择的范围有限、可撤销或风险可控的阶段性决定；不代表完整需求可以收尾 |
| 高 | 证据、备选方案、风险、负责人和验证方法比较明确，当前决定有充分依据 |
| 很高 | 有多种相互印证的证据，结果可以观察，主要假设能够被后续结果证明或推翻 |

The strict level order is `很低 < 低 < 中 < 高 < 很高`. Use “很高” sparingly.
需求理解 reaching “很高” does not imply that 决策充分 also reaches “很高”.

Confidence describes readiness for the actual decision being discussed. During
requirement design, future product performance need not have been measured to
reach “高”: assess the evidence available now, real alternatives, accepted
risks, owner, and executable validation plan. Future tests remain hypotheses,
not proven outcomes. A test plan can resolve observability but cannot resolve
missing business rules, option tradeoffs, or decision authority. Never raise a
level just to unlock a card, and never use a displayed high level to bypass a
substantive blocker.

After each user reply, rejudge both levels from all current information rather
than accumulating a mechanical score:

- Rise when a key unknown is resolved, a boundary becomes clear, or direct
  evidence is added.
- Hold when an answer is useful but does not change the pivotal judgment.
- Fall when information conflicts, a new material risk appears, or earlier
  evidence proves unreliable.
- A material change may cross more than one level, but the reason must say why.
- On the first turn, judge the initial input truthfully instead of starting from
  a fixed level; render “初始” because there is no prior level to compare.
- On every later turn, compute each dimension's trend only from its current
  level and its immediately preceding level. A higher current level must use
  `↑`; an unchanged level must use `→`; a lower current level must use `↓`.
  This comparison is the only source of the trend; never judge it separately.
- Before display, self-check each dimension against the strict order. If its
  level change and symbol disagree, correct the trend symbol before rendering.
- Give each dimension one short phrase explaining its current level or change.

Do not assess the user's honesty, use percentages or hidden scores, combine the
dimensions into an overall level, or raise a level merely because another turn
has passed. This display applies only to New episodes, not Outcome review or
History search.

When the Decision Card is confirmed, store only the latest level and reason for
each dimension, using `very_low`, `low`, `medium`, `high`, or `very_high`.
Never store the trend or per-turn confidence history.

## 6. Prepare for later documents

Assume the user is an 不懂技术的业务人员. Use 日常语言 and 一次只问一件事.
Across the conversation, learn who uses the feature, where the flow starts,
what should happen step by step, who cannot act, limits/exceptions/failures,
how success is observed, and what existing behavior must not change.

Prefer questions such as “谁会使用这个功能？” and “做完以后应该看到什么？”
Do not ask the user to explain technical implementation. Do not repeat facts
already supported by their answers or reliable source material.
Use the progression protocol to distinguish main-flow blockers from reversible
design details and new capabilities. A high confidence display does not itself
end clarification, nor does an unasked local detail require another turn.

## 7. Challenge once

Present the strongest material challenge, not a catalogue. Explicitly check
selection bias. Example: measuring “cannot find support” only through support
complaints misses people who never reached support.

Stop challenging when the risk is understood, no new information appears, or a
small experiment is more informative.

## 8. Check observability

A learning-eligible Episode needs a legal and executable way to observe an
outcome in a reasonable period. Record:

- observation source;
- review date or event trigger;
- expected result before the outcome;
- known blind spots.

If no source exists, offer parking as a choice. Only after the user explicitly
chooses it and its limited-decision gate passes, preserve the decision as
`parked` with `learning_eligible=false`. Do not invent a survey, interview,
log, or future integration that has no owner and date.

## 9. Decide and confirm

### Optional card choice

Decision Cards are optional. When card recording is the selected next action,
substantive business clarification is complete, and the next step would be
card preparation, explain its purpose in
one or two plain-language sentences and ask exactly one choice question before
rendering any card. For example:

> 决策卡用来记录这次选择、理由、风险和复核条件，方便以后回顾。它是可选的，跳过也能继续整理需求或生成 PRD。
>
> 你需要生成决策卡吗？

For requirement-design conversations with no explicit card request, use the
flow readback/document choice first. If the user requests PRD/test documents,
the existing optional-card choice can then be resolved before generation.
Declining all documents is not a reason to offer a card or collect its fields.

Use the paired direction/confidence block for this New-episode question. The
offer is not the card itself: do not append a full card, ask another independent
question, or assume consent in the same reply. Continue clarifying substantive
business blockers before offering this step; card consent does not establish
readiness.

Track the current user choice as unknown, requested, or skipped, using the
conversation and draft's explicit state. A direct request to generate a card
counts as requested: briefly explain its purpose if not already explained,
then apply the formal-card gate without asking the same choice again. Choosing
generation does not confirm the card's content or authorize formal saving.

If the user declines, acknowledge the skip briefly and bypass card generation,
card confirmation, and creation of `决策记录.md`. Retain their choice as a
short explicit fact under the draft's `边界与约束`, such as
`决策卡：用户选择跳过，不生成决策记录。` Do not collect fields solely to fill
the card schema, invent an Episode ID, or substitute a differently named card
or formal record. Continue unresolved business clarification or a pending
explicit document request. If nothing else was requested and the requirement
is clear, finish with a brief status rather than automatically creating PRD or
test documents or asking the card question again.

Skipping does not declare missing business rules resolved and does not change
the PRD generation gate. A skipped card leaves the draft as current requirement
state, eligible as a PRD source under [artifact-protocol.md](artifact-protocol.md)
but excluded from formal decision history. Preserve any existing formal record.
Do not offer again in later turns or after context compression unless the user
explicitly changes their choice; replace the stored choice rather than keeping
a preference history.

### Formal Decision Card gate

A formal Decision Card is a complete proposal offered for confirmation and
later saving as `决策记录.md`. Before presenting it, reread the current state
and verify all conditions below for the user's actual requested scope:

1. The problem, users, scenario, desired outcome, and scope are explicit. For
   requirement design, the main flow, material business rules, permissions,
   limits, important exceptions/failures, and acceptance results are clear.
2. No unresolved pivotal business-rule or current option-choice question could
   materially change that agreed scope, a main-flow action or required result, content
   retention/overwrite rules, permissions, failure behavior, acceptance result,
   or chosen option. Such an item is a blocking unknown,
   even when many other facts are known. Do not relabel it “implementation
   detail”, “minor detail”, or an accepted risk without resolving the business
   choice with the user. Use the clarification protocol's materiality test;
   local reviewable suggestions and unrequested extensions do not fail this gate.
3. The user has explicitly identified the Decision Owner. Being the initiator,
   product manager, first user, or test participant does not establish ownership.
4. At least two real options and their important tradeoffs have been discussed,
   and the user's selected option is explicit. An agent recommendation or
   options first introduced inside the card are not a confirmed choice.
5. Evidence and its limits, assumptions, accepted material risks, prediction,
   falsification signal, and a practical review date/event are explicit. Future
   effectiveness or engineering tests may remain non-blocking hypotheses when
   their risk and validation/exit plan are understood and accepted. If their
   uncertainty still changes whether to proceed, keep clarifying or let the
   user choose a limited experiment instead.
6. Rejudge confidence from those facts. For completing requirement design,
   both 需求理解 and 决策充分 must be at least “高”; “很高” is not required.
   Passing the level threshold alone never substitutes for conditions 1–5.

**STOP — gate not passed:** keep the requirement in the draft stage. State the
one most important unresolved point in everyday language, render the paired
direction/confidence block, and ask exactly one highest-information-value
question. Do not render a formal card, ask whether it can be saved, or imply
the requirement is complete. “继续下一步”, a long conversation, confirmed
function lists, and a future test plan are not completion instructions.

If the user asks for an early summary, label it `阶段性汇总（需求未完成）`;
it is not a formal card and does not invite formal saving. Do not independently
replace the user's full requirement-design goal with “confirm first-version
direction” to pass the gate.

### Explicit limited or parked decisions

Lower confidence can support a different, bounded decision only when the user
explicitly chooses its scope and action: for example, run a small validation
experiment, take no action, or park the requirement. Verify conditions 1–5
within that limited scope; unresolved rules of the full feature must not affect
the safety or meaning of the limited action. Do not infer this choice from
“continue”, approval of a summary, or an agent's own suggestion.

Label the card `阶段性决定` or `暂缓决定`, state that the original requirement
remains incomplete, and preserve its blockers for later work. Keep confidence
truthful rather than raising it; an unobservable parked decision remains
`learning_eligible=false`. A limited record never authorizes a full-feature PRD
or marks the original requirement design complete; its separate PRD gate still
applies.

After the user chooses generation and the applicable gate passes, present the
complete card, retaining
non-blocking unknowns and accepted risks, and ask for explicit confirmation.
Recheck the gate before saving if the state or scope changed, or an earlier
card was premature. Confirmation of a premature card does not supply its
missing conditions; return to the one-question flow instead of saving it.
