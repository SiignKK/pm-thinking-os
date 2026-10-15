# Safe validation scenarios

These scenarios preserve the behavioral contract formerly exercised by bundled
PowerShell tests. They are review cases for an isolated evaluator, not commands
that normal Skill use should run. Use only synthetic text and a disposable
temporary folder without Git. Do not use network access, secrets, personal data,
the real target project, the installed Skill directory, or global/system
locations.

## Project boundary

- With an explicitly selected existing temporary folder that has no `.git`,
  the folder itself is `<root>` and New episode can create
  `<root>/需求/<requirement-name>/需求草稿.md`.
- When the current opened project folder is unambiguous and no folder was
  explicitly named, it is `<root>` even without Git.
- When no project folder can be established, the Skill asks only for its
  location and creates no directory or file.
- An explicit project folder takes precedence over an unrelated current
  directory; the Skill never substitutes its own installation directory.
- A missing, non-directory, or ambiguous project location is rejected. No
  `git init` or Git discovery command is run.
- All writes remain beneath the selected `<root>/需求` tree; existing project
  code and files outside that tree are not modified.

## Episode create

- A complete synthetic card creates one
  `需求/<direct-child>/决策记录.md` with the required frontmatter and heading
  order.
- A record nested more deeply than one requirement folder is rejected as an
  invalid storage layout.
- Repeating a title produces a new ID and path without changing the first file.
- A missing required value causes no directory or file change.
- Invalid status, malformed review date, fewer than two options, or an
  unobservable-but-learning-eligible card causes no write.
- Final confidence accepts exactly the two documented dimensions, their exact
  field names, string values, and lowercase level enum.
- Missing, additional, differently cased, non-string, or unsupported confidence
  data causes no write.
- An event-only review trigger saves an empty calendar review date.
- A generated ID or path collision is retried and never overwrites.
- The saved record contains no unexpanded placeholder token.

## Requirement draft

- From a selected non-Git temporary folder, New episode can create a populated
  draft and later a confirmed formal record in the same direct requirement
  folder; History search finds the formal record without Git.
- Once the synthetic requirement has a readable temporary name, its first New
  episode turn renders current state in memory and creates exactly one populated
  `需求草稿.md` in one direct folder; it does not create an empty draft first and
  still creates no `决策记录.md`, `PRD.md`, or `测试文档.md`.
- On every later New episode turn, the existing draft is read before the new
  answer is merged, compressed, written, or used to choose the next question.
- When a reply changes no current requirement state, the draft remains
  byte-for-byte unchanged and its modification time does not move.
- A fact stated directly by the synthetic user, or a lossless shorter paraphrase
  that adds no meaning, enters `已明确`. Agent-added meaning, ambiguities, and
  inferences enter `待验证`, never `已明确`, until a later user answer verifies
  them.
- A later answer can correct a prior fact or inference. The rewritten draft
  contains only the current conclusion and does not retain the rejected
  conclusion, conversation text, reasoning, confidence history, or a change
  log.
- A draft beyond the approximately 800-Chinese-character soft limit is
  rewritten, deduplicated, and merged rather than mechanically truncated. The
  goal, users and scenario, effective requirements, boundaries, pivotal items
  awaiting verification, and next decision all remain present. Irreducible
  current state may exceed the soft target rather than lose required meaning.
- Ordinary draft updates produce no user-facing draft status or confirmation
  question. Only first creation, write failure, path/content conflict, and a
  successful transition to the formal record are reported.
- An existing folder or draft that cannot be proven to belong to the current
  requirement is never overwritten or merged. The flow stops and reports the
  collision or selects and reports a non-conflicting direct folder.
- A direct folder containing only `需求草稿.md` is excluded from History search,
  Outcome review, and Due reviews; it is not a Decision Episode or historical
  evidence.
- After the confirmed Decision Card is successfully created and read back as
  `决策记录.md` in the same folder, the existing draft remains in place but New
  episode no longer reads or writes it. The formal record is the authority.
