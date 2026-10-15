# Main-flow clarification and progression

Read this protocol for requirement design, including continued design after a
Decision Card is saved or skipped. Decide the next useful action before
deciding the next question. One question per turn limits the size of a needed
question; it does not require a question on every turn.

## Establish the flow, without restarting what is known

Use existing answers and reliable materials first. Move through these areas as
needed, skipping areas already clear rather than administering a checklist:

1. Business background: who encounters what problem, why it matters, and the
   relevant business constraints. Once that is understandable, move on; do not
   exhaust background research or demand unavailable metrics as a ritual.
2. Current business process and the intended improvement: how work happens
   today and what this requirement should fix. A new product may have no
   existing system; understand the current way of working instead.
3. Feature flow: who starts it, the ordered actions, important branches, and
   what the user receives at the end; identify what this version includes.
4. Pivotal details inside that flow: rules whose absence could make an agreed
   step or important outcome wrong, unsafe, or contradictory.

“Understand the complete flow” means understand the business behavior, not
exhaust every interaction or ask a business user to choose implementation
techniques. Investigation/parking decisions need only their own chosen flow;
do not invent a feature-design exercise for them.

## Classify each remaining unknown

Keep a compact internal list with the affected agreed step/result and one of:

- **Blocking business unknown**: two plausible answers would materially change
  the agreed main flow, scope, permissions, required output, acceptance,
  failure recovery, or preservation of existing/user-edited content. State
  the concrete difference that makes user input necessary. A genuine conflict
  between confirmed rules also belongs here; first check whether the rules
  apply to different actors, stages, or triggers before calling them a conflict.
- **Reviewable design detail**: a local, reversible presentation/interaction
  choice whose alternatives preserve those business results. Propose a
  recommendation labeled `设计建议，待评审` when useful; do not turn it into
  a confirmed fact, required behavior, or a separate prerequisite question.
- **Extension outside the agreed flow**: a new capability rather than an
  unresolved part of the current requirement. Keep it out of the clarification
  queue. Discuss it if the user requests it; an explicitly accepted extension
  becomes scope and must not later be silently removed to claim readiness.
- **Future validation**: a known performance/effectiveness uncertainty with an
  accepted practical validation plan. Preserve it as untested, not a new reason
  to ask the same design questions or claim that testing has passed.

“Related to the feature” alone does not make a detail blocking. Before asking,
be able to say in ordinary language: “Without this answer, step/result X could
be wrong in way Y.” Do not resolve a real business choice by calling it minor,
inventing a default, or hiding it in a future test. Conversely, do not promote
every conceivable UI state into a business blocker.

For example, whether ongoing recognition overwrites a user's corrected text
is pivotal. Whether a status badge appears beside that text is usually a
reviewable detail. Automatic relative-date conversion is an extension until
the user accepts it; do not introduce it merely to find another question.

## Choose the next action on every design turn

After reading/updating the current state, compare clarification with delivery:

1. **A blocking business unknown remains:** ask the highest-impact one, with a
   short explanation of which agreed step/result depends on it and the paired
   direction/confidence block. Do not offer completion or final documents.
   Reassess after the answer; a resolved blocker is not permission to start a
   chain of adjacent optional questions.
2. **The main flow and pivotal rules are clear, and document choice is unknown:**
   briefly restate the understood goal, end-to-end flow and scope. Say that the
   requirement is understood, then ask one question, for example:
   `是否据此生成 PRD 和测试文档？` This is the readback and document offer in
   one reply, not a separate summary-confirmation round. Retain uncertainty
   about future product performance. High confidence alone is not this check.
3. **Documents are already explicitly requested:** do not ask for consent again.
   A yes to an unambiguous PRD/test offer counts as the request. Run the artifact
   gate and the existing optional-card choice; card consent/content confirmation
   remain separate. A saved or skipped card alone is not a document request.
4. **The user declines documents and wants to keep talking:** acknowledge once,
   retain that choice, and inspect the remaining unknowns. Ask only a blocking
   question inside the agreed flow. If none remains, say the main flow is clear
   and wait for the user's additions; do not manufacture a question or a new
   feature. Answer user-led detail questions normally without creating a new
   clarification checklist.
5. **The user requested a Decision Card or a bounded decision rather than
   requirement documents:** follow that optional-card/decision route and its
   own gate. Do not impose a PRD offer or collect unrelated feature details.

For the same scope, do not repeat a declined document offer after each answer
or after context compression. Reopen it when the user asks for documents or
explicitly requests a fresh document decision for changed scope. `继续下一步`
means advance this workflow, not always ask, not automatic file consent, and
not proof that missing rules have been resolved. Do not enforce a fixed
question count or raise confidence to force a transition.

## Preserve state and separate gates

Keep the current goal, scope, flow, blocking unknowns, and document choice
(`待选择` / `已请求` / `暂不生成`) in the current requirement state. Before a
formal record, retain the choice compactly under the draft's `边界与约束` and
make `下一决策` reflect the actual next action, which may be document choice
or waiting for user additions rather than another question.

After a verified formal record exists, use it with confirmed later answers and
verified follow-up materials. Do not reactivate the old draft or rewrite the
original decision. If persistent follow-up state is needed, use a verified
`需求细化.md` in that same requirement folder: create populated content only
when absent, or safely patch only a file already proven to track this
requirement. Preserve confirmed facts and distinguish suggestions. Apply the
same path checks, no-op comparison, no silent overwrite and complete readback
as draft storage; stop on ownership, concurrent-edit or readback conflict.
Follow-up state is not a new Decision Episode or historical evidence.

Document readiness depends on the agreed requirement and its required tests,
not unresolved optional suggestions. Card-only metadata such as a review event
does not become a business-detail questionnaire before document choice. Keep
the formal-card gate intact when a card is wanted. A user who declines all
documents should not be routed into card preparation unless they request it.
