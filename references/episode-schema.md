# Decision Episode schema

Render a confirmed Decision Episode directly as UTF-8 Markdown. Do not create
an intermediate JSON payload.

## Required values

Collect and validate these values before creating a file:

| Value | Rule |
|---|---|
| Title | Short neutral title |
| Status | Exactly `active`, `parked`, or `closed` |
| Learning eligible | Boolean; use false when an outcome cannot be observed legally and practically |
| Sensitivity | Use `private` unless the user explicitly chooses another project-approved label |
| Question | The decision that must be made |
| Desired outcome | The observable change sought |
| Owner | Decision Owner role |
| Evidence | One or more sources with limitations |
| Unknowns | One or more pivotal unknowns |
| Options | At least two real choices, including no action or a smaller experiment when relevant |
| Decision | The explicitly confirmed choice |
| Assumptions | One or more assumptions with confidence |
| Prediction | Observable pre-outcome expectation |
| Falsification signal | What would change the judgment |
| Review trigger | A meaningful date or event |
| Review date | A real calendar date only when the user has a calendar commitment; otherwise empty |
| Tags | Optional search terms that contain no sensitive data |

The final confidence snapshot has exactly two dimensions:
`requirement_understanding` and `decision_sufficiency`. Each dimension has
exactly one non-empty `level` and one non-empty `reason`. Allowed levels are
`very_low`, `low`, `medium`, `high`, and `very_high`. Store no trend
symbol and no per-turn history.

## Markdown template

Use this exact heading order. Replace every angle-bracket token before writing.
Use a UTC ISO timestamp for `created_at`. Quote all text frontmatter values.
Keep an absent calendar review date as an empty quoted value.

```markdown
---
id: "<DE-UTC-DATE-TIME-RANDOM>"
title: "<title>"
status: "<active|parked|closed>"
created_at: "<UTC ISO timestamp>"
review_at: "<calendar date or empty>"
owner: "<Decision Owner role>"
sensitivity: "<sensitivity>"
learning_eligible: <true|false>
tags: "<comma-separated search terms or empty>"
---

# <title>

## Question

<question>

## Desired Outcome

<desired outcome>

## Evidence

- <source and limitation>

## Unknowns

- <pivotal unknown>

## Options

- <real option>
- <real alternative>

## Decision

<confirmed choice>

## Assumptions

- <assumption and confidence>

## Final Confidence

- Requirement Understanding: <very_low|low|medium|high|very_high> — <reason>
- Decision Sufficiency: <very_low|low|medium|high|very_high> — <reason>

## Prediction

<observable expectation>

## Falsification Signal

<signal that would change the judgment>

## Review Trigger

<meaningful date or event>

## Outcome Reviews

_No reviews recorded at creation._
```

The Episode ID format is `DE-` followed by the UTC calendar date, UTC time,
and six lowercase hexadecimal characters, separated by hyphens. Generate the
random suffix locally; never derive it from a user, account, device, network
address, secret, or repository identifier.

Episodes created before this schema may omit `Final Confidence`. They remain
searchable and may receive appended outcome reviews. Older no-review markers
are historical content and remain unchanged when the first review is appended.

Do not put personal data, credentials, access tokens, customer identifiers, or
real sensitive company examples into a Decision Episode.