- If formal creation or read-back verification fails, the flow does not claim
  a transition: the draft remains the current working state and later New
  episode turns continue to read and maintain it when no formal file was
  created. A created file that fails read-back is reported as a content conflict
  and automatic operations stop rather than treating either file as verified.

## Formal-card readiness regression

- Replay a requirement-design conversation for an offline meeting-transcription
  app immediately after the user confirms future local testing with real audio
  and says “继续下一步”. The preceding state still has 需求理解=`中`,
  决策充分=`低`, unresolved source-text/translation update rules,
  speaker-correction behavior, regeneration handling of manual edits, an
  unconfirmed Decision Owner, and no user-selected overall option. The next
  response continues clarification with the paired block and exactly one
  question. It presents no formal card, no save-confirmation request, and
  creates no formal record, PRD, or test document. The draft retains the blockers.
- The same unresolved business rules still block the card if a preceding
  response displayed “高” for both dimensions. A level label never replaces
  substantive readiness.
- A future-test plan resolves observability only. It does not silently convert
  the tester into the Decision Owner or an agent recommendation into a chosen
  option, and “continue” does not authorize requirement closure.
- After material business rules, owner, real alternatives, selected option,
  accepted risks, and validation/exit plan are clear, an unperformed future
  effectiveness test alone does not prevent both levels reaching “高” or a
  requested card offer. For requirement design with no card request, the
  understood-flow readback/document offer is the next action instead of
  automatic card preparation. “很高” is not required. A full card appears only after
  the user chooses generation, and no formal file is saved until the user
  separately confirms its content.
- A user-requested early summary stays labeled as an incomplete stage summary
  and never invites formal saving. The agent does not independently narrow
  complete requirement design into a first-version direction decision.
- A user explicitly choosing a bounded experiment, no action, or parking can
  receive a truthfully lower-confidence limited card when its own gate passes.
  The card states its limited scope and the original requirement's unfinished
  status; full-feature blockers are preserved and no full-feature PRD follows.
- User confirmation of a previously premature card does not bypass the gate
  on save. Legacy lower-confidence records remain searchable and reviewable.

## New-episode question framing

- Every New episode reply that shows `当前把握程度` also shows
  `当前思考方向` immediately before it.
- `当前思考方向` contains exactly one short bullet sentence in everyday
  language. It names the current business decision or unknown being clarified,
  remains tied to the original requirement, and does not expose internal
  reasoning.
- The direction sentence is updated as the inquiry advances instead of merely
  repeating the user's latest answer.
- Before rendering, the direction is checked against the original business
  need and corrected if it no longer advances that need.
- If an explanation-only reply omits `当前把握程度`, it also omits
  `当前思考方向`.
- Requirement artifacts, Outcome review, History search, and Due reviews do
  not gain this direction block.
- One-question formatting is conditional on needing a question; a brief
  waiting status does not invent one just to complete the paired block.

## Main-flow progression and long-dialogue regression

- From a feature name with no context, ask about the relevant business
  background first. Once the problem, users and reason are understandable,
  advance to current work/problem and intended feature flow rather than
  exhausting background metrics. Reuse facts already supplied; do not restart
  each stage or ask a business user for implementation techniques.
- Replay the offline-transcription conversation after formal-card saving and
  later confirmation of main-flow rules. Classify remaining gaps by affected
  step/result. A genuine content-overwrite contradiction may need clarification,
  but local status-label/export presentation or unrequested date conversion
  does not by itself block a brief flow readback and PRD/test-document offer.
  No PRD is created until the user accepts or already requested it.
- Continue the replay after a pivotal answer is supplied. The agent rechecks
  readiness and advances to delivery choice, rather than asking the next
  adjacent local detail. Repeat over multiple turns; one successful reply is
  not evidence that a long dialogue converges.
- A saved card and two high confidence labels never override unresolved
  main-flow rules. The earlier transcript with undefined translation updates,
  speaker correction or manual-edit retention asks one pivotal question and
  offers no final documents or formal completion. Inflated labels do not help.
