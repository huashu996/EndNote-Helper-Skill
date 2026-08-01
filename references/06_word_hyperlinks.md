# Word Hyperlinks

## Link Layers

EndNote CWYW fields and Word navigation hyperlinks are different:

- `ADDIN EN.CITE` keeps citation data connected to EndNote.
- citation-number-to-bibliography jumps require Word hyperlinks/bookmarks or output-style support.
- DOI and URL links in the bibliography are separate hyperlink requirements.

## Required Checks When Requested

Inspect `word/document.xml` for:

- `<w:hyperlink`;
- `<w:bookmarkStart`;
- `w:anchor=`;
- DOI text or `https://doi.org/`;
- URL text.

## DOI And URL Rules

- Prefer DOI permanent links: `https://doi.org/<doi>`.
- If no DOI exists, use the official publisher page.
- Avoid temporary download URLs, redirect-only links, search result links, and irrelevant pages.
- Do not show duplicate DOI and URL unless the output style requires both.
- If DOI or URL is absent in the rendered bibliography, fix EndNote fields or output style and rerun CWYW formatting.

## Post-Processing Warning

If EndNote does not generate citation-number hyperlinks, a post-processing step can add bookmarks and internal hyperlinks only after final EndNote formatting. Warn the user that future EndNote updates may remove those post-processed links.

