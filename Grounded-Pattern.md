# LLM Wiki: The Grounded Pattern

A pattern for building personal knowledge bases using LLMs, balancing the power of structured accumulation with the safety of raw evidence.

This file explains the pattern at a high level. It is not the operating manual. The strict execution rules belong in `AGENTS.md` or `CLAUDE.md`.

## The core idea

Most people's experience with LLMs and documents looks like raw RAG: you upload files, the LLM retrieves chunks at query time, and generates an answer. This works, but almost nothing accumulates.

A better pattern is to let the LLM build a persistent wiki over your raw sources. But the wiki must not replace the sources themselves.

The grounded pattern is simple:

- raw sources remain the ground truth
- the wiki becomes a navigation and synthesis layer
- important answers must return to raw sources before finalizing

The wiki helps the model understand where knowledge lives, how ideas connect, and where contradictions exist. But final truth stays tied to the underlying documents.

## Architecture

There are three layers:

### 1. Raw sources (Ground Truth)

Immutable source documents:

- articles
- papers
- transcripts
- PDFs
- notes
- data files

The LLM reads them but never rewrites them.

### 2. The wiki (Navigation and Synthesis Layer)

A directory of LLM-generated markdown files used for structure and routing, not as the final authority.

Recommended page types:

- `source-note`
- `entity`
- `concept`
- `contradiction`
- `stable-synthesis`
- `provisional`

### 3. The schema

A file such as `AGENTS.md` or `CLAUDE.md` that defines:

- operating rules
- trust boundaries
- ingest behavior
- query behavior
- saving rules
- lint rules

## Operations

### Ingest

When a new source arrives, the LLM should:

- read the raw source
- create or update a `source-note`
- update `index.md`
- update only the most relevant entity or concept pages
- record disagreements in `conflicts.md`
- append an entry to `log.md`

Default ingest should be conservative.

### Query

The wiki may route attention, but it must not silently replace evidence.

Three query modes:

- Browsing questions: the wiki is usually enough
- Exploratory synthesis: the wiki helps, but raw sources may still be inspected
- High-stakes questions: raw sources must be checked before the final answer

High-stakes questions include:

- factual claims
- comparisons
- recommendations
- contradiction resolution
- decision-relevant answers
- any answer where precision matters

### Selective feedback

Do not automatically save answers back into the wiki.

Only save:

- durable syntheses
- high-value comparisons
- mature concept pages
- contradiction records
- reusable framework notes

Ordinary conversational answers should remain ephemeral.

### Lint

Lint should check for:

- orphan pages
- broken links
- duplicated pages
- missing source links
- outdated syntheses
- unresolved contradictions

When validating contradictions or stale claims, the LLM must check raw sources, not just smooth over the wiki.

## Contradictions

Maintain `conflicts.md` or typed contradiction pages.

Each contradiction record should contain:

- the disputed topic
- source A
- source B
- what exactly conflicts
- whether the issue is unresolved, partially resolved, or resolved
- links back to the raw sources

## Summary

The grounded pattern is not:

`replace RAG with a wiki`

It is:

`use a wiki to organize and route understanding, while keeping final truth anchored in raw evidence`
