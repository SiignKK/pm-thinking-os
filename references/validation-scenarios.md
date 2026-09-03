# Safe validation scenarios

These scenarios preserve the behavioral contract formerly exercised by bundled
PowerShell tests. They are review cases for an isolated evaluator, not commands
that normal Skill use should run. Use only synthetic text and a disposable
temporary Git repository. Do not use network access, secrets, personal data,
the real target project, the installed Skill directory, or global/system
locations.

## Project boundary

- From a deep child of a temporary Git repository, project discovery returns
  exactly that repository root.
- From a non-Git temporary directory, discovery fails and creates no
  `需求` directory.
- Ambient Git discovery overrides do not redirect the resolved project and are
  not changed by the check.
- Empty, relative, root-relative, or multiple-path discovery output is rejected.
- All searches and writes remain beneath the resolved temporary
  `<root>/需求` tree.

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

- From a non-Git temporary directory, New episode discovery fails, creates no
  directory or file, and reports that the user must open or enter a Git
  project.
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
- No artifact is written at repository root, beneath `pm-os-data`, or inside
  the Skill directory.

## Packaging and installation

- The public Skill contains no executable script and makes no network request.
- Normal operation does not inspect secret stores or secret-bearing environment
  variables.
- The Skill does not create or alter global Skill entries; installation belongs
  to the host's standard workflow.
- Validation never stages, commits, pushes, rewrites history, or modifies the
  source Skill.
