# Agent-native storage protocol

Use ordinary read-only shell inspection and the agent's normal file-editing
mechanism. This Skill intentionally bundles no executable scripts, performs no
network requests, and has no installer.

## Safety boundary

- Resolve the current project only with read-only Git discovery from the user's
  current working directory.
- Read and write only beneath the resolved `<root>/需求` tree.
- Never inspect credential stores or secret-bearing environment variables.
- Never use network access for storage, search, validation, or review.
- Never modify the Skill directory, system directories, global Skill
  directories, Git configuration, or Git history during normal use.
- Never stage, commit, or push.
- Treat every existing record as user data. Refuse silent overwrite.

## Resolve the project

Run `git rev-parse --show-toplevel` from the current working directory without
Git directory/work-tree overrides. Accept exactly one existing absolute
directory. If the command fails or returns anything else, stop and ask the user
to open or enter the target Git project.

Do not create `需求` merely to test whether a location is usable. Create it
only while saving a user-confirmed Episode under an already resolved project.

## Search

Enumerate only direct records matching
`<root>/需求/*/决策记录.md`. Do not treat a more deeply nested file as a
Decision Episode.

Search case-insensitively across the complete Markdown text for the user's ID,
title, or requirement keywords. A read-only tool such as `rg` is appropriate.
For every match, read its frontmatter and relevant sections before reporting
it. Return no match when the text is absent.

For a create flow, use similar matches as context but never reuse their ID or
path. For a review flow, require one unique record. If more than one record
matches, ask for one identifying detail rather than choosing by recency.

## Create

1. Validate the confirmed card against
   [episode-schema.md](episode-schema.md). If a required value is missing or
   invalid, do not write anything.
2. Generate an Episode ID in the documented format using current UTC time and
   a locally generated six-character hexadecimal suffix.
3. Derive a readable requirement folder name from the title. Remove path
   separators, control characters, and characters forbidden by the host file
   system. Trim trailing spaces and punctuation. If the result is empty, use
   the Episode ID.
4. Set the candidate path to
   `<root>/需求/<readable-name>/决策记录.md`.
5. Check the complete `<root>/需求` tree for both the candidate path and the
   generated ID. If either exists, generate a new suffix and recheck. If the
   readable folder already belongs to another Episode, append the new Episode
   ID to the folder name. Never replace or merge an existing record.
6. Render the complete Markdown template in memory and verify no placeholder
   tokens remain.
7. Create only the needed `需求` and requirement directories, then add the
   new file with the agent's normal patch/edit mechanism. The operation must be
   create-only: if the target appeared since step 5, stop and report the
   collision.
8. Read the saved file back. Confirm the ID, title, direct-child layout,
   required heading order, final confidence, and review trigger. Report the ID
   and absolute path.

No temporary payload, home-directory fallback, global installation, or
network service is part of this procedure.

## Append an outcome review

Before editing, keep the original complete text in context. Validate the
confirmed review against [review-protocol.md](review-protocol.md). Append the
first review after the creation-time no-review marker, and append later reviews
after the previous review. A legacy marker remains unchanged as historical
content.

Use this shape:

```markdown
### Outcome Review — <UTC ISO timestamp>

#### Actual Outcome

<observed result and source>

#### Prediction Difference

<expected versus actual>

#### Assumption Updates

- <supported, rejected, or unknown>

#### External Factors

- <material factor or none observed>

#### Process Quality

<assessment using information available then>

#### Outcome Quality

<assessment of observed business result>

#### Next Time

<specific future reminder>
```

Apply one tight append patch inside `Outcome Reviews`. Do not change
frontmatter, original sections, markers, or earlier reviews. Read the result
back and verify it starts with the exact original content; if it does not,
restore the original file and report that the review was not saved.

## Inspect due reviews

Read only direct records. Ignore `closed` records and records with an empty
`review_at`. Parse a non-empty value as a real calendar date; do not guess an
invalid date. A record is due when its date is on or before the requested
as-of date. An event-only trigger is not calendar-due.

## Standard installation

This public Skill is the directory containing `SKILL.md`, `agents/`,
`references/`, `SOURCE.md`, and `LICENSE.txt`. Installation or linking is
delegated to the host's standard Skill installation workflow. The Skill must
not create, replace, or remove global entries itself.
