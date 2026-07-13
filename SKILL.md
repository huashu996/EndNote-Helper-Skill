---
name: endnote-helper-skill
description: End-to-end academic writing workflow for importing references into EndNote, validating metadata, managing EndNote groups, writing related work, semantically optimizing existing prose, inserting group-scoped real Word/EndNote CWYW citations based on sentence meaning, preserving Word formatting, checking hyperlinks, and learning prose plus journal layouts such as IEEE RA-L two-column style from samples.
---

# EndNote Helper Skill

## Purpose

Use this skill when the task involves any combination of:

- importing papers, websites, DOI records, PDFs, BibTeX, RIS, or EndNote XML into an EndNote library;
- cleaning, validating, deduplicating, or organizing EndNote references and groups;
- generating related work, literature review, thesis introduction, or research-status sections from a specified EndNote group;
- analyzing existing prose sentence by sentence, making limited evidence-based revisions, improving local argument flow, and inserting suitable references from a specified EndNote group;
- inserting real EndNote Cite While You Write citations into Microsoft Word;
- preserving Word template formatting, headings, table of contents, EndNote fields, cross-references, DOI links, and URL links;
- learning writing style from a thesis, paper, Word template, existing chapter, or user-provided writing sample.
- rebuilding an existing English draft in an IEEE/RA-L-like two-column Word layout while keeping citations as updateable EndNote CWYW fields.

This skill combines the practical workflows of `related-work` and `endnote-cwyw-citations` into a single reusable process. It must not overwrite those original skills.

## Non-Negotiable Rules

- Interactive task configuration is mandatory. Before any functional work, use `references/00_interactive_task_configuration.md`.
- Every task starts by identifying the requested function and checking required parameters.
- If required parameters are missing, ask for all missing required items together where practical.
- Do not repeat questions for information the user already supplied.
- After the user answers, output a task configuration summary and wait for explicit confirmation.
- Do not read EndNote groups, generate final prose, insert citations, import references, or modify Word files until the user confirms the task summary.
- Only the state `READY_TO_EXECUTE` permits file modification or EndNote/Word actions.
- Do not treat default values as confirmed unless the user explicitly says to use defaults.
- Never invent metadata, publication status, methods, experiments, conclusions, citations, DOI, URLs, or EndNote record numbers.
- Never cite papers that were not read or analyzed at the level required by the user.
- Never add references outside a user-specified EndNote group unless the user explicitly allows it.
- For existing prose, match references to the meaning of each claim, not merely to shared keywords, current citation numbers, file order, or EndNote group order.
- Treat static citations such as `[1]` as placeholders unless the user explicitly asks to restore the original sources. Do not assume that `[1]` maps to the first record in a group.
- When the user allows source-text optimization, make only the smallest revision needed for evidentiary support and coherent transitions. Preserve the original research intent, terminology, paragraph role, and surrounding logic.
- If the specified group cannot support a claim, do not force an irrelevant citation. Revise, narrow, reorder, or bridge the claim only when the user allowed text modification; otherwise leave it unchanged and report it as unsupported.
- Never replace EndNote CWYW citations with plain text, static `[1]` numbers, or hand-written bibliography entries.
- Never run `ENRemoveFieldCodes` unless the user explicitly asks to break EndNote links.
- Never silently delete suspected duplicate references. Report candidates and ask for confirmation before deletion or merge.
- Preserve unrelated Word content, existing styles, page setup, table of contents, headers, footers, page numbers, cross-references, and existing EndNote fields.
- If a required step cannot be verified, state what was done, what failed, and what remains manual.

## Required Reference Files

Before acting, open only the reference files needed for the task:

