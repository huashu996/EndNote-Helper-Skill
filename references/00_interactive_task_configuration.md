# Interactive Task Configuration

This file is the mandatory front gate for every EndNote-Helper-Skill task.

## Highest Priority Rule

Before reading an EndNote group, generating text, inserting citations, importing references, modifying Word, or changing files, the assistant must:

1. identify the requested function;
2. check which required parameters are already provided;
3. ask only for missing required parameters;
4. ask all missing required parameters together where practical;
5. avoid repeating questions for information the user already supplied;
6. produce a task configuration summary after the user answers;
7. ask the user to confirm the summary;
8. wait for an explicit confirmation such as `确认执行`;
9. execute only after confirmation.

Do not treat recommended defaults as confirmed user requirements. Defaults are allowed only when the user explicitly says `使用默认设置直接执行`, `按默认设置执行`, or equivalent. Even then, output the task configuration summary before execution.

## Task States

- `TASK_DETECTED`: requested function identified.
- `PARAMETERS_INCOMPLETE`: required parameters are missing; ask questions.
- `WAITING_FOR_CONFIRMATION`: task summary has been produced; wait for confirmation.
- `READY_TO_EXECUTE`: user confirmed the task summary.
- `EXECUTING`: confirmed task is running.
- `QUALITY_CHECK`: output is being verified.
- `COMPLETED`: task is complete.
- `BLOCKED`: required file, EndNote group, Word/EndNote capability, or user decision is unavailable.

Only `READY_TO_EXECUTE` permits modifying EndNote, reading a specified group, generating final prose, inserting CWYW citations, or writing Word files.

## Function Type Selection

If the task is unclear or the user only says to use the skill, ask:

```text
请选择本次需要执行的功能：

A. 将网页、DOI、PDF 或文献文件导入 EndNote
B. 整理或检查指定 EndNote Group
C. 基于指定 EndNote Group 撰写 Related Work
D. 为已有正文补充 EndNote 引用
E. 学习指定论文或 Word 文档的写作风格
F. 学习并复用 Word 排版格式
G. 修改已有 Related Work
H. 检查正文引用、参考文献和超链接
I. 执行以上多个功能的组合工作流
```

Then ask the parameter set for the selected function.

## Required Questions By Function

### A. Reference Import

Ask for missing items:

- source type: webpage links, DOI, PDF, BibTeX, RIS, EndNote XML, list file;
- source path or pasted source content;
- target EndNote library;
- target EndNote group: existing group, new group, or library only;
- version preference: journal, conference, Early Access, arXiv, keep all;
- duplicate handling: report only, skip, keep formal version, ask per item;
- whether to download and attach PDFs;
- whether to complete Abstract, Keywords, DOI, URL, and publication fields;
- output files: RIS, BibTeX, EndNote XML, import report, all.

Do not import before confirmation.

### B. EndNote Group Checking

Ask for missing items:

- EndNote library;
- EndNote group;
- checks required: metadata, duplicates, missing PDF, missing abstract, publication status, group membership;
- whether to modify records or only report;
- desired output report format and path.

### C. Related Work Writing

Ask for missing items:

- EndNote library;
- EndNote group;
- whether only group references may be cited;
- whether external references may be added;
- year range and excluded source types;
- topic, paper/chapter title, user's work or innovation points, target method/chapter served by this related work;
- desired organization: time, technical route, sensor, algorithm framework, optimization, centralized/distributed, single/multi-robot, localization/mapping/reconstruction/understanding, traditional/learning, discrete/continuous time, custom outline, or skill recommends outline after reading and user confirms;
- language;
- document type: doctoral thesis, journal paper, conference paper, report;
- continuous prose or subsections;
- section title and numbering;
- whether to keep existing title structure;
- background opening, subsection summaries, final link to user's work;
- whether lists are allowed;
- style sample file and specific section/page range;
- style dimensions to learn: language, sentence length, paragraph structure, citation density, comparison method, critical analysis, transitions, Word formatting;
- target words or pages;
- page estimation basis if pages are requested;
- subsection allocation;
- maximum length and paragraph length control;
- citation insertion requirement;
- citation mode: real EndNote CWYW, temporary placeholders, no citations;
- EndNote output style;
- numeric or author-year format;
- grouped citations allowed;
- citation density;
- whether all group references must be cited;
- bibliography generation;
- citation-to-bibliography navigation;
- DOI and URL hyperlinks;
- Word write requirement;
- target Word file;
- exact insertion location;
- replace, append, or optimize existing text;
- preserve existing EndNote fields, comments, and tracked changes;
- tracked changes on/off;
- reuse target Word styles;
- update table of contents and bibliography;
- output path;
- backup requirement.

Do not read the EndNote group, draft final text, insert citations, or write Word until confirmation.

### D. Insert Citations Into Existing Text

Ask for missing items:

- target Word file;
- chapter, section, paragraph, or range;
- EndNote library;
- EndNote group;
- output style;
- real CWYW or placeholders;
- whether source text may be modified;
- existing citation handling: restore original sources or treat static numbers as placeholders and choose suitable group references;
- sentence-level or paragraph-level matching;
- permitted optimization scope: citation-only, wording/grammar, claim narrowing, sentence reordering, transition improvement, or paragraph restructuring;
- unsupported-claim policy: leave and report, revise within permission, or ask before revision;
- multiple citations per sentence allowed;
- bibliography update;
- citation-to-bibliography navigation;
- DOI/URL hyperlink requirements;
- whether to report uncited records and unsupported claims.

