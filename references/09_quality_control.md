# Quality Control

## Before Final Response

Confirm:

- task type was detected before execution;
- all required parameters were collected;
- task configuration summary was shown to the user;
- user explicitly confirmed before EndNote, Word, or file modification started;
- no default values were used unless explicitly allowed;
- all specified records were processed;
- group scope was followed;
- metadata validation was performed;
- publication versions were distinguished;
- duplicate candidates were listed;
- imported records were added to the correct EndNote group;
- related work has clear organization logic;
- related work is not a paper-summary collage;
- every factual claim has support;
- every inserted citation has a documented sentence- or proposition-level semantic match;
- existing static citation numbers were not mapped by group order or keyword overlap alone;
- existing-text revisions stayed within the confirmed permission and material meaning changes were logged;
- transitions and sentence order remain coherent around inserted citations;
- unsupported claims were revised with permission or reported without forced citations;
- no unverified conclusions are stated as facts;
- no references outside scope were added;
- EndNote CWYW fields remain updateable;
- bibliography matches in-text citations;
- DOI and URL links are present when required;
- citation-to-bibliography navigation was checked when requested;
- Word formatting matches the target document;
- existing fields and cross-references were preserved;
- output document opens normally.

## Required Artifacts

Keep task artifacts in `build/endnote-helper/<timestamp>/`:

- metadata table;
- duplicate report;
- literature analysis table;
- outline;
- style profile;
- Word backup;
- citation check report;
- sentence-to-reference semantic mapping table;
- existing-text revision log;
- final validation summary.

## Failure Reporting

If a required check fails, report:

1. failing check;
2. likely cause;
3. completed work;
4. incomplete work;
5. recommended next step.
