# Grounded LLM Wiki Operating Rules

You are maintaining a grounded LLM wiki.

## Core rules

1. Raw sources are the ground truth.
2. The wiki is a navigation and synthesis layer, not the final authority.
3. Do not save ordinary conversational answers into the wiki.
4. Any high-stakes answer must be checked against raw sources before finalizing.
5. Keep ingest conservative unless the user explicitly asks for broader updates.

## High-stakes query rule

You must not produce a final answer about:

- numbers
- dates
- direct factual claims
- important comparisons
- recommendations
- contradiction resolution
- decision-relevant judgments

from wiki pages alone.

You may use the wiki to route attention, but the final answer must be grounded in linked raw sources.

If unsure, treat the question as high-stakes.

## Ingest rule

When a new source is added, default actions are:

- create or update one `source-note`
- update `index.md`
- update only the most relevant entity or concept page
- update `conflicts.md` if needed
- append to `log.md`

Do not perform broad multi-page rewrites by default.

If the source would require many page updates, ask the user first.

## Taxonomy rule

Every wiki page or major section must have a clear type.

Allowed page types:

- `source-note`
- `entity`
- `concept`
- `contradiction`
- `stable-synthesis`
- `provisional`

Do not blur stable knowledge and temporary reasoning.

## Saving rule

Do not save normal answers back into the wiki.

Only save content if it is clearly:

- durable
- reusable
- source-linked
- mature enough to deserve promotion

If saved, label it explicitly as `stable-synthesis` or another valid page type.

## Contradiction rule

All important disagreements must be recorded in `conflicts.md` or typed contradiction pages.

Each contradiction record must include:

- disputed topic
- source A
- source B
- what conflicts
- status: unresolved / partially resolved / resolved
- links to raw sources

Do not prematurely collapse disagreements unless the evidence is clear.

## Lint rule

When checking the wiki:

- find broken links
- find orphan pages
- find missing source links
- find outdated stable syntheses
- find unresolved contradictions

When validating claims, use raw sources.

Do not smooth contradictions just to make the wiki cleaner.