- `references/00_interactive_task_configuration.md` for mandatory task detection, missing-parameter questions, task states, default rules, and confirmation gating.
- `references/01_endnote_import.md` for importing from URLs, DOI, BibTeX, RIS, EndNote XML, PDFs, and website records.
- `references/02_metadata_validation.md` for metadata completeness, publication status, and version preference.
- `references/03_endnote_group_workflow.md` for group-scoped reading and reporting.
- `references/04_related_work_writing.md` for related-work planning and doctoral-style writing.
- `references/05_cwyw_citations.md` for real EndNote CWYW insertion and verification.
- `references/06_word_hyperlinks.md` for citation-to-bibliography navigation, DOI, and URL hyperlink checks.
- `references/07_style_learning.md` for extracting writing style from samples.
- `references/08_word_formatting.md` for Word template/style preservation.
- `references/09_quality_control.md` for final acceptance checks.
- `references/10_examples.md` for reusable task patterns.
- `references/11_existing_text_semantic_citation.md` when optimizing existing prose and inserting references according to sentence meaning.
- `references/12_ral_two_column_semantic_cwyw.md` when a journal PDF is the language/layout sample, especially for RA-L/IEEE two-column English drafts with group-scoped semantic citations.

Use templates from `templates/` when producing process artifacts. Use scripts from `scripts/` when they fit the task; do not treat script output as authoritative when EndNote or Word UI verification is required.

For RA-L/IEEE-style semantic citation work, start from `templates/ral_semantic_cwyw_task.md`. If PowerShell Word automation fails with `TYPE_E_CANTLOADLIBRARY` or `0x80029C4A`, use the tested late-bound fallback `scripts/format_ral_cwyw.vbs` instead of repeatedly retrying the broken early-bound COM path.

## Standard Workflow

### Phase 0: Interactive Task Configuration

Use `references/00_interactive_task_configuration.md` first for every task.

Required state flow:

1. `TASK_DETECTED`: identify the requested function.
2. `PARAMETERS_INCOMPLETE`: ask for missing required parameters.
3. `WAITING_FOR_CONFIRMATION`: output task configuration summary and wait.
4. `READY_TO_EXECUTE`: proceed only after explicit user confirmation.

If the user only says to use this skill or the task is unclear, ask the function-type selection question from `references/00_interactive_task_configuration.md`.

If the task is clear but missing parameters, ask only the missing items. Do not ask for already supplied information.

Do not start import, group reading, related-work drafting, CWYW insertion, Word writing, style learning, or quality modification before `READY_TO_EXECUTE`.

### Phase 1: Task Parsing

Capture:

- input source: links, DOI list, paper pages, PDFs, BibTeX, RIS, EndNote XML, EndNote library, EndNote group, Word document, style sample;
- target EndNote `.enl` library and group;
- related-work topic, research question, section titles, length, language, time span, target venue or thesis format;
- citation requirement: EndNote CWYW, Word citation, manual numbering, or no citation insertion;
- Word target file and exact insertion point;
- style source and template source;
- output constraints: page count, word count, bibliography handling, hyperlink expectations;
- existing-text citation mode: restore original sources or replace placeholders with suitable group references;
- permitted revision scope: citation-only, wording/grammar optimization, claim narrowing, sentence reordering, transition improvement, or paragraph restructuring;
- unsupported-claim policy and semantic matching granularity.

If any required item is missing, remain in `PARAMETERS_INCOMPLETE`. If all items are present, output the task configuration summary and remain in `WAITING_FOR_CONFIRMATION` until the user confirms. Proceed only in `READY_TO_EXECUTE`.

### Phase 2: Reference Import

Requires `READY_TO_EXECUTE`.

Use `references/01_endnote_import.md`.

1. Read links, DOI, paper pages, PDFs, BibTeX, RIS, EndNote XML, or website records.
2. Extract metadata without fabricating missing fields.
3. Prefer official published versions over preprints.
4. Create RIS, BibTeX, or EndNote XML import files in a build directory.
5. Import records into the requested EndNote library.
6. Add imported records to the requested EndNote group.
7. Report imported, skipped, duplicate-candidate, and incomplete records.

### Phase 3: Metadata Validation And Deduplication

Requires `READY_TO_EXECUTE`.

Use `references/02_metadata_validation.md` and scripts:

- `scripts/validate_metadata.py`
- `scripts/detect_duplicate_references.py`

For each record, validate at least:

