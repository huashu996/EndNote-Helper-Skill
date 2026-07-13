# EndNote Helper Skill

[English](README.md) | [简体中文](README.zh-CN.md)

An end-to-end Agent Skill for evidence-grounded academic writing with EndNote and Microsoft Word. It can validate and organize references, analyze a confirmed EndNote group, improve an existing draft according to sentence meaning, insert real Cite While You Write (CWYW) fields, learn writing and layout conventions from samples, and verify the final Word document.

This skill is designed for workflows where citation correctness, group scope, metadata quality, Word formatting, and updateable EndNote links all matter.

## Highlights

- Import references from DOI lists, URLs, PDFs, BibTeX, RIS, EndNote XML, and web records.
- Validate metadata and report incomplete or suspicious records without inventing missing values.
- Restrict evidence selection to an explicitly confirmed EndNote group.
- Map sentence- or proposition-level claims to the smallest sufficient citation set.
- Improve grammar, claim scope, transitions, and paragraph logic while preserving the author's research intent.
- Insert real Word/EndNote `ADDIN EN.CITE` fields and an updateable `ADDIN EN.REFLIST` bibliography.
- Learn prose and formatting from thesis templates, papers, and journal PDFs without copying source sentences.
- Rebuild English drafts in an IEEE/RA-L-like two-column layout.
- Preserve unrelated Word content and refuse to remove CWYW field codes unless explicitly requested.
- Produce backups, semantic mapping tables, revision logs, previews, and validation reports.

## Safety-First Execution Model

The skill uses a mandatory confirmation gate:

1. `TASK_DETECTED` — identify the requested workflow.
2. `PARAMETERS_INCOMPLETE` — ask for all missing required information.
3. `WAITING_FOR_CONFIRMATION` — summarize the exact files, library, group, citation policy, revision scope, formatting target, and overwrite policy.
4. `READY_TO_EXECUTE` — modify files or operate EndNote/Word only after explicit confirmation.

It never treats existing static numbers such as `[1]` as a reliable mapping to EndNote records unless the user explicitly asks to restore the original sources.

## Supported Workflows

| Workflow | What the skill does |
|---|---|
| Reference import | Build import files, import records, add them to the requested group, and report skipped or incomplete records. |
| Metadata validation | Check authors, year, title, source, volume, issue, pages, DOI, URL, type, abstract, keywords, and attachments. |
| Group-scoped analysis | Read only the confirmed group and build a literature/claim evidence table before writing. |
| Related work | Synthesize method families, technical evolution, advantages, limitations, and research gaps. |
| Existing-draft optimization | Decompose prose into claims, remap placeholder citations, make permitted revisions, and improve local logic. |
| Real CWYW insertion | Convert real temporary EndNote citations into updateable Word citation and bibliography fields. |
| Style learning | Extract rhetorical and visual traits from Word documents, thesis templates, or journal PDFs. |
| RA-L/IEEE formatting | Apply a learned two-column profile after successful CWYW conversion and verify the rendered output. |
| Quality control | Inspect fields, OOXML, rendered text, PDF previews, hyperlinks, and native Word openability. |

## Requirements

### Core skill

- An agent that supports the Agent Skills format, such as Codex.
- Python 3 for the bundled metadata and citation-checking scripts.

### Real Word/EndNote CWYW workflows

- Windows.
- Microsoft Word desktop.
- EndNote desktop with the Cite While You Write add-in enabled.
- The target EndNote library and any required group/PDF attachments available locally.

### Optional style extraction

PDF layout inspection benefits from Poppler tools such as `pdfinfo`, `pdftotext`, and `pdftoppm`.

Metadata analysis and writing guidance can still be useful without Word or EndNote, but real CWYW field creation requires the desktop applications above.

## Installation

### Codex on Windows

Clone directly into the personal skills directory:

```powershell
git clone https://github.com/huashu996/EndNote-Helper-Skill.git `
  "$env:USERPROFILE\.codex\skills\EndNote-Helper-Skill"
