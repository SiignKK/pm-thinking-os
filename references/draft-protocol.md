# New-episode requirement draft protocol

Use this protocol only while a requirement is in New episode and has no
successfully saved Decision Episode. `需求草稿.md` is the compact current
working state for that requirement. It is not a conversation summary,
reasoning log, decision history, change log, or short PRD.

## Authority and lifetime

- Before touching a draft, check whether the tracked direct requirement folder
  already contains a successfully saved and verified `决策记录.md`. If it does,
  the formal record is authoritative: do not read or write the draft.
- A confirmed Decision Card does not end draft maintenance by itself. Switch
  authority only after `决策记录.md` is created and read-back verification
  succeeds.
- Keep `需求草稿.md` after that switch. Do not delete, rename, archive, or
  silently update it.
- If the user skips the card, no authority switch occurs: keep the draft as
  current requirement state, even when PRD/test documents are later generated.
  Read the stored skip choice before deciding whether to offer a card again.
- If formal creation fails before a file exists, report the failure and
  continue the draft loop.
- If a created formal file fails complete read-back verification, do not switch
  authority. Report a content conflict and stop automatic draft and formal
  operations until the conflict is resolved; retain the draft unchanged.

## Resolve or create the draft

After resolving the selected project folder, keep an initial requirement in conversation
memory only until its input yields a readable temporary requirement name. Do
not delay until Decision Card confirmation.

Sanitize the temporary name using the readable-folder rules in
[storage-protocol.md](storage-protocol.md). The intended layout is exactly
`<root>/需求/<temporary-name>/需求草稿.md`.
If sanitization leaves no readable business name, keep the state in memory and
wait for enough business context; do not generate an Episode ID for a draft.

Before creating or resuming it:

1. Inspect the candidate direct folder without writing. A previously remembered
   path may be resumed only when it is still inside `<root>/需求`, has no formal
   record, and the complete draft can be read and matched to the current goal,
   users, and scenario without contradiction.
2. Never infer that an existing same-named folder or file belongs to the
   current requirement merely from its name. If ownership or contents conflict,
   select and report a sanitized direct folder with a locally generated
   six-character hexadecimal suffix; if that path is also unsafe, stop and
   report the collision.
3. On the first named turn, build the initial current state in memory, compress
   it if needed, then create the populated draft once. Do not create an empty
   draft first. Create only the needed `需求` directory, one direct requirement
   folder, and `需求草稿.md`. The file creation is create-only. Do not create
   `决策记录.md`, `PRD.md`, or `测试文档.md` at this stage.
4. Read the complete created file back and compare it with the rendered initial
   state before treating the path as active. Tell the user once that the draft
   was created and include its path.

## Per-turn state loop

For every New-episode turn after a readable temporary name is available, use
this order before asking the next question:

1. Check for a formal record as described above. If none exists, read the
   complete current draft if it exists. Absence is expected only on the first
   named turn; do not reconstruct a missing or unreadable established draft
   from conversation memory. On a continuing turn, judge the preceding
   confidence levels from this pre-answer state before merging the answer; do
   not store a confidence history in the draft.
2. Merge the user's new answer into an in-memory current-state model. A direct
   user statement, including a lossless shorter paraphrase that adds no meaning,
   belongs in `已明确`. Agent-added meaning, unresolved ambiguity, and inference
   belong in `待验证` until a later normal answer verifies them. A correction
   replaces the old conclusion.
3. Render the complete current state in memory and judge the current confidence
   levels from it. Around the approximately 800-Chinese-character soft limit,
   rewrite, deduplicate, and merge; never mechanically truncate. Content
   preservation takes priority over the target, so an irreducible current state
   may temporarily remain longer than 800 characters.
4. On the first named turn, perform the single populated create and read-back
   from **Resolve or create the draft**. On later turns, compare the rendered
   state with the text read in step 1. If requirement state did not change, do
   not write and preserve the file byte-for-byte. If it changed, apply one tight
   edit against the exact text that was read, then read back and verify the
   result. If the file changed concurrently, the edit no longer applies, or
   read-back differs, stop rather than overwrite and report the conflict or
   failure.
5. Compute confidence trends from the preceding and current levels. Apply
   **Optional card choice** in [decision-protocol.md](decision-protocol.md):
   once business clarification is sufficient, explain and ask once if the
   choice is unknown; if skipped, bypass the card and continue the actual task
   or pending explicit PRD request. Preserve the current choice under
   `边界与约束`, including after context compression. Before changing the next
   action from a question to a requested formal Decision Card, check
   the **Formal Decision Card gate** in
   [decision-protocol.md](decision-protocol.md) against the verified updated
   state. When blocked, keep the blockers in `待验证` and make `下一决策` the
   most important unresolved business choice, not card confirmation. After a
   skip, never make `下一决策` card confirmation unless the user changes their
   choice. For remaining business clarification, ask the single
   highest-information-value question. If no clarification remains, continue
   the requested next task or end with a brief status instead of inventing a
   question. Future validation plans do
   not remove unresolved business rules from the draft.

An ordinary successful update is silent. Do not announce it, ask whether to
write it, or add a separate draft-confirmation question. User-facing draft
messages are limited to first creation, write/read-back failure, path or content
conflict, and successful transition to the formal Decision Episode.

## Content contract

Use these headings in this order when their content exists:

```markdown
# 目标
# 用户与场景
# 已明确
# 边界与约束
# 待验证
# 下一决策
```

Empty sections may be combined or omitted to stay compact, but preserve the
meaning and order of the remaining sections. Always retain the goal, users and
scenario, effective requirements, boundaries and constraints, pivotal items
awaiting verification, and next decision. Keep only current effective state.

Never store conversation quotations, internal reasoning, confidence changes,
superseded conclusions, rejected assumptions, or a change history. Do not put
personal data, credentials, customer identifiers, or sensitive real examples
in the draft.
