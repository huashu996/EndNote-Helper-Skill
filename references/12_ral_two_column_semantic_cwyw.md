# RA-L / IEEE Two-Column Semantic CWYW Workflow

Use this reference when the user provides an existing Word draft, an EndNote group, and a journal PDF such as an IEEE Robotics and Automation Letters paper as the prose/layout sample.

## Contents

1. Scope and prerequisites
2. Confirmation contract
3. Build artifacts
4. Target and style preflight
5. Group and metadata preflight
6. Claim decomposition and evidence mapping
7. RA-L prose reconstruction
8. Temporary citation construction
9. Safe Word/EndNote execution
10. RA-L layout profile
11. EndNote output-style rules
12. Automation fallback
13. Staged validation gates
14. Failure handling
15. Final reporting

## 1. Scope and prerequisites

Apply this workflow only after the task reaches `READY_TO_EXECUTE`.

Required confirmed inputs:

- target `.docx` and overwrite policy;
- target EndNote `.enl` library;
- exact group path;
- style sample PDF;
- language;
- permitted revision scope;
- whether existing static numbers are placeholders or original-source targets;
- whether exact IEEE/RA-L numeric output is required;
- whether headers, footers, and page numbers may be replaced by the learned journal layout.

Use only records verified as members of the confirmed group. A paper that exists elsewhere in the library is out of scope unless the user expands the scope.

## 2. Confirmation contract

The configuration summary must state:

- `semantic unit`: sentence or proposition;
- `citation policy`: smallest sufficient citation set;
- `text policy`: citation-only, local optimization, or paragraph restructuring;
- `unsupported claim policy`;
- `layout source`: sample PDF and which attributes will be learned;
- `CWYW policy`: real fields, no manual bibliography;
- `backup policy`: timestamped backup plus working copy;
- `validation policy`: field, XML, rendered text, visual PDF, and Word-open checks.

Do not assume that “format like this PDF” authorizes deleting existing headers, footers, or page numbers. Confirm that separately or preserve them.

## 3. Build artifacts

Create `build/endnote-helper/<timestamp>/` and keep these artifacts when applicable:

- `target.before.docx` — immutable backup;
- `target.working.docx` — file used for conversion and formatting;
- `style_profile.md` — prose and layout traits learned from the PDF;
- `sentence_reference_mapping.md` — claim-to-record evidence table;
- `revision_log.md` — material wording and logic changes;
- `metadata_exclusions.md` — records rejected because of malformed or incomplete metadata;
- `preview.pdf` and rendered page images;
- `preview.txt` — laid-out PDF text for reference audit;
- `citation_check.md` — field/link check output;
- automation scripts and logs.

Use `templates/ral_semantic_cwyw_task.md` as the artifact skeleton.

## 4. Target and style preflight

### 4.1 Audit the current Word file

Before rewriting, record:

- modified time and size;
- paragraph text and paragraph count;
- existing `ADDIN EN.CITE` count;
- existing `ADDIN EN.REFLIST` count;
- temporary citation count;
- whether visible numbers such as `[12]` are plain text;
- page size, margins, columns, section count, headers, footers, and page numbers;
- whether a References section already exists.

Never rely on an earlier audit if the file modification time changed. The current file may have been replaced during the conversation.

### 4.2 Learn from the PDF in three views

Use all three:

1. Document metadata: paper size, page count, producer.
2. Laid-out text: column order, headings, citations, paragraph rhythm, reference form.
3. Rendered pages: first page, representative body page, reference page.

Suggested read-only commands:

```text
pdfinfo sample.pdf
pdftotext -layout -enc UTF-8 sample.pdf sample-layout.txt
pdftoppm -png -f <page> -singlefile -r 150 sample.pdf page
```

Separate the resulting style profile into:

- `rhetorical profile`: background → limitation → gap → response, claim density, transitions, method contrasts;
- `visual profile`: paper size, margins, columns, fonts, spacing, headings, citations, references;
- `non-transferable content`: title block, author affiliations, received dates, copyright footer, manuscript-specific figures.

Do not copy sample sentences.

## 5. Group and metadata preflight

### 5.1 Verify group membership first

List exact group record numbers before selecting citations. Store the list in the mapping artifact.

### 5.2 Read evidence at the required level

For each candidate claim, inspect at least the title and abstract. Inspect introduction or method text when the claim concerns implementation details, limitations, sensor assumptions, or evaluation scope.

