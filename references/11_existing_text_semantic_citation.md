# Existing Text Semantic Citation

Use this workflow when the user asks to optimize existing prose and insert suitable EndNote references according to sentence meaning.

## Confirmed Modes

Distinguish these modes before execution:

- `RESTORE_ORIGINAL`: identify and restore the sources intended by existing citation numbers.
- `GROUP_SEMANTIC_REPLACE`: treat existing numbers as placeholders and select suitable references from the confirmed EndNote group.
- `ADD_TO_UNCITED_TEXT`: keep supported prose and add citations where needed.

Never infer the mode from a static number alone.

## Claim Extraction

1. Read the confirmed Word range and preserve paragraph order.
2. Split compound sentences into propositions when different clauses require different evidence.
3. Classify each proposition as background fact, method description, comparison, limitation, trend, dataset/result claim, or author contribution.
4. Record existing citation markers without assuming their source identity.

## Semantic Matching

Build a mapping table with:

| Claim ID | Existing sentence or proposition | Allowed revision | Candidate record | Evidence read | Supported meaning | Confidence | Action |
|---|---|---|---|---|---|---|---|

Match by research meaning and evidence:

- use title metadata only for broad topical claims;
- use abstract or full text for method, limitation, experiment, result, or comparison claims;
- prefer direct support over loose topical similarity;
- prefer the smallest sufficient citation set;
- do not use EndNote order, PDF filename, static citation number, or keyword overlap as proof.

## Limited Text And Logic Optimization

When the user allows existing content to be modified according to available references:

- correct grammar, terminology, references, and sentence structure;
- narrow an overbroad claim to the scope supported by the selected records;
- replace an unsupported trend or application statement with a nearby claim that the group evidence supports;
- reorder adjacent sentences when the progression of background, problem, evidence, and conclusion is unclear;
- add or revise transition phrases so the citation fits the argument rather than interrupting it;
- repair pronoun reference, repeated subjects, abrupt topic shifts, and weak cause-effect links;
- preserve the paragraph's research purpose and the user's intended argument;
- avoid adding new technical conclusions, numerical results, novelty claims, or criticism not supported by the records;
- log each material semantic change as `original -> revised -> supporting record numbers`.

If no group record supports the sentence and revision would change its research intent, do not rewrite it. Mark it `[Unsupported by confirmed group]` in the process report and ask the user when necessary.

## Citation Placement

- place citations immediately after the supported clause or sentence;
- group references only when they support the same proposition;
- reuse the same EndNote record when a later sentence depends on the same source;
- do not preserve obsolete static numbering after the CWYW conversion;
- use real record numbers in temporary citations, then run EndNote formatting.

## Acceptance Checks

Verify:

- every inserted record belongs to the confirmed group unless broader scope was approved;
- each citation has documented sentence-level support;
- all text changes stay within the confirmed revision scope;
- paragraph logic remains coherent before and after each citation;
- static placeholders were removed only where replaced by real CWYW fields;
- `ADDIN EN.CITE` and, when requested, `ADDIN EN.REFLIST` exist;
- no temporary `{Author, Year #RecordNumber}` citations remain;
- unsupported claims and unused relevant records are reported.
