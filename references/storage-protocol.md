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
- Treat every existing folder, draft, and record as user data. Refuse silent
  overwrite or merge.

## Resolve the project

Run `git rev-parse --show-toplevel` from the current working directory without
Git directory/work-tree overrides. Accept exactly one existing absolute
directory. If the command fails or returns anything else, stop and ask the user
to open or enter the target Git project.

Do not create `需求` merely to test whether a location is usable. Under an
already resolved project, create it only for the first named New-episode draft
or while saving a user-confirmed Episode.

## Search

Enumerate only direct records matching
`<root>/需求/*/决策记录.md`. Do not treat a more deeply nested file as a
Decision Episode. A direct folder containing only `需求草稿.md` is not a
Decision Episode, historical evidence, an Outcome review target, or a due
review; do not include it in those scans.

Search case-insensitively across the complete Markdown text for the user's ID,
title, or requirement keywords. A read-only tool such as `rg` is appropriate.
For every match, read its frontmatter and relevant sections before reporting
it. Return no match when the text is absent.

For a create flow, use similar matches as context but never reuse their ID or
path. For a review flow, require one unique record. If more than one record
matches, ask for one identifying detail rather than choosing by recency.

## New-episode draft storage

Read and follow [draft-protocol.md](draft-protocol.md). Draft operations use the
same resolved `<root>/需求/<requirement-name>/` boundary and the agent's normal
patch/edit mechanism. Compare against the complete text just read, make no
write when current state is unchanged, and read back every create or update.
An edit that no longer applies is a content conflict, not permission to replace
the file.

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
4. If the active New-episode draft folder has been verified as belonging to
   this requirement and contains no `决策记录.md`, set the candidate path to
   that same folder's `决策记录.md`. Otherwise set it to
   `<root>/需求/<readable-name>/决策记录.md`.
5. Check the complete `<root>/需求` tree for both the candidate path and the
   generated ID. If either exists, generate a new suffix and recheck. If the
   readable folder belongs to another Episode or cannot be proven to belong to
   the current requirement, append the new Episode ID to the folder name. A
   verified active draft folder for this requirement is reusable; no other
   existing folder is. If `决策记录.md` appears inside that active draft folder,
   stop and report the collision instead of creating a second requirement
   folder. Never replace or merge an existing record.
6. Render the complete Markdown template in memory and verify no placeholder
   tokens remain.
7. Create only the needed `需求` and requirement directories, then add the
   new file with the agent's normal patch/edit mechanism. The operation must be
   create-only: if the target appeared since step 5, stop and report the
   collision.
8. Read the complete saved file back and require byte-for-byte equality with
   the rendered card. Also confirm the ID, title, direct-child layout, required
   heading order, final confidence, and review trigger. Report the ID and
   absolute path. Only after every check succeeds does `决策记录.md` become the
   authority; keep any draft file in place and stop its automatic reads and
   writes. If creation fails before a file exists, the draft remains
   authoritative and maintenance continues. If a file exists but read-back
   verification fails, do not switch authority or overwrite it: report a
   content conflict and stop automatic operations until that conflict is
   resolved.

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

Read only direct `需求/*/决策记录.md` records. Ignore draft-only folders,
`closed` records, and records with an empty `review_at`. Parse a non-empty value
as a real calendar date; do not guess an invalid date. A record is due when its
date is on or before the requested as-of date. An event-only trigger is not
calendar-due.

## Standard installation

This public Skill is the directory containing `SKILL.md`, `agents/`,
`references/`, `SOURCE.md`, and `LICENSE.txt`. Installation or linking is
delegated to the host's standard Skill installation workflow. The Skill must
not create, replace, or remove global entries itself.