### 5.3 Preflight rendered metadata

Before inserting a record, check:

- author parsing and order;
- year;
- title;
- journal/conference;
- volume/issue/pages;
- DOI when available;
- document type.

Pay special attention to raw author strings containing commas plus prose conjunctions, because EndNote may render them as malformed initials. If a generated bibliography displays an impossible author string, exclude the record or repair it in EndNote and rerun CWYW.

Do not manually rewrite the bibliography field result to hide a bad record.

### 5.4 Avoid accidental self-citation

If the draft describes the same paper that also appears as a record in the group, use that record only as a factual cross-check. Do not cite the paper inside its own introduction unless the document is a thesis or later work where self-citation is contextually correct.

## 6. Claim decomposition and evidence mapping

Split the draft into proposition-level units. A sentence may contain multiple independently citable claims.

For each unit record:

- original wording;
- proposed wording;
- paragraph role;
- candidate record numbers;
- evidence read;
- exact supported meaning;
- unsupported portion;
- confidence;
- action: cite, narrow, reorder, bridge, retain as author contribution, or report unsupported.

Use the smallest sufficient citation set. Prefer one direct source over several keyword-similar sources.

Recommended paragraph roles for a compact RA-L introduction:

1. Define the technical problem and importance.
2. Explain the hardware or methodological dependency.
3. Compare generalization-oriented alternatives and remaining limitations.
4. State the gap and the present work's response.

Do not cite author-contribution sentences merely to increase citation count.

## 7. RA-L prose reconstruction

Use compact English and a visible argument chain:

`background → representative mechanism → limitation → alternative → residual gap → proposed response`

Apply these rules:

- open with a precise technical role, not “has attracted extensive attention”;
- define acronyms once;
- keep one main assertion per sentence where practical;
- place citations adjacent to the supported clause;
- use method names only when they advance the comparison;
- distinguish demonstrated capability from intended design;
- prefer cautious verbs: `supports`, `targets`, `reports`, `is intended to`;
- avoid unsupported universals such as `independent of all environments`;
- repair spacing, hyphenation, capitalization, and sentence fragments;
- preserve the draft's core contribution and terminology.

When an existing paragraph is copied from a published paper, reconstruct the logic in original wording rather than performing superficial synonym replacement.

## 8. Temporary citation construction

Use real EndNote record numbers:

```text
{AuthorA, YearA #RecordNumberA}
{AuthorA, YearA #RecordNumberA; AuthorB, YearB #RecordNumberB}
```

These are syntax placeholders only. Replace all values with verified metadata and numeric record numbers from the confirmed library; never insert the placeholders as real citations.

Rules:

- verify every record number against the group membership list;
- use the primary author's family name and stored year;
- keep repeated sources as repeated temporary citations where the claims recur;
- do not manually assign final numeric labels;
- treat final numbers as EndNote output, not author input.

## 9. Safe Word/EndNote execution

Use this sequence:

1. Create timestamped backup.
2. Create a separate working copy.
3. Insert revised prose and temporary citations into the working copy.
4. Save the working copy.
5. Run `Project.EndNoteMacros.ENFormatBibliography` or the available equivalent.
6. Wait for EndNote formatting to finish.
7. Reject the result if any `{... #123}` citation remains.
8. Count display citation fields and the bibliography field.
9. Audit the generated bibliography text for metadata corruption.
10. Apply final journal layout without replacing any EndNote field result.
11. Export a PDF preview.
12. Validate the working copy.
13. Copy it over the target only after every required gate passes.

When replacing an existing range that contains fields, replace only the confirmed target range. Never run `ENRemoveFieldCodes` as a shortcut.

## 10. RA-L layout profile

Use measurements from the supplied sample whenever possible. The following values are a reusable starting profile learned from an IEEE RA-L sample, not a universal mandate:

| Property | Starting value |
|---|---:|
| Paper | US Letter, 612 × 792 pt |
| Orientation | Portrait |
| Top margin | 54 pt |
| Bottom margin | 45 pt |
| Left/right margins | 48 pt |
| Columns | Two equal columns |
| Column gap | 18 pt |
| Body font | Times New Roman, 10 pt |
| Body alignment | Justified |
| Body spacing | Single, 0 pt before/after |
| First-line indent | 12 pt |
| Section heading | Centered, 10 pt, uppercase/small caps |
| Reference font | Times New Roman, 8 pt |
| Reference indent | 14 pt hanging |
| Citation position | Inline, not superscript |

