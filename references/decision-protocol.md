# Decision protocol

Use this protocol for a new requirement. Keep it conversational: one question
per turn, not a visible questionnaire.

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

Clarify:

- what choice must be made;
- what outcome should change;
- who owns the decision;
- when the choice is needed.

Do not assume the product manager is the Decision Owner.

## 3. Find the pivotal unknown

Ask the one question most likely to change the choice. Prefer source,
frequency, scenario, impact, existing behavior data, or authority over cosmetic
details.

## 4. Judge confidence before the next question

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
| 中 | 证据有限，但方案、主要风险和负责人明确，可以作出可撤销或风险可控的决定 |
| 高 | 证据、备选方案、风险、负责人和验证方法比较明确，当前决定有充分依据 |
| 很高 | 有多种相互印证的证据，结果可以观察，主要假设能够被后续结果证明或推翻 |

The strict level order is `很低 < 低 < 中 < 高 < 很高`. Use “很高” sparingly.
需求理解 reaching “很高” does not imply that 决策充分 also reaches “很高”.

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

## 5. Prepare for later documents

Assume the user is an 不懂技术的业务人员. Use 日常语言 and 一次只问一件事.
Across the conversation, learn who uses the feature, where the flow starts,
what should happen step by step, who cannot act, limits/exceptions/failures,
how success is observed, and what existing behavior must not change.

Prefer questions such as “谁会使用这个功能？” and “做完以后应该看到什么？”
Do not ask the user to explain technical implementation. Do not repeat facts
already supported by their answers or reliable source material.

## 6. Challenge once

Present the strongest material challenge, not a catalogue. Explicitly check
selection bias. Example: measuring “cannot find support” only through support
complaints misses people who never reached support.

Stop challenging when the risk is understood, no new information appears, or a
small experiment is more informative.

## 7. Check observability

A learning-eligible Episode needs a legal and executable way to observe an
outcome in a reasonable period. Record:

- observation source;
- review date or event trigger;
- expected result before the outcome;
- known blind spots.

If no source exists, preserve the decision as `parked` with
`learning_eligible=false`. Do not invent a survey, interview, log, or future
integration that has no owner and date.

## 8. Decide and confirm

Include a real alternative such as no action or a smaller experiment. Record
accepted risks and unresolved disagreement. Present the full Decision Card and
ask for explicit confirmation before saving.
