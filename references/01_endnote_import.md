# EndNote Import Workflow

## Supported Inputs

- paper URLs, DOI lists, publisher pages, Google Scholar snippets, IEEE Xplore, Springer, Elsevier, Wiley, ACM, arXiv, author pages;
- BibTeX, RIS, EndNote XML;
- PDF files and folders;
- text files, CSV, Excel-like exports, or pasted lists containing many links or identifiers;
- official webpages, standards pages, government notices, and online documentation.

## Import Principles

- Prefer official publisher metadata and DOI metadata over partial webpage text.
- Prefer formally published records over arXiv/preprint/author-page PDF records.
- Do not fabricate missing volume, issue, pages, DOI, URL, abstract, keywords, or publisher.
- If publication status cannot be confirmed, mark `[Publication status requires verification]`.
- If required fields are missing, mark `[Metadata incomplete]`.
- Do not import duplicates silently.

## Procedure

1. Create `build/endnote-helper/<timestamp>/`.
2. Normalize input into a source table with: `source_id`, `source_type`, `input`, `notes`.
3. For DOI input, resolve DOI metadata when network access is allowed; otherwise mark it for verification.
4. For publisher pages, extract title, authors, venue, year, DOI, URL, abstract, and publication status.
5. For PDFs, extract title/first page metadata when possible, but verify against DOI/publisher metadata before import.
6. Generate an import file: RIS by default, BibTeX or EndNote XML if requested.
7. Import into the target EndNote library.
8. Add imported records to the requested group.
9. Confirm assigned EndNote record numbers after import.
10. Produce an import report with successful imports, skipped items, incomplete records, and duplicate candidates.

## Website Records

For web sources, capture at least:

- corporate author or author;
- year;
- title;
- website/publisher;
- publication or update date;
- URL;
- access date;
- document type.

Use `Web Page` for ordinary web pages and `Government Document`, `Report`, or `Standard` when the output style handles those types correctly.

For Chinese corporate authors, use a trailing comma in EndNote when needed to prevent name inversion, for example:

`工业和信息化部,`

## Minimal RIS Example For A Website

```ris
TY  - ELEC
AU  - 工业和信息化部,
PY  - 2024
TI  - 四部门关于印发《通用航空装备创新应用方案（2024-2030年）》的通知
T2  - 工业和信息化部
PB  - 工业和信息化部
DA  - 2024/03/27
UR  - https://example.gov.cn/page.html
Y2  - 2026/07/13
ER  -
```

## Output Artifacts

- `sources.csv`
- `import_candidates.ris` or equivalent
- `import_report.md`
- `metadata_validation.json`
- `duplicate_candidates.json`