If the user says existing content may be modified according to available references, treat that as permission for limited evidence-based and logic-improving edits, not permission for a full rewrite. Preserve the paragraph's research intent and report material meaning changes.

Do not match references only by keywords. Read enough evidence to support each citation.

### E. Style Learning

Ask for missing items:

- style source file;
- section, chapter, or page range;
- Chinese or English style;
- dimensions to learn: language, paragraph, logic, citation, terminology, headings, formatting, all;
- whether to generate `style_profile.md`;
- whether to immediately apply the style;
- target writing task;
- target length.

### F. Word Formatting Learning

Ask for missing items:

- template Word file;
- section or sample paragraphs to learn;
- styles to learn;
- target Word file;
- application range;
- whether to preserve existing target styles;
- whether new styles may be created;
- whether to process headings, body, captions, equations, bibliography;
- whether to update table of contents;
- whether to preserve headers, footers, pagination, and sections;
- backup requirement.

### G. Existing Related Work Revision

Ask for missing items:

- source file and section;
- task type: polish, restructure logic, add references, full rewrite;
- preserve original paragraph structure;
- preserve original citations;
- allow deleting irrelevant citations;
- EndNote group for supplemental references;
- allow external references;
- target words or pages;
- writing style;
- write back to Word;
- preserve EndNote CWYW fields;
- output revision notes or comparison table.

### H. Citation, Bibliography, And Link Checking

Ask for missing items:

- target Word file;
- check scope;
- expected EndNote library/group if relevant;
- whether to check CWYW fields, bibliography synchronization, numbering continuity, citation jumps, DOI links, URL links, duplicate bibliography entries;
- whether to modify or report only;
- report output path.

### I. Combined Workflow

Ask once for all relevant staged parameters:

- import parameters: sources, library, group, version choice, duplicate handling, PDF attachment;
- related-work parameters: topic, scope, outline, language, style, words/pages, citation density, use all references;
- Word and citation parameters: target file, insertion location, replace/append/optimize, CWYW, output style, hyperlinks, bibliography update, output path.

Execute stages only after the combined configuration is confirmed.

## Related Work Question Template

Use this template when many parameters are missing:

```text
开始前需要确认本次写作配置。请填写以下信息；已明确的项目可以保留当前设置。

1. EndNote Library：
2. EndNote Group：
3. 是否仅使用该 Group 内文献：
4. Related Work 主题：
5. 论文或章节题目：
6. 本文研究内容或创新点：
7. 写作语言：
8. 文档类型：博士论文 / 期刊论文 / 会议论文
9. 写作大纲：
10. 组织逻辑：
11. 写作风格参考文件：
12. 需要学习的章节或页码：
13. 目标字数：
14. 目标页数：
15. 每小节篇幅：
16. 是否插入文献引用：
17. 引用方式：EndNote CWYW / 临时引用 / 不插入引用
18. EndNote Output Style：
19. 是否生成文后参考文献：
20. 是否生成正文到参考文献的跳转：
21. 是否添加 DOI 和 URL 超链接：
22. 目标 Word 文件：
23. 写入位置：
24. 原内容处理方式：保留优化 / 替换 / 追加
25. 是否备份原文件：
26. 输出文件路径：
27. 其他特殊要求：
```

## Simplified Question Mode

When the user already supplied most parameters, ask only missing items. Example:

```text
当前还需要确认以下 5 项：

1. 使用哪个 EndNote Library？
2. 是否仅引用该 Group 内文献？
3. EndNote Output Style 沿用目标 Word 当前样式，还是指定其他样式？
4. 原内容处理方式：替换 / 保留优化 / 追加？
5. 输出文件路径是否覆盖原文件？
```

## Task Configuration Summary

After the user answers, output:

```text
任务配置摘要：

- 执行功能：
- EndNote Library：
- EndNote Group：
- 文献范围：
- 写作主题：
- 写作语言：
- 文档类型：
- 大纲结构：
- 组织逻辑：
- 写作风格：
- 目标字数：
- 目标页数：
- 引用方式：
- Output Style：
- 是否插入参考文献：
- 超链接要求：
- 目标 Word：
- 写入位置：
- 原文处理方式：
- 现有引文处理方式：恢复原来源 / 作为占位符重新匹配：
- 语义匹配粒度：
- 允许的正文优化范围：
- 无法支撑的句子处理方式：
- 输出路径：

请确认以上配置。回复“确认执行”后开始；需要调整时请直接指出修改项。
```

Remain in `WAITING_FOR_CONFIRMATION` until the user confirms.

## Default Values

Only use defaults when the user explicitly allows defaults. Suggested defaults:

- language: infer from user input;
- document type: doctoral thesis;
- reference scope: specified EndNote group only;
- organization: technical route;
- citation mode: real EndNote CWYW;
- output style: current Word EndNote output style;
- writing style: current target Word section;
- original text handling: preserve and optimize;
- Word styles: reuse target document styles;
- hyperlinks: preserve EndNote-supported links;
- backup: create before modification.

Even with defaults, output and confirm the task configuration summary.

## Forbidden Behavior

- Do not read the whole library when no group is specified.
- Do not choose an EndNote group automatically.
- Do not choose outline, length, citation mode, or Word insertion point automatically.
- Do not insert citations if the user has not requested or confirmed citation insertion.
- Do not write Word when target location is unclear.
- Do not continue after asking questions before the user answers.
- Do not treat recommended settings as confirmed settings.