Preserve existing headers and footers unless the confirmed task authorizes replacing them. A sample PDF may contain publisher material that should not be recreated.

For a short excerpt, references may continue on a second page with unused space. Do not shrink text merely to force a one-page result unless the user requires an exact page count.

## 11. EndNote output-style rules

- Select a real IEEE/RA-L-compatible EndNote output style before formatting when exact punctuation is required.
- Accept final numbering only from EndNote.
- Do not manually change `[1, 2]` into `[1], [2]` inside field results; correct the EndNote output style instead.
- Do not hand-type reference entries.
- A DOI may exist inside EndNote field data even if the selected output style does not display it. Report display and embedded metadata separately.
- Re-run the bibliography after metadata repairs or record substitutions.

## 12. Automation fallback

### 12.1 Preferred path

Use background Word automation only when it does not require the user to stop normal input. Keep Word hidden and alerts disabled.

### 12.2 Early-bound COM failure

PowerShell Word COM may fail with:

```text
TYPE_E_CANTLOADLIBRARY
0x80029C4A
Unable to cast ... Word.ApplicationClass ... _Application
```

After one clean retry, stop repeating the same early-bound method. Verify and terminate only orphaned hidden Word processes created by the task. Never terminate a user-visible Word process or one with unsaved work.

Use the late-bound VBScript fallback:

```text
cscript.exe //nologo scripts/format_ral_cwyw.vbs working.docx preview.pdf 12
```

Late binding uses `CreateObject("Word.Application")` and can work even when the Office type library is unavailable to PowerShell.

Run Word `/regserver` only with explicit user approval and only when late binding is also unavailable or broader Office repair is required. It is not the first fallback.

### 12.3 Background-operation rule

Prefer one batched write/format/validate pass. Do literature collection and mapping before controlling Word so the computer is not occupied while no text is being entered.

## 13. Staged validation gates

All applicable gates must pass before target replacement.

### Gate A — semantic and metadata

- every citation maps to an analyzed group record;
- no unsupported claim is forced;
- no malformed rendered author/title/source;
- no accidental self-citation.

### Gate B — EndNote field integrity

- `ADDIN EN.CITE` count matches citation locations;
- one `ADDIN EN.REFLIST` exists when required;
- temporary citation count is zero;
- bibliography entries come from the field result.

### Gate C — OOXML layout

- two-column section exists;
- paper size matches the sample;
- margins and column gap are present;
- citation results are not superscript.

### Gate D — rendered-text audit

Convert the preview to laid-out text and check:

- introduction order and paragraph continuity;
- final citation sequence;
- reference count;
- author names and source metadata;
- no visible temporary citations or duplicated References heading.

### Gate E — visual audit

Render every page or at least all pages changed. Check:

- column balance and overflow;
- heading placement;
- orphaned headings;
- reference hanging indents;
- unexpected headers, footers, or page numbers;
- excessive whitespace caused by accidental breaks.

### Gate F — native Word open

Open the final working copy read-only in Word and report:

- page count;
- paragraph count;
- total Word field count;
- display citation field count;
- bibliography field count.

### Gate G — final copy

After copying the validated working file to the target, compare file hashes. Re-run the field check on the target when risk is high.

## 14. Failure handling

If a gate fails:

- keep the original target untouched;
- retain the working copy and logs;
- fix the earliest failing layer rather than patching visible output;
- rerun all downstream gates.

Examples:

- malformed author in bibliography → repair/exclude the EndNote record, rebuild citations, rerun all gates;
- temporary citation remains → fix record identity or macro execution, do not convert it to static text;
- wrong citation punctuation → change EndNote output style, do not edit field results;
- layout correct in XML but visually poor → adjust layout, export, and rerun visual plus Word-open checks;
- PowerShell COM type-library error → switch to late-bound script.

## 15. Final reporting

Report concisely:

- target file and backup;
- library and exact group;
- number of unique cited records and citation locations;
- real `EN.CITE` and `EN.REFLIST` counts;
- temporary citation count;
- page/column/paper checks;
- records excluded for metadata reasons;
- preview and semantic-mapping artifacts;
- remaining output-style or hyperlink limitations.
