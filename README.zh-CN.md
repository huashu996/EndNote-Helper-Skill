# EndNote Helper Skill

[English](README.md) | [简体中文](README.zh-CN.md)

一个面向 EndNote 与 Microsoft Word 的端到端科研写作 Agent Skill。它可以校验和整理文献，只分析用户确认的 EndNote 组，根据句子含义优化已有正文，插入真实 Cite While You Write（CWYW）字段，从范文学习语言与版式，并验证最终 Word 文档。

本技能适用于同时重视引用正确性、EndNote 组范围、元数据质量、Word 排版以及 EndNote 引用可更新性的工作。

## 核心能力

- 从 DOI、网页、PDF、BibTeX、RIS、EndNote XML 等来源导入文献。
- 校验文献元数据，对缺失或可疑字段进行报告，不臆造内容。
- 将证据检索严格限制在用户确认的 EndNote 组内。
- 按句子或命题建立语义引用映射，并选择最小充分引用集合。
- 在保留研究意图的前提下优化语法、论断范围、过渡和段落逻辑。
- 在 Word 中插入真实 `ADDIN EN.CITE` 引用字段和可更新的 `ADDIN EN.REFLIST` 参考文献表。
- 从学位论文模板、论文和期刊 PDF 学习语言与版式，但不复制原句。
- 将英文正文重新组织为 IEEE/RA-L 风格双栏版式。
- 保留无关 Word 内容；除非用户明确要求，否则绝不移除 CWYW 字段代码。
- 自动保留备份、语义映射表、修改记录、预览文件和验证报告。

## 确认后执行

本技能强制使用确认门控：

1. `TASK_DETECTED`：识别任务类型。
2. `PARAMETERS_INCOMPLETE`：一次性询问缺失的必要信息。
3. `WAITING_FOR_CONFIRMATION`：汇总文件、EndNote 库、组、引用策略、正文修改范围、排版目标和覆盖策略。
4. `READY_TO_EXECUTE`：只有用户明确确认后，才修改文件或操作 EndNote/Word。

除非用户明确要求恢复原始出处，否则 `[1]` 等静态编号仅被视为占位符，不会被错误地映射到 EndNote 组中第一个记录。

## 支持的工作流

| 工作流 | 功能 |
|---|---|
| 文献导入 | 生成导入文件、导入记录、加入指定组，并报告跳过项和元数据缺失项。 |
| 元数据校验 | 检查作者、年份、标题、来源、卷期页、DOI、URL、类型、摘要、关键词和附件。 |
| 组内文献分析 | 只读取确认的组，写作前先建立文献分析表或论点证据表。 |
| 相关工作写作 | 综合方法类别、技术演进、优势、局限和研究空白。 |
| 已有正文优化 | 将正文拆成论点，重新匹配占位引用，并在允许范围内优化逻辑。 |
| 真实 CWYW 插入 | 将真实临时 EndNote 引文转换为可更新的 Word 引用和参考文献字段。 |
| 风格学习 | 从 Word、学位论文模板或期刊 PDF 提取论述方式和视觉版式。 |
| RA-L/IEEE 排版 | CWYW 转换成功后应用双栏版式，并检查渲染结果。 |
| 质量检查 | 检查字段、OOXML、文本、PDF 预览、超链接以及 Word 原生可打开性。 |

## 使用条件

### 基础功能

- 支持 Agent Skills 格式的智能体，例如 Codex。
- Python 3，用于运行元数据和引用检查脚本。

### 真实 Word/EndNote CWYW

- Windows。
- Microsoft Word 桌面版。
- EndNote 桌面版，并启用 Cite While You Write 加载项。
- 本地可访问的 EndNote 库、指定组及所需 PDF 附件。

### 可选 PDF 风格提取

建议安装 Poppler 工具，包括 `pdfinfo`、`pdftotext` 和 `pdftoppm`。

没有 Word 或 EndNote 时，仍可使用元数据分析和写作指导功能；但创建真实 CWYW 字段必须依赖上述桌面应用。

## 安装

### Windows Codex

直接克隆到个人技能目录：

```powershell
git clone https://github.com/huashu996/EndNote-Helper-Skill.git `
  "$env:USERPROFILE\.codex\skills\EndNote-Helper-Skill"
