# EndNote Group Workflow

## Scope Rule

When the user specifies an EndNote group, use only that group unless the user explicitly allows using the whole library or adding external references.

## Required Group Report

Before writing, report or save:

- target library;
- target group name;
- number of records found;
- record numbers if available;
- records with missing PDF;
- records with missing abstract;
- records with incomplete metadata;
- duplicate candidates;
- unreadable records;
- records outside scope that were excluded.

## Reading Strategy

1. Prefer EndNote UI/export for exact group membership.
2. If using `.Data/rdb/refs.MYD`, treat group membership parsing as fragile and verify when exact membership matters.
3. Build a literature analysis table before drafting.
4. Track which records support each paragraph/topic.
5. For existing prose, track the exact sentence or proposition supported by each record and the evidence level used.
6. Do not infer citation mapping from static numbers or group order.
7. Do not force irrelevant records into unrelated paragraphs. If permitted, minimally narrow, reorder, or optimize the sentence and its transitions to match available evidence; otherwise report it as unsupported.
8. List unused records and the reason: irrelevant, duplicate, unreadable, metadata incomplete, outside topic, or manually excluded.

## Evidence Discipline

Use citations only for claims supported by the cited record. If only title/abstract was read, do not make detailed claims about experiments unless the abstract supports them clearly.

When modifying existing text, preserve its research purpose and improve logical continuity without inventing claims. Log any material change in meaning and the record numbers that justify it.
