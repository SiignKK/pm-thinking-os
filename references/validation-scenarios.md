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
