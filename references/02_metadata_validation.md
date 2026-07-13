# Metadata Validation

## Required Fields

Check each record for:

- `Author`
- `Year`
- `Title`
- `Journal` or `Conference`
- `Volume`
- `Issue`
- `Pages`
- `DOI`
- `URL`
- `Publisher`
- `Document Type`
- `Abstract`
- `Keywords`
- `Accession Number`
- `PDF Attachment`

Not every document type needs every field. For example, a conference paper may not have issue, and a website may not have pages. Mark missing fields according to document type rather than blindly failing every empty field.

## Status Labels

Use exact labels:

- `[Publication status requires verification]`
- `[Metadata incomplete]`
- `[The conclusion of this paper requires manual verification]`

## Version Preference

For same or near-same papers, default preference is:

1. formal journal version with volume/issue/pages;
2. formal conference version;
3. publisher early access;
4. accepted manuscript;
5. arXiv/preprint;
6. author homepage PDF;
7. temporary download page or search result.

Do not delete lower-priority versions automatically. Report candidates.

## Duplicate Signals

- Same DOI: high-confidence duplicate.
- Same title after normalization: high-confidence duplicate.
- High title similarity and overlapping author/year: candidate duplicate.
- Same arXiv ID and similar title: candidate duplicate.
- Same PDF filename only: weak signal.

## Validation Output

For each record report:

- record number if known;
- title;
- DOI;
- document type;
- missing fields;
- publication status;
- duplicate candidate IDs;
- recommended action.