```

更新已安装版本：

```powershell
git -C "$env:USERPROFILE\.codex\skills\EndNote-Helper-Skill" pull
```

如果没有立即发现技能，请重新启动或重新加载智能体会话。

### 其他兼容 Agent Skills 的客户端

将仓库克隆到客户端配置的技能目录。仓库根目录就是技能根目录，其中包含必须的 `SKILL.md`。

## 快速开始

显式调用技能，并提供已经确定的文件和范围：

```text
使用 $endnote-helper-skill 检查当前 EndNote 库中“毕业论文 / 02 LO”组的全部文献，
根据 test.docx 每句话的含义重新匹配组内文献，适当优化英文逻辑，并插入真实 CWYW
字段。语言和双栏版式参考提供的 RA-L PDF，覆盖前先建立备份。
```

其他示例：

```text
使用 $endnote-helper-skill 将这份 DOI 列表导入当前库，加入“第三章”组，
并报告元数据缺失项和重复候选项。
```

```text
使用 $endnote-helper-skill 根据“协同 SLAM”组写一篇 2000 字相关工作，
并在 Word 中插入可更新的 EndNote 引用。
```

技能会先输出执行配置摘要，等待用户确认后再修改 EndNote 或 Word。

## 语义引文流程

处理已有正文时，技能按以下顺序执行：

1. 重新审计当前 Word 文件，不依赖过期的文件状态。
2. 验证指定组的成员和候选记录号。
3. 将正文拆分为句子级或命题级论点。
4. 按论点需要读取相应深度的文献证据。
5. 建立包含置信度与证据边界的“论点—文献”矩阵。
6. 为每个论点选择最小充分引用集合。
7. 只在确认范围内缩小论断、调整顺序或补充过渡。
8. 记录实质性修改和仍无证据支持的论断。
9. 插入 `{Zhang, 2016 #69}` 形式的真实临时 EndNote 引文。
10. 运行 CWYW；只要仍存在临时引文，就判定转换失败。

该流程不会按关键词、文件顺序或 EndNote 组顺序机械匹配，也不会给无证据论断强塞引用。

## 真实 CWYW，而非静态编号

只有 Word 文档包含以下 EndNote 字段时，才算插入成功：

- `ADDIN EN.CITE`：正文显示引用；
- `ADDIN EN.REFLIST`：EndNote 生成的参考文献表；
- 不再残留 `{Author, Year #123}` 形式的临时引文。

技能不会手工输入参考文献，也不会通过修改字段显示结果来掩盖错误元数据。存在异常的记录必须在 EndNote 中修正，或排除后明确报告。

## IEEE RA-L 双栏工作流

如果用户提供期刊 PDF 作为风格来源，技能会检查：

- PDF 元数据和纸张尺寸；
- 保留版面的文本及分栏顺序；
- 渲染后的首页；
- 代表性正文页；
- 参考文献页。

技能会将可迁移的论述方式和版式，与论文专属的标题块、收稿日期、版权信息和插图分开处理。

可复用的 RA-L 起始配置包括 US Letter、等宽双栏、Times 系正文字体、行内数字引用、紧凑居中标题和小字号悬挂缩进参考文献。实际执行时会以用户提供的样本为准，而不是机械套用固定数值。

详见 [RA-L 双栏语义 CWYW 流程](references/12_ral_two_column_semantic_cwyw.md)。

## 后台 Word 自动化

正常情况下，技能在后台运行 Word 的 EndNote 格式化宏。如果 PowerShell Word 自动化出现 `TYPE_E_CANTLOADLIBRARY` 或 `0x80029C4A`，可以使用仓库中的后期绑定 VBScript：

```powershell
cscript.exe //nologo scripts\format_ral_cwyw.vbs `
  working.docx preview.pdf 12
```

脚本必须作用于可丢弃的工作副本，并且需要预先选择正确的 EndNote 输出样式。它不能替代语义匹配和元数据检查。

## 分层验收

覆盖目标文档之前，相关工作流会依次检查：

1. **证据与元数据**：论点支撑、组范围、作者与来源显示质量、意外自引。
2. **EndNote 字段**：引用字段数、参考文献字段及临时引文残留。
3. **OOXML 版式**：纸张、页边距、分栏和引用格式。
4. **渲染文本**：段落顺序、引用编号、文献数量和元数据。
5. **视觉预览**：分栏流动、标题、间距、溢出和参考文献缩进。
6. **Word 原生打开**：以只读方式检查页数、段落数和字段数。
7. **最终复制**：验证后的工作副本与交付文件哈希一致。

## 仓库结构

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

## 构建目录规范

每个任务应建立本地构建目录，例如：

```text
build/endnote-helper/<timestamp>/
```

其中可保存备份、工作副本、语义映射表、修改记录、风格画像、预览、导入文件和验证报告。不要将这些任务产物提交到本仓库。

## 隐私与安全

- 不要把 EndNote 库、`.Data` 文件夹、PDF、Word 稿件或文献提取笔记放入技能仓库。
- 除非用户明确要求并理解公开范围，否则绝不上传用户文档或参考文献库。
- 不臆造元数据、记录号、论文结论、DOI 或发表状态。
- 不静默删除或合并重复文献。
- 除非用户明确要求断开 EndNote 链接，否则不运行 `ENRemoveFieldCodes`。
- 恢复后台自动化时，不结束用户可见的 Word 进程，也不丢弃未保存内容。

## 局限性

- 不同 EndNote 版本的库内部结构和组信息可能不同，部分步骤仍需在原生应用中验证。
- 引用标点形式取决于当前选择的 EndNote 输出样式。
- DOI 可能存在于 EndNote 字段数据中，但不会被当前参考文献样式显示。
- 引用跳转和 DOI/URL 可点击性取决于 Word、EndNote 以及输出样式设置。
- 后期绑定格式化脚本只是故障恢复方案，不能替代证据分析或元数据校验。

## 参与改进

修改技能时应保留确认门控、组内证据约束、真实 CWYW 要求和分层验收模型。请在可丢弃的测试文件上验证改动，不要直接使用正式论文进行首次测试。
