# EndNote Helper Skill

**An academic writing skill for EndNote reference management, evidence-based literature reviews, and editable Word citations.**

[中文说明](README.zh-CN.md) · [Skill instructions](SKILL.md) · [Task examples](references/10_examples.md)

EndNote Helper Skill guides an AI coding/writing agent through reference import, metadata review, group-scoped reading, literature synthesis, and real EndNote **Cite While You Write (CWYW)** citation insertion. It also covers semantic citation matching for existing drafts and sample-guided Word formatting.

The repository contains **agent instructions, reusable templates, four offline Python helpers, and one optional Windows automation script**. Installing it does not install EndNote, provide a native EndNote connector, or automatically grant access to Word or a reference library.

## Contents

- [Capabilities](#capabilities)
- [Requirements](#requirements)
- [Installation and updates](#installation-and-updates)
- [Quick start](#quick-start)
- [Workflow and expected deliverables](#workflow-and-expected-deliverables)
- [Command-line helpers](#command-line-helpers)
- [Advanced: Word/CWYW two-column formatting](#advanced-wordcwyw-two-column-formatting)
- [Verification](#verification)
- [Troubleshooting](#troubleshooting)
- [Privacy and local data](#privacy-and-local-data)
- [Repository guide](#repository-guide)
- [Contributing](#contributing)

## Capabilities

| Task | What the workflow does | What it needs |
|---|---|---|
| Reference import | Prepare verified metadata and import files; import into the selected library/group and check assigned record numbers | Source access and an available EndNote interaction method |
| Metadata review | Flag incomplete metadata, uncertain publication status, and candidate duplicates | Exported records; publisher/DOI access for authoritative verification |
| Group-scoped review | Read only the permitted group and build a literature evidence table | Verifiable group membership and source text/abstracts |
| Related-work writing | Synthesize method families, relationships, evidence, and limitations | Topic, outline, length, language, evidence, and target document |
| Existing-draft citations | Match each claim to supporting records; make only permitted local revisions | Confirmed revision scope and citation mode |
| Word/CWYW insertion | Insert real EndNote citations and generate an updateable bibliography | Desktop Word, EndNote, working CWYW add-in, and application access |
| Style learning | Analyze prose and inspect a sample's layout; apply relevant traits | User-provided sample and suitable document/PDF inspection tools |
| Link checks | Distinguish EndNote fields, citation navigation, and DOI/URL links | DOCX inspection plus Word/rendered-output verification |

The helpers cover only parts of this workflow. They do not select papers semantically, import a library, read PDFs, or write a complete literature review by themselves.

## Requirements

### Agent-guided work

- An agent environment that can load `SKILL.md` and read the selected project files.
- Network access when checking publisher pages or DOI metadata. Mark inaccessible sources as unresolved.
- EndNote and Microsoft Word with the CWYW add-in for native citation insertion. The agent needs a supported way to interact with those applications; this repository does not supply a general-purpose EndNote API bridge.
- PDF extraction/rendering tools when learning a journal PDF or inspecting page layout. No PDF renderer is bundled here.

The optional `.vbs` automation targets **Windows desktop Word and EndNote**. Full application automation on macOS/Linux is not provided. Word for the web is not a substitute for the COM automation used by that script.

### Offline helpers

- **Python 3.10 or newer** is the recommended baseline.
- All four Python helpers use the standard library; no `pip install` step is required.
- Keep `detect_duplicate_references.py` beside `validate_metadata.py`, which it imports.
- Git is needed only for cloning/updating. A downloaded repository ZIP can also be extracted into the skill directory.

## Installation and updates

### Install in Codex on Windows

Run in PowerShell. This honors an existing `CODEX_HOME` value without changing it:

```powershell
$codexRoot = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE '.codex' }
$skillRoot = Join-Path $codexRoot 'skills'
New-Item -ItemType Directory -Path $skillRoot -Force | Out-Null
git clone https://github.com/huashu996/EndNote-Helper-Skill.git (Join-Path $skillRoot 'endnote-helper-skill')
```

The resulting folder must contain `SKILL.md`, `references/`, `scripts/`, and `templates/`. Keep only one installed copy of this skill to avoid ambiguous discovery. Start a new session or reload the skill list if the current session does not discover it.

For another compatible agent, place the same folder in that agent's documented skill location. The repository is self-contained; the historical `related-work` and `endnote-cwyw-citations` names mentioned in `SKILL.md` are not extra installation dependencies.

### Update an existing installation

```powershell
$installedSkill = Join-Path $skillRoot 'endnote-helper-skill'
git -C $installedSkill status --short
git -C $installedSkill pull --ff-only
```

Run this after setting `$skillRoot` as above. If you installed under another folder name, use that exact path. Review and preserve local edits before updating; do not reset a customized installation just to make a pull succeed. ZIP installations must be updated manually after backing up local changes.

## Quick start

Invoke the skill with `$endnote-helper-skill`. File, group, and author labels below are placeholders, not a real research library.

### 1. Import and organize references

```text
Use $endnote-helper-skill to inspect the sources in links.txt and prepare a RIS
import for the library I select, under the existing group "Example Group".
Prefer verified published versions. Report possible duplicates without deleting
or merging them. Do not download PDFs. Save the import file and audit report
under build/endnote-helper/. Ask for the missing library path before proceeding.
```

### 2. Write a literature review

```text
Use $endnote-helper-skill with my selected library and "Example Group" only.
Write an English related-work section of about 1,200 words in manuscript.docx.
Organize it by research problem, method families, evidence, and limitations.
Use the existing document styles and the selected numeric EndNote output style.
Insert real CWYW citations. Save as manuscript_reviewed.docx and keep the source.
Confirm the topic and exact insertion range before execution.
```

### 3. Add suitable citations to existing prose

```text
Use $endnote-helper-skill to review the Introduction in manuscript.docx using
only "Example Group" in the selected library. Treat existing static citation
numbers as placeholders and use GROUP_SEMANTIC_REPLACE mode. Permit grammar
corrections and small claim-narrowing edits, but preserve the research argument.
Report claims that the group cannot support. Create a sentence-to-reference
matrix, insert real CWYW fields, and save a separate reviewed copy.
```

### 4. Learn a sample's prose and layout

```text
Use $endnote-helper-skill to analyze the body and reference pages of sample.pdf.
Record prose structure, citation patterns, page dimensions, margins, columns,
and typography. Apply the agreed profile to a working copy of manuscript.docx,
preserving verified CWYW fields. Report any layout feature that cannot be verified.
```

For actual EndNote/Word tasks, the skill collects missing parameters and presents a task summary before execution. It should not ask again for information already supplied. Review the selected library, group, revision scope, output style, target range, and save policy before confirming. See [task configuration](references/00_interactive_task_configuration.md).

## Workflow and expected deliverables

1. **Configure the task:** capture inputs, permitted sources, output location, and modification scope.
2. **Verify references:** establish group membership, metadata, publication status, and duplicate candidates.
3. **Read evidence:** build a literature table and a claim-to-record mapping. A title match alone does not support a detailed experimental claim.
4. **Draft or revise:** organize the argument, keep citations beside the supported claim, and log material semantic changes.
5. **Create citations:** use actual record numbers from the confirmed library, then run EndNote formatting on a backed-up working copy.
6. **Inspect the result:** check fields, bibliography, source scope, hyperlinks where requested, and the rendered document before replacing an authorized target.

Existing-draft citation modes:

| Mode | Meaning |
|---|---|
| `RESTORE_ORIGINAL` | Identify and restore the original sources intended by existing markers |
| `GROUP_SEMANTIC_REPLACE` | Replace static placeholders with suitable records from the confirmed group |
| `ADD_TO_UNCITED_TEXT` | Add evidence-backed citations to uncited prose |

`[1]` is not EndNote record 1. Record numbers are library-specific. A grouped citation can contain several records while remaining one Word citation field.

Typical task outputs belong in `build/endnote-helper/<timestamp>/` near the user's project:

| Artifact | Purpose |
|---|---|
| Import file and import report | Record successful imports, skips, unresolved metadata, and assigned records |
| Literature table | Summarize methods, evidence, limitations, and source scope |
| Claim-to-reference matrix | Explain which source supports each claim and what was read |
| Revision log and style profile | Record meaningful edits and measured/sample-derived formatting |
| Working/reviewed DOCX and backup | Preserve the original while validating editable citations |
| Validation reports and PDF preview | Record checks, unresolved items, and rendered appearance |

These are workflow deliverables, not files automatically created by every helper command.

## Command-line helpers

Run the examples below **from the repository root**, substituting your local inputs. Output paths are illustrative. The Python helpers read their inputs and write reports; they do not edit EndNote libraries or the source DOCX. Each supports `--help` and `-o`/`--output`; without `-o`, it prints its report.

All four return **0 when report generation succeeds and 2 on handled errors**. Exit code 0 does not mean metadata is complete, citations are correct, or the document passes final review. Inspect the report itself.

### Validate metadata

```powershell
python scripts/validate_metadata.py records.json -o build/endnote-helper/metadata.json
```

Input support:

| Input | Details and limits |
|---|---|
| JSON | One record, an array, or an object with a `records`, `items`, or `references` array |
| CSV | UTF-8/UTF-8-BOM, comma-separated, with a header row |
| RIS | Basic tagged records; the minimal parser requires `ER  - ` including the space after the hyphen; multiline continuations are not fully handled |
| BibTeX | Basic brace/quoted fields; nested braces, macros, and complex entries are not fully parsed |

The `.tsv` extension is accepted by the code, but it currently uses the same comma parser as CSV. Convert real tab-separated input to CSV first. These helpers do **not** parse EndNote XML, `.enl`, PDFs, or Excel files; those inputs require a separate workflow/export step.

A synthetic JSON example, suitable for demonstrating the report format only:

```json
[
  {
    "record": "demo-web-1",
    "type": "webpage",
    "author": "Example Organization",
    "year": "2024",
    "title": "Demonstration web record — not a research source",
    "url": "https://example.org/reference-example",
    "access_date": "2024-02-01"
  }
]
```

Use flat string fields for predictable results. Common aliases include `authors → author`, `journal/conference/booktitle → venue`, and `record_number → record`. Author arrays are not deeply parsed.

The offline checks require title/year/author plus DOI/venue for scholarly records or URL/access date for web records. A URL without a DOI also triggers web classification. This is a completeness heuristic, not a universal bibliographic rule. The report contains `input`, `count`, and per-record `missing_fields`/`status`. Empty status does not prove that a publication exists or that its metadata is correct. Publisher verification, attachments, and the broader field checklist remain separate checks.

### Find candidate duplicates

```powershell
python scripts/detect_duplicate_references.py records.json --title-threshold 0.92 -o build/endnote-helper/duplicates.json
```

Uses the same parsers and limitations as metadata validation. Matching DOI or normalized title produces high-confidence candidates. Similar titles with matching year or first author produce medium-confidence candidates. Output includes `duplicate_candidates`, record IDs, titles, confidence, and reasons.

This is a pairwise comparison and can be slow on large libraries. It reports candidates only; it never merges or deletes records. Journal extensions and conference versions still require human review.

### Inspect DOCX citation markers

```powershell
python scripts/check_citations.py manuscript_reviewed.docx -o build/endnote-helper/citations.json
```

Reports EndNote marker occurrences, temporary citations, hyperlink elements, bookmarks, anchor links, and DOI/URL text samples from **`word/document.xml` only**.

These are raw XML heuristics, not authoritative Word field counts: `ADDIN EN.CITE` can also match `EN.CITEDATA`, and text split across XML runs can be missed. Headers, footers, notes, and hyperlink relationship targets are not inspected. A DOI string or hyperlink count does not prove that a link is clickable, correct, or resolves successfully. Verify real fields in Word and test requested navigation separately.

### Extract a basic prose profile

```powershell
python scripts/extract_style_profile.py sample.docx -o build/endnote-helper/style.md
python scripts/extract_style_profile.py sample.txt --json -o build/endnote-helper/style.json
```

Accepts TXT, Markdown, and DOCX. Reports character counts, approximate paragraph/sentence lengths, citation density, connectors, and common terms. DOCX extraction is a rough body-text pass; it does not measure fonts, margins, columns, heading styles, or visual layout. PDF samples need separate extraction and rendered inspection. Character-based counts are not linguistic word counts.

## Advanced: Word/CWYW two-column formatting

[`scripts/format_ral_cwyw.vbs`](scripts/format_ral_cwyw.vbs) is a **specialized formatter for a disposable working copy**, including the late-bound fallback for `TYPE_E_CANTLOADLIBRARY` / `0x80029C4A` failures. It is not a general document-preservation tool or a certified journal template.

Before running it:

1. Back up the original and create a separate working DOCX. The script saves and modifies the file passed to it; it does not create a backup.
2. Open the correct EndNote library, verify real record numbers, and choose the required EndNote output style.
3. Ensure desktop Word, the CWYW add-in, and Windows Script Host/VBScript are available and permitted in your environment.
4. Use absolute input/output paths and an existing output directory. Confirm that the fixed layout below fits this task.

```powershell
$workingDoc = (Resolve-Path 'build/working.docx').Path
$previewPdf = Join-Path (Split-Path $workingDoc) 'preview.pdf'
cscript.exe //nologo scripts/format_ral_cwyw.vbs $workingDoc $previewPdf 8
```

Arguments:

| Position | Argument | Meaning |
|---|---|---|
| 1 | Working DOCX | Required; modified in place |
| 2 | Preview PDF | Required; exported after formatting |
| 3 | Expected citation fields | Optional; default 0. A positive value is a **minimum field count**, not an exact count or number of unique references |
| 4 | `clear-footers` | Optional; removes footer content. Omit to retain footers. Supply argument 3, including `0`, when using argument 4 |

The script tries `Project.EndNoteMacros.ENFormatBibliography`, `EndNoteMacros.ENFormatBibliography`, then `ENFormatBibliography`. It rejects remaining temporary citations, zero citation fields, a field count below the requested minimum, or a missing bibliography field. It does not select the library/output style, choose evidence, insert a semantic mapping, or repair metadata.

Fixed formatting applied after CWYW:

| Setting | Value |
|---|---|
| Paper | US Letter, 612 × 792 pt |
| Margins, top/bottom/left/right | 54 / 45 / 48 / 48 pt |
| Columns | Two equal columns, 18 pt gap |
| Body | Times New Roman, 10 pt, justified, single spacing |
| Body paragraph | 12 pt first-line indent; 0 pt space before/after |
| References | Times New Roman, 8 pt, 14 pt hanging indent |
| Citation results | Inline, 10 pt |

**The script changes all sections and resets body bold, italic, and superscript formatting.** A first paragraph beginning with `I.` receives special heading treatment. Complex manuscripts, equations, mixed page layouts, and documents whose existing emphasis must be preserved need a more selective workflow. Page count and official journal compliance are not guaranteed. The sample-guided workflow can use other measurements; this script does not learn them automatically.

On success it prints paths, macro used, citation/Word-field counts, paragraphs, and pages. Exit codes: `0` success, `1` for handled automation/check failures, `2` for missing required arguments; an unhandled script/runtime error may use another nonzero code. Inspect the working document after any failure; the script is not transactional.

## Verification

For a completed citation task, record evidence for all applicable checks:

- Selected records belong to the permitted library/group and support the adjacent claim.
- Metadata and publication status were verified or explicitly marked unresolved.
- Real `ADDIN EN.CITE` fields exist; `ADDIN EN.REFLIST` exists when a bibliography is required.
- Temporary citations are gone; citation locations and unique reference counts are checked separately.
- EndNote can update the working copy; requested citation navigation and DOI/URL links work.
- The intended Word range was changed and unrelated content was preserved.
- The rendered PDF has readable text, correct layout, and no clipped content.
- The original is recoverable, and the delivered copy matches the reviewed copy.

Use [the citation checklist](templates/citation_checklist.md) and [quality-control guidance](references/09_quality_control.md). If desktop validation is unavailable, report it as unverified; do not equate XML marker detection with completed CWYW integration.

## Troubleshooting

| Symptom | Next step |
|---|---|
| Skill is not discovered | Check the folder location, `SKILL.md`, and duplicate installations; start a fresh session |
| Helper cannot parse an export | Export flat JSON/CSV or basic RIS/BibTeX; check the format limitations above |
| RIS report has zero records | Check the `TY`/`ER` structure and trailing space required by the minimal `ER` parser |
| `.enl`, XML, or PDF is rejected by a Python helper | Use an appropriate export/conversion workflow; renaming the extension is insufficient |
| Library or group cannot be verified | Provide the correct library and exact group; do not expand to the whole library silently |
| EndNote formatting macro is unavailable | Verify the desktop CWYW add-in and that Word can format a citation manually |
| Temporary citations remain | Check library-specific record numbers, ambiguous authors/years, and add-in availability |
| Citation counts differ between XML and Word | Distinguish marker occurrences, actual fields, grouped citations, and unique records |
| Bibliography punctuation/DOI display is wrong | Repair metadata/output style and rerun CWYW; do not hand-edit field results |
| `TYPE_E_CANTLOADLIBRARY` / `0x80029C4A` | Consider the late-bound VBS path only when its fixed formatting is appropriate |
| VBScript is unavailable or blocked | Use an available permitted Word interaction method; do not globally disable security controls |
| Layout or emphasis changes unexpectedly | Recover the backup and use a selective formatter instead of the broad two-column preset |
| Console encoding errors with non-ASCII text | Write to `-o` or set `PYTHONIOENCODING=utf-8` for that process/session |

Do not repeatedly run a failing formatter against the original manuscript. Report the failing step, completed work, and remaining manual action.

## Privacy and local data

Public examples use generic labels and the reserved `example.org` domain. They do not represent real papers, real EndNote record IDs, or a contributor's library.

- Keep `.enl`/`.Data` libraries, manuscript files, PDFs, exports, backups, and task reports outside the published skill package.
- `.gitignore` excludes common research files, build/report directories, and credential files. It does not protect files already tracked, every possible filename, or previous Git commits.
- Helper reports can contain source paths, titles, DOI/URLs, and extracted terms. Redact them before sharing an issue or a debugging bundle.
- The offline Python helpers contain no network/upload step. Agent use of browsers, cloud services, and document tools follows that environment's permissions; local helper execution alone is not a promise that every agent workflow is offline.
- Before publishing changes, review `git diff --cached --name-only`, `git diff --cached`, and commit author/committer identity. Use synthetic test fixtures and a GitHub noreply email when appropriate.
- Do not commit API keys, access tokens, cookies, private keys, or real application logs. `.gitignore` is a convenience, not a security boundary.

## Repository guide

| File or directory | Purpose |
|---|---|
| [SKILL.md](SKILL.md) | Agent entry point, scope, workflow, and constraints |
| [00 — Task configuration](references/00_interactive_task_configuration.md) | Parameters, confirmation, and task states |
| [01 — Import](references/01_endnote_import.md) | Source verification and EndNote import procedure |
| [02 — Metadata](references/02_metadata_validation.md) | Completeness and publication/version checks |
| [03 — Groups](references/03_endnote_group_workflow.md) | Group membership and evidence scope |
| [04 — Related work](references/04_related_work_writing.md) | Literature synthesis and organization |
| [05 — CWYW](references/05_cwyw_citations.md) | Real citation fields and EndNote formatting |
| [06 — Hyperlinks](references/06_word_hyperlinks.md) | Navigation, DOI, and URL checks |
| [07 — Style learning](references/07_style_learning.md) | Prose and style profiles |
| [08 — Word formatting](references/08_word_formatting.md) | Preserve document styles and protected content |
| [09 — Quality control](references/09_quality_control.md) | Acceptance checklist |
| [10 — Examples](references/10_examples.md) | Prompts and expected task-configuration behavior |
| [11 — Semantic citations](references/11_existing_text_semantic_citation.md) | Claim-level evidence matching and limited revisions |
| [12 — Two-column workflow](references/12_ral_two_column_semantic_cwyw.md) | Sample inspection, CWYW conversion, formatting, and review |
| [scripts/](scripts/) | Four offline Python helpers and the optional VBS formatter |
| [templates/](templates/) | Literature table, review outline, style profile, citation checklist, and two-column task sheet |

## Contributing

Keep instructions portable and examples synthetic. Describe whether a change affects agent behavior, offline reports, or desktop automation. Validate changed helpers with small non-private fixtures; check Markdown links and `git diff --check`. For CWYW changes, state the actual tested Word/EndNote environment and what was verified in a working copy. Do not claim that a syntax check or mock DOCX test validates real desktop integration.

EndNote and Microsoft Word remain external products. This repository does not bundle their software, licenses, output styles, or official journal templates.
