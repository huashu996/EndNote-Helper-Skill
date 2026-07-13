# Examples

All examples must follow the interactive gate:

1. detect task type;
2. ask missing required parameters;
3. output task configuration summary;
4. wait for `确认执行`;
5. execute only after confirmation.

## Example 1: Import Paper Links Into EndNote

```text
使用 EndNote-Helper-Skill，读取 links.txt 中的论文链接，检查正式发表信息，生成 RIS 文件，导入 My EndNote Library，并加入 Collaborative SLAM 组集。
```

Expected first response if some parameters are missing:

```text
当前还需要确认以下项目：

1. EndNote Library 的完整路径是什么？
2. `Collaborative SLAM` 是已有 Group 还是需要新建？
3. 版本优先规则：正式期刊 / 正式会议 / Early Access / arXiv / 保留全部？
4. 疑似重复文献如何处理：只报告 / 跳过 / 保留正式版本 / 逐条确认？
5. 是否下载并关联 PDF？
6. 输出报告保存到哪里？
```

After user answers, output task summary and wait for `确认执行`.

Execution after confirmation:

1. Parse links.
2. Extract and validate metadata.
3. Generate RIS.
4. Import to EndNote.
5. Add to group.
6. Report duplicates and incomplete records.

## Example 2: Generate Related Work

```text
使用 EndNote-Helper-Skill，读取 EndNote Library 中的 Collaborative SLAM 组集，按照“集中式协同 SLAM、分布式协同 SLAM、通信受限协同 SLAM、协同地图融合”的逻辑生成高质量 Related Work，并将 EndNote CWYW 引用插入 thesis.docx 的 1.2.3 节。
```

Expected first response if some parameters are missing:

```text
当前还需要确认以下项目：

1. EndNote Library 的完整路径是什么？
2. 是否仅允许引用 `Collaborative SLAM` 组集内文献？
3. 写作语言和文档类型是什么？
4. 目标字数或页数是多少？
5. 是否有写作风格参考文件或沿用 thesis.docx 现有风格？
6. EndNote Output Style 沿用 Word 当前样式还是指定其他样式？
7. 写入方式：替换 1.2.3 原文 / 追加 / 保留并优化？
8. 输出文件覆盖 thesis.docx 还是另存？
```

After user answers, output task summary and wait for `确认执行`.

Execution after confirmation:

1. Read group only.
2. Build literature analysis table.
3. Plan sections by technical route.
4. Draft synthesis.
5. Insert temporary EndNote citations.
6. Run CWYW formatting.
7. Verify EndNote fields and bibliography.

## Example 3: Learn Writing Style

```text
使用 EndNote-Helper-Skill，学习 example_thesis.docx 中第 1.2 节的写作风格、引用密度和排版格式，然后基于 EndNote 的 LiDAR SLAM 组集撰写新的研究现状。
```

Expected first response if some parameters are missing:

```text
当前还需要确认以下项目：

1. EndNote Library 的完整路径是什么？
2. `LiDAR SLAM` 组集是否为唯一文献范围？
3. 新研究现状的章节标题和写入位置是什么？
4. 目标字数或页数是多少？
5. 是否插入真实 EndNote CWYW 引用？
6. 输出 Word 文件是什么？
```

After user answers, output task summary and wait for `确认执行`.

Execution after confirmation:

1. Extract style profile.
2. Inspect Word styles.
3. Read EndNote group.
4. Draft using style profile.
5. Write into Word using existing styles.

## Example 4: Update Existing Related Work

```text
使用 EndNote-Helper-Skill，检查 thesis.docx 中已有 Related Work，结合 EndNote 指定组集补充缺失文献，修正错误引用，保留现有 CWYW 字段，并更新文后参考文献。
```

Expected first response if some parameters are missing:

```text
当前还需要确认以下项目：

1. 需要检查 thesis.docx 的哪个章节或范围？
2. 使用哪个 EndNote Library 和 Group？
3. 是否允许删除不相关引用？
4. 是润色、补充、重构，还是三者都做？
5. 目标字数或页数是否需要变化？
6. 是否直接写回 thesis.docx？
7. 是否输出修改说明或对照表？
```

After user answers, output task summary and wait for `确认执行`.

Execution after confirmation:

1. Backup Word document.
2. Inspect existing citation fields.
3. Compare cited references with target group.
4. Draft targeted additions.
5. Preserve existing CWYW fields.
6. Rerun EndNote formatting and verify.

## Required Behavior Tests

### Test 1

User:

```text
使用 Collaborative SLAM 组集写研究现状。
```

Skill must ask at least:

- EndNote Library;
- writing topic;
- outline or organization logic;
- language;
- target pages/words;
- citation insertion requirement;
- citation format;
- Word file;
- insertion location;
- writing style.

It must not read the whole library or start writing.

### Test 2

User:

```text
基于 EndNote 中的 LiDAR SLAM 组写博士论文 1.2.1 节，约 10 页，按点到点、点到面、固态雷达、连续时间和通用场景组织，写入毕业论文.docx，并插入 CWYW 引用。
```

Skill must not repeat already clear parameters:

- group;
- page count;
- document type;
- outline;
- write location;
- citation mode.

It should ask only missing items:

- EndNote Library;
- style reference;
- EndNote Output Style;
- whether only the group may be cited;
- replace or optimize existing content;
- output path.

### Test 3

User:

```text
把 links.txt 中的论文导入 EndNote。
```

Skill must ask:

- EndNote Library;
- EndNote Group;
- formal-version preference;
- duplicate handling;
- whether to download PDFs;
- output report format.

It must not import before confirmation.

### Test 4

User:

```text
学习 example.docx 的写作风格。
```

Skill must ask:

- section or page range;
- style dimensions;
- whether to generate `style_profile.md`;
- whether to apply immediately;
- target task if applying.

It must not generate or apply a style profile before confirmation.