`Author`, `Year`, `Title`, `Journal/Conference`, `Volume`, `Issue`, `Pages`, `DOI`, `URL`, `Publisher`, `Document Type`, `Abstract`, `Keywords`, `Accession Number`, and `PDF Attachment`.

Mark unresolved status explicitly:

- `[Publication status requires verification]`
- `[Metadata incomplete]`
- `[The conclusion of this paper requires manual verification]`

### Phase 4: EndNote Group Reading

Requires `READY_TO_EXECUTE`.

Use `references/03_endnote_group_workflow.md`.

When the user specifies a group:

- read only that group;
- list the number of records found;
- list missing PDF, missing abstract, incomplete metadata, duplicate candidates, and unreadable records;
- build a literature analysis table before writing;
- track which record supports which paragraph or topic;
- for existing prose, track the exact sentence or proposition supported by each record;
- list unused relevant records and explain why they were not cited.

### Phase 4.5: Existing Text Semantic Citation Mapping

Requires `READY_TO_EXECUTE`.

Use `references/11_existing_text_semantic_citation.md`.

1. Extract the confirmed Word range into sentence-level or proposition-level claim units.
2. Classify existing static citations as original-source targets or replaceable placeholders according to the confirmed configuration.
3. Build a claim-to-reference matrix from the confirmed EndNote group, including record number, evidence read, supported meaning, confidence, and proposed insertion point.
4. Select the smallest relevant citation set for each claim. Do not cite every superficially similar paper.
5. When text optimization is allowed, minimally revise wording, grammar, sentence order, transitions, scope, or claim strength so the sentence is accurate, supported, and logically connected to adjacent sentences.
6. Preserve the original paragraph purpose and report every material meaning change. Mark unsupported claims instead of forcing citations.

### Phase 5: Literature Analysis And Related Work Planning

Requires `READY_TO_EXECUTE`.

Use `references/04_related_work_writing.md` and templates:

- `templates/literature_summary_table.md`
- `templates/related_work_outline.md`

For each paper extract:

- research problem;
- core method;
- sensor or data type;
- optimization model;
- system framework;
- dataset;
- main results;
- advantages;
- limitations;
- relation to the user's work;
- recommended citation placement;
- research category.

Organize the review by method evolution or research logic, not by one paper per paragraph.

### Phase 6: Style And Formatting Learning

Requires `READY_TO_EXECUTE`.

Use:

- `references/07_style_learning.md`
- `references/08_word_formatting.md`
- `references/12_ral_two_column_semantic_cwyw.md` for RA-L/IEEE two-column samples
- `templates/style_profile.md`
- `scripts/extract_style_profile.py`

Extract style from thesis samples, Word templates, existing chapters, journal examples, or user guidelines. Follow the extracted style profile without copying source sentences.

For journal PDFs, extract prose and layout separately: inspect document metadata and laid-out text, then render at least the first page, a representative body page, and a reference page. Record stable measurements such as paper size, margins, column gap, body/reference font sizes, heading treatment, citation form, and paragraph rhythm. Do not infer the entire template from the title page alone.

Inspect the target Word document before writing. Reuse existing styles such as heading styles, body text, bibliography style, caption style, and table style. Do not type fake numbered headings when Word numbering controls section numbers.

### Phase 7: Draft Generation

Requires `READY_TO_EXECUTE`.

Write formal academic prose:

- synthesize method families and technical evolution;
- compare mechanisms, assumptions, advantages, and limitations;
- connect literature gaps to the user's research;
- place citations next to the claims they support;
- avoid unsupported broad claims, exaggerated contribution language, and repetitive templates;
- when editing existing prose, prefer local revisions over full rewrites unless the user approved restructuring;
- improve antecedents, transition phrases, sentence order, and claim progression when needed for continuity;
- keep each citation adjacent to the precise clause or sentence it supports.

### Phase 8: Word Writing And EndNote CWYW Formatting

Requires `READY_TO_EXECUTE`.

Use `references/05_cwyw_citations.md`.