```

To update an existing installation:

```powershell
git -C "$env:USERPROFILE\.codex\skills\EndNote-Helper-Skill" pull
```

Restart or reload the agent session if the skill is not discovered immediately.

### Other Agent Skills-compatible clients

Clone the repository into the client's configured skills directory. The repository root is the skill root and contains the required `SKILL.md`.

## Quick Start

Invoke the skill explicitly and provide the files and scope you already know:

```text
Use $endnote-helper-skill to check every reference in the current EndNote
group "Thesis / 02 LO", improve the English introduction in test.docx,
replace static citation placeholders with semantically appropriate group
references, and insert real CWYW fields. Learn the language and two-column
layout from the supplied RA-L PDF. Create a backup before overwriting.
```

Other example requests:

```text
Use $endnote-helper-skill to import this DOI list into my current library,
add the records to "Chapter 3", and report incomplete metadata and duplicates.
```

```text
Use $endnote-helper-skill to write a 2,000-word related-work section from
the EndNote group "Collaborative SLAM" and insert updateable Word citations.
```

The skill will summarize the execution configuration and wait for confirmation before changing EndNote or Word files.

## Semantic Citation Workflow

For existing prose, the skill follows this sequence:

1. Audit the current Word file instead of relying on an earlier version.
2. Verify exact group membership and candidate record numbers.
3. Split the target text into sentence- or proposition-level claims.
4. Read evidence at the level required by each claim.
5. Build a claim-to-reference matrix with confidence and support boundaries.
6. Select the smallest relevant citation set.
7. Narrow, reorder, or bridge claims only within the confirmed revision scope.
8. Record material wording changes and unsupported claims.
9. Insert real temporary EndNote citations such as `{Zhang, 2016 #69}`.
10. Run CWYW and reject the result if temporary citations remain.

The workflow avoids keyword-only citation matching, group-order matching, and forced citations for unsupported statements.

## Real CWYW, Not Static Numbers

The skill treats a Word citation as successful only when the document contains real EndNote fields:

- `ADDIN EN.CITE` for displayed citations;
- `ADDIN EN.REFLIST` for the generated bibliography;
- no remaining temporary citations such as `{Author, Year #123}`.

It does not manually type the bibliography or replace a field result to hide bad metadata. Malformed records must be repaired in EndNote or excluded and reported.

## IEEE RA-L Two-Column Workflow

When a journal PDF is supplied as a style source, the skill inspects:

- PDF metadata and paper size;
- laid-out text and column flow;
- a rendered first page;
- a representative body page;
- a reference page.

It separates transferable rhetorical/layout traits from manuscript-specific material such as title blocks, received dates, copyright notices, and figures.

The reusable RA-L starting profile includes US Letter paper, two equal columns, Times-family body text, inline numeric citations, compact centered section headings, and a smaller hanging-indent bibliography. Measurements are adjusted to the supplied sample rather than blindly imposed.

See [the detailed RA-L semantic CWYW workflow](references/12_ral_two_column_semantic_cwyw.md).

## Background Word Automation

The normal route runs Word's EndNote formatting macro in the background. If PowerShell Word automation fails with `TYPE_E_CANTLOADLIBRARY` or `0x80029C4A`, the skill can use the bundled late-bound VBScript fallback:

```powershell
cscript.exe //nologo scripts\format_ral_cwyw.vbs `
  working.docx preview.pdf 12
```

The script must be run on a disposable working copy after the required EndNote output style is selected. It does not replace the semantic mapping or metadata review stages.

## Validation Gates

Before replacing the target document, applicable workflows verify:

1. **Evidence and metadata** — claim support, group scope, rendered author/source quality, and accidental self-citation.
2. **EndNote fields** — expected citation fields, bibliography field, and zero temporary citations.
3. **OOXML layout** — page size, margins, columns, and citation formatting.
4. **Rendered text** — paragraph order, citation sequence, reference count, and bibliography metadata.
5. **Visual preview** — column flow, headings, spacing, overflow, and reference indentation.
6. **Native Word open** — pages, paragraphs, and field counts from a read-only Word open.
7. **Final copy** — file-hash match between the validated working copy and the delivered target.

## Repository Structure

```text
EndNote-Helper-Skill/
├── SKILL.md
├── README.md
├── README.zh-CN.md
├── references/
│   ├── 00_interactive_task_configuration.md
│   ├── 01_endnote_import.md
│   ├── 02_metadata_validation.md
│   ├── 03_endnote_group_workflow.md
│   ├── 04_related_work_writing.md
│   ├── 05_cwyw_citations.md
│   ├── 06_word_hyperlinks.md
│   ├── 07_style_learning.md
│   ├── 08_word_formatting.md
│   ├── 09_quality_control.md
│   ├── 10_examples.md
│   ├── 11_existing_text_semantic_citation.md
│   └── 12_ral_two_column_semantic_cwyw.md
├── scripts/
│   ├── check_citations.py
│   ├── detect_duplicate_references.py
│   ├── extract_style_profile.py
│   ├── format_ral_cwyw.vbs
│   └── validate_metadata.py
└── templates/
    ├── citation_checklist.md
    ├── literature_summary_table.md
    ├── ral_semantic_cwyw_task.md
    ├── related_work_outline.md
    └── style_profile.md
```

## Build Directory Discipline

Each task should create a local build directory such as:

```text
build/endnote-helper/<timestamp>/
```

It can contain backups, working copies, semantic mapping tables, revision logs, style profiles, previews, import files, and validation reports. These task artifacts should not be committed to this repository.

## Privacy and Safety

- Keep EndNote libraries, `.Data` folders, PDFs, Word manuscripts, and extracted notes out of the skill repository.
- Never upload user documents or reference libraries unless the user explicitly requests and understands the publication scope.
- Never invent metadata, record numbers, conclusions, DOI values, or publication status.
- Never delete or merge duplicate records silently.
- Never run `ENRemoveFieldCodes` unless the user explicitly asks to break EndNote links.
- Never terminate a user-visible Word process or discard unsaved work while recovering background automation.

## Limitations

- EndNote library internals and group metadata vary by version; UI or native application verification may still be required.
- Exact citation punctuation depends on the selected EndNote output style.
- A DOI may be embedded in EndNote field data without being displayed by the active bibliography style.
- Citation-to-reference navigation and clickable DOI/URL behavior depend on Word, EndNote, and output-style settings.
- The late-bound formatter is a recovery path, not a substitute for evidence review or metadata validation.

## Contributing

Improvements should preserve the confirmation gate, group-scoped evidence rules, real-CWYW requirement, and staged validation model. Test changes on disposable files rather than production manuscripts.