- Contrast an ongoing background result with an explicitly started new task:
  the former must preserve manual edits when that rule is confirmed; the
  latter may display a new version while retaining the old version if confirmed.
  Do not repeatedly ask about a conflict that disappears when the triggers are
  kept distinct. An actual same-trigger contradiction still requires a choice.
- After a user declines documents and asks to continue, inspect the unknowns.
  With no main-flow blocker, give a short waiting status without another
  question, optional-card offer, PRD, test document or extension proposal. On a
  later unchanged turn, retain the refusal and do not reoffer documents.
- With a genuinely unresolved main-flow blocker after refusal, ask only that
  blocking question; after its answer, stop if nothing pivotal remains. Retain
  the refusal rather than restarting the document offer.
- A user-led question about a local UI detail is answered normally. Do not
  launch an unsolicited sequence of related feature questions afterward.
- A yes to the explicit PRD/test offer becomes the pending request; do not
  require the literal word “PRD” again. Preserve separate card choice and save
  confirmation. An accepted request with complete rules and a skipped card
  generates both files from the draft, without a decision record.
- Reviewable suggestions stay visibly unconfirmed and outside mandatory
  requirements/tests. A confirmed scope item is never demoted to an optional
  suggestion to claim readiness; required tests have definite expected results.
- After formal saving, subsequent confirmed answers and document choice can
  be retained in safely verified follow-up state in the same folder, while
  both the original formal record and frozen draft remain unchanged. A
  follow-up-file collision or failed readback stops writes, not overwrites.

## Optional card and skipped-card documents

- At a card-ready state with card recording selected and no prior choice, the next reply explains the
  purpose in one or two plain-language sentences and asks only whether a card
  is wanted. It includes the paired block and no full card or formal file.
- A yes to that offer allows rendering only after the substantive gate passes;
  it does not authorize saving. A direct explicit card request counts as yes,
  without a repeated choice question. Content confirmation is still required.
- A no records the current skip choice compactly in the draft, skips generation
  and save confirmation, and creates no decision record or Episode ID. No
  differently named replacement card is created. Without an explicit PRD
  request, no PRD/test document is created either.
- A skipped choice persists across later turns and context compression; the
  Skill does not repeat the offer. An explicit change to requesting a card
  replaces that state and uses the normal substantive gate.
- A pending explicit PRD request continues after a skip. With complete business
  rules, it creates PRD and test documents from the verified draft without a
  formal record, accurately labels the absent Episode ID, and cites real source
  files. With missing rules it asks one pivotal question and creates neither.
- Card-only metadata is not collected merely to fill a declined card, while
  unresolved permissions, scope, acceptance, or other business rules still
  require clarification. Draft/PRD folders without a formal record remain
  excluded from history, due checks, and formal outcome reviews.

## Search and due review

- Search returns every direct record containing a synthetic matching ID, title,
  or keyword and excludes nonmatches.
- An ambiguous review search does not select a record automatically.
- A non-closed record with a past or same-day review date is due.
- A future date, empty date, event-only trigger, or closed record is not
  calendar-due.
- A malformed non-empty date is reported rather than guessed.

## Outcome review

- A complete confirmed review is added only inside `Outcome Reviews`.
- The original frontmatter, Decision, Assumptions, Prediction, Falsification
  Signal, and any previous review remain unchanged.
- A review missing any required value causes no file change.
- A legacy Episode without `Final Confidence` remains searchable and can
  receive an append-only review.
- The first and later reviews preserve the complete original byte prefix and
  retain chronological append order.

## Requirement artifacts

- Below the readiness gate, no PRD or test document is created.
- At the gate, an explicit PRD request creates both files in the same existing
  requirement folder and every requirement maps to at least one test.
- Existing artifact files are not overwritten without explicit approval for
  those exact files.
- No artifact is written at the project root, beneath `pm-os-data`, or inside
  the Skill directory.

## Packaging and installation

- The public Skill contains no executable script and makes no network request.
- Normal operation does not inspect secret stores or secret-bearing environment
  variables.
- The Skill does not create or alter global Skill entries; installation belongs
  to the host's standard workflow.
- Validation never initializes Git, stages, commits, pushes, rewrites history,
  or modifies the source Skill.
