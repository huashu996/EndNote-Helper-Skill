# EndNote CWYW Citations

## Goal

Create real, updateable EndNote Cite While You Write fields in Microsoft Word. Plain text citations, static bibliography entries, or manually typed `[1]` references are not acceptable.

## Preconditions

- EndNote is installed and the target library is open.
- Word has EndNote CWYW add-in available.
- Target `.docx` is writable.
- EndNote record numbers are known or can be resolved.

## Temporary Citation Format

Use:

`{FirstAuthorSurname, Year #RecordNumber}`

For multiple references:

`{AuthorA, YearA #RecordNumberA; AuthorB, YearB #RecordNumberB}`

For corporate website authors:

`{CorporateAuthor, Year #RecordNumber}`

These are syntax placeholders, not real citations. Obtain every author, year, and numeric record number from the confirmed library before use. The record number identifies a record within that library; it is not a portable identifier across libraries. Author/year text helps EndNote disambiguate.

## Existing Text Insertion

Before replacing static citations in existing prose:

1. confirm whether static numbers represent original sources or replaceable placeholders;
2. complete a sentence-to-reference semantic mapping within the confirmed EndNote group;
3. make only the text and logic revisions the user allowed;
4. preserve coherent progression between the preceding claim, cited evidence, and following conclusion;
5. place each temporary citation beside the precise supported claim;
6. keep unsupported claims uncited and reported instead of inserting loosely related references.

Do not assume that `[1]` corresponds to group record 1 or to the first displayed EndNote item.

## Formatting

Run the EndNote formatting macro through Word, commonly:

- `Project.EndNoteMacros.ENFormatBibliography`
- `EndNoteMacros.ENFormatBibliography`
- `ENFormatBibliography`

After formatting, update tables of contents if needed and save.

## Verification

Inspect `word/document.xml`:

- require `ADDIN EN.CITE`;
- require `ADDIN EN.REFLIST` when bibliography is generated;
- require zero remaining temporary citations such as `{... #123}`;
- count Word fields through Word COM when possible.

Do not claim linked EndNote citations exist unless these checks pass.

## Prohibited Actions

- Do not use `ENRemoveFieldCodes` unless explicitly requested.
- Do not type bibliography manually.
- Do not manually renumber references.
- Do not replace EndNote fields with static hyperlinks.