1. Build temporary EndNote citations only from real EndNote records, preferably `{Author, Year #RecordNumber}`.
2. For existing text, replace static markers only after the claim-to-reference mapping is complete and verified against the confirmed group scope.
3. Preflight the selected records for rendered author names, year, title, and source. Exclude malformed records or repair them in EndNote before citation insertion; do not discover obvious metadata corruption only after overwriting the target.
4. Create a timestamped backup and a separate working copy. Never test the first CWYW conversion on the only target copy.
5. Insert content into the confirmed Word location using existing styles.
6. Run Word's EndNote CWYW formatting macro, usually `Project.EndNoteMacros.ENFormatBibliography`.
7. Confirm that the bibliography was generated by EndNote before applying final page, column, font, and paragraph formatting. Never replace the field result with a manually typed bibliography.
8. Update table of contents if needed.
9. Validate the working copy and only then replace the confirmed target file.

For RA-L/IEEE-style output, keep citation results inline rather than superscript, use a real EndNote numeric output style, and apply two-column formatting after successful CWYW conversion. See `references/12_ral_two_column_semantic_cwyw.md` for exact sequencing, measurements, automation fallback, and acceptance gates.

### Phase 9: Hyperlink And Field Verification

Use state `QUALITY_CHECK`.

Use:

- `references/05_cwyw_citations.md`
- `references/06_word_hyperlinks.md`
- `scripts/check_citations.py`

Verify:

- `ADDIN EN.CITE` exists in `word/document.xml`;
- `ADDIN EN.REFLIST` exists when a bibliography is generated;
- temporary citations like `{... #123}` do not remain;
- Word reports EndNote fields;
- citation-to-reference navigation exists if requested;
- DOI and URL are clickable when the output style requires them;
- EndNote fields are preserved.

### Phase 10: Quality Control

Use state `QUALITY_CHECK`.

Use `references/09_quality_control.md` and `templates/citation_checklist.md`.

Before final response, confirm:

- task configuration was confirmed before execution;
- imported and analyzed record counts;
- group scope was followed;
- metadata status and duplicate candidates were reported;
- related work has a coherent structure, not paper-by-paper summaries;
- citations match claims;
- existing-text revisions stay within the confirmed optimization scope and improve local logical continuity;
- unsupported claims were revised with permission or reported without forced citations;
- no unverified conclusions are stated as facts;
- Word formatting and insertion point are correct;
- EndNote fields and bibliography are real and updateable;
- links are checked or unresolved links are reported;
- output files, backups, import files, logs, and validation artifacts are in a build directory.

For rewritten journal drafts, also confirm the staged acceptance gates from `references/12_ral_two_column_semantic_cwyw.md`: metadata preflight, semantic mapping, EndNote field integrity, OOXML layout checks, rendered-text audit, visual PDF audit, Word read-only open, and final-copy hash match.

## Build Directory Discipline

For each task, create a build directory near the user project, for example:

`build/endnote-helper/<timestamp>/`

Use it for:

- imported RIS/BibTeX/XML files;
- extracted metadata tables;
- literature analysis tables;
- sentence-to-reference semantic mapping tables;
- existing-text revision logs;
- style profiles;
- Word backups;
- validation logs;
- temporary scripts or script outputs.

Keep the project root clean.

## Error Handling

Do not guess. Report clearly when:

- a webpage cannot be accessed;
- DOI cannot be resolved;
- formal publication status cannot be confirmed;
- EndNote library or group cannot be opened;
- group membership cannot be verified;
- PDF is missing or unreadable;
- EndNote plugin is unavailable;
- Word document is locked or read-only;
- CWYW fields cannot be created;
- hyperlinks cannot be generated;
- citations and references do not match.

Report:

1. failing step;
2. probable cause;
3. completed work;
4. incomplete work;
5. needed user action.

## Final Response Requirements

Keep the final report concise and include:

- target files changed or created;
- EndNote library/group used;
- records imported, skipped, incomplete, duplicate candidates;
- related-work output location and length;
- Word/EndNote field verification counts;
- DOI/URL hyperlink status;
- formatting/page-count checks when requested;
- limitations that remain.
