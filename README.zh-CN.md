# EndNote Helper Skill

**面向学术写作的 EndNote 工作流：文献整理、证据驱动的综述写作，以及可更新的 Word 引用。**

[English](README.md) · [Skill 主文件](SKILL.md) · [使用示例](references/10_examples.md)

本 skill 指导 AI 助手完成文献导入、元数据核验、指定文献组阅读、相关工作撰写，以及真实 EndNote **Cite While You Write（CWYW，即边写边引）** 字段插入。也支持根据已有正文的语义匹配参考文献、有限度优化原文，以及参考样稿学习语言和 Word 排版。

仓库包含 **Agent 指令、可复用模板、4 个离线 Python 辅助脚本和 1 个可选的 Windows 自动化脚本**。安装 skill 不会安装 EndNote，也不会自动连接文献库、提供通用 EndNote API 或取得 Word 控制权限。

## 目录

- [功能范围](#功能范围)
- [环境要求](#环境要求)
- [安装与更新](#安装与更新)
- [快速使用](#快速使用)
- [工作流程与交付物](#工作流程与交付物)
- [命令行辅助脚本](#命令行辅助脚本)
- [进阶：Word 与 CWYW 双栏排版](#进阶word-与-cwyw-双栏排版)
- [结果验收](#结果验收)
- [常见问题](#常见问题)
- [隐私与本地数据](#隐私与本地数据)
- [仓库文件说明](#仓库文件说明)
- [参与维护](#参与维护)

## 功能范围

| 功能 | 工作流负责什么 | 需要什么条件 |
|---|---|---|
| 文献导入 | 核验来源、整理导入文件，导入指定库和组并核对记录编号 | 可访问的来源和可用的 EndNote 操作方式 |
| 元数据检查 | 标记缺失字段、待核实的发表状态和疑似重复记录 | 导出的记录；权威核验还需要 DOI 或出版方来源 |
| 指定组阅读 | 限定文献范围，建立证据与方法分析表 | 可核实的组成员关系、摘要或全文 |
| 相关工作写作 | 按问题、方法关系、证据与局限组织综述 | 主题、大纲、长度、语言、文献证据和目标文档 |
| 已有正文配引文 | 逐句匹配证据，只在允许范围内改写 | 明确的引用模式、文献组和修改权限 |
| Word/CWYW 引用 | 插入真实 EndNote 字段，生成可更新的参考文献表 | 桌面版 Word、EndNote、有效的 CWYW 插件和应用操作权限 |
| 风格学习 | 分析样稿的语言特点并检查版式，应用适合当前任务的规则 | 用户样稿及相应的文档/PDF 检查工具 |
| 链接检查 | 分别检查引用字段、正文到文后跳转、DOI/URL 链接 | DOCX 结构检查，以及 Word/渲染结果验证 |

辅助脚本只覆盖其中一部分。它们本身不会完成语义选文献、导入文献库、阅读 PDF 或自动写完整综述。

## 环境要求

### 使用 Agent 工作流

- 支持加载 `SKILL.md`、读取用户指定文件的 AI 助手环境。
- 核验 DOI 或出版方信息时，需要网络访问能力；无法访问的来源应明确标记为未核实。
- 插入真实引用时，需要 EndNote、桌面版 Microsoft Word 和有效的 CWYW 插件。助手还需要可用的应用交互方式；仓库不附带通用 EndNote API 桥接器。
- 学习期刊 PDF 或检查版面时，需要额外的 PDF 提取和渲染工具；本仓库未捆绑 PDF 渲染器。

可选 `.vbs` 脚本面向 **Windows 桌面版 Word 与 EndNote**。仓库未提供 macOS/Linux 的完整应用自动化方案。Word 网页版不能替代脚本使用的桌面 COM 自动化。

### 仅运行离线辅助脚本

- 建议使用 **Python 3.10 或更高版本**。
- 4 个 Python 脚本均只使用标准库，不需要额外执行 `pip install`。
- `detect_duplicate_references.py` 会导入同目录的 `validate_metadata.py`，请保留目录结构。
- Git 仅用于克隆和更新；也可以下载仓库 ZIP 后解压安装。

## 安装与更新

### 在 Windows 的 Codex 中安装

在 PowerShell 中执行。下列命令会使用现有 `CODEX_HOME` 设置，不会修改该变量：

```powershell
$codexRoot = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE '.codex' }
$skillRoot = Join-Path $codexRoot 'skills'
New-Item -ItemType Directory -Path $skillRoot -Force | Out-Null
git clone https://github.com/huashu996/EndNote-Helper-Skill.git (Join-Path $skillRoot 'endnote-helper-skill')
```

安装后的目录应直接包含 `SKILL.md`、`references/`、`scripts/` 和 `templates/`。同一 skill 建议只保留一个安装副本，避免重复发现。如果当前会话没有发现它，重新加载技能列表或开启新会话。

其他支持 skill 的助手，应使用其文档规定的安装位置。仓库内容自包含；`SKILL.md` 中提及的历史名称 `related-work` 和 `endnote-cwyw-citations` 不是需要另行安装的依赖。

### 更新已有安装

先按上文设置 `$skillRoot`，再执行：

```powershell
$installedSkill = Join-Path $skillRoot 'endnote-helper-skill'
git -C $installedSkill status --short
git -C $installedSkill pull --ff-only
```

如果安装时使用了其他文件夹名，请填写实际路径。有本地改动时，先检查并保存改动，不要为完成更新而直接重置自定义内容。通过 ZIP 安装的副本，需要备份本地修改后手动更新。

## 快速使用

使用 `$endnote-helper-skill` 调用。下列文件名、组名和作者标识均为通用示例，不对应真实个人文献库。

### 1. 导入和整理文献

```text
使用 $endnote-helper-skill 检查 links.txt 中的文献来源，为我指定的 EndNote
Library 生成 RIS 文件，导入已有的“Example Group”组。优先采用已核实的
正式发表版本。疑似重复项只报告，不删除、不合并。不下载 PDF。
将导入文件和检查报告放到 build/endnote-helper/。
执行前补齐 Library 路径等缺失信息并确认任务配置。
```

### 2. 撰写相关工作

```text
使用 $endnote-helper-skill，仅依据指定 Library 的“Example Group”组，
为 manuscript.docx 撰写约 1200 英文词的 Related Work。
按研究问题、方法类别、证据和局限组织，沿用现有 Word 样式与选定的数字式
EndNote Output Style，插入真实 CWYW 引用。
另存为 manuscript_reviewed.docx，保留原文件。
执行前确认具体主题和写入范围。
```

### 3. 给已有正文匹配引用

```text
使用 $endnote-helper-skill 检查 manuscript.docx 的 Introduction，文献范围
仅限指定 Library 中的“Example Group”。使用 GROUP_SEMANTIC_REPLACE 模式，
将已有静态编号作为待替换占位符。允许纠正语法和适度收窄缺乏证据的表述，
保留原研究论点。无法由组内文献支持的句子单独报告。
生成逐句证据映射表，插入真实 CWYW 字段，并另存审阅版。
```

### 4. 学习语言与版式

```text
使用 $endnote-helper-skill 分析 sample.pdf 的正文页和参考文献页，记录语言
结构、引文习惯、纸张尺寸、页边距、栏数与字体。确认风格方案后，将其应用到
manuscript.docx 的工作副本，保留已验证的 CWYW 字段。
对无法可靠测量或验证的版式项目明确说明。
```

实际 EndNote/Word 任务会先补齐必要参数，再展示任务配置摘要。已提供的信息不应重复询问。确认时重点核对 Library、Group、修改范围、Output Style、目标段落与保存方式。具体规则见[交互式任务配置](references/00_interactive_task_configuration.md)。

## 工作流程与交付物

1. **确定范围：**明确输入、允许引用的来源、输出位置和可修改内容。
2. **检查文献：**核对组成员、元数据、发表状态和重复候选项。
3. **阅读证据：**建立文献分析表和“正文主张—记录编号”映射。标题相似不能支撑具体实验结论。
4. **撰写或修改：**梳理论证关系，将引文放在被支持的句子旁，并记录实质语义变化。
5. **生成引用：**使用指定库中的真实记录编号，在已备份的工作副本上运行 EndNote 格式化。
6. **检查交付：**验证字段、文后参考文献、来源范围、按要求生成的链接与实际版面；检查通过后再替换已获授权的目标文件。

已有正文有三种引用模式：

| 模式 | 含义 |
|---|---|
| `RESTORE_ORIGINAL` | 找回并恢复已有标记原本指向的来源 |
| `GROUP_SEMANTIC_REPLACE` | 将静态编号作为占位符，按语义从确认的组内重新选文献 |
| `ADD_TO_UNCITED_TEXT` | 为尚未引用的正文补充有证据支持的引文 |

`[1]` 不等于 EndNote 中的第 1 条记录。记录编号属于特定文献库；一处组合引用可以包含多篇文献，但在 Word 中仍可能只有一个引用字段。

任务文件一般保存在用户项目附近的 `build/endnote-helper/<timestamp>/`：

| 交付物 | 用途 |
|---|---|
| 导入文件与导入报告 | 记录导入成功、跳过、待核实项和分配后的记录编号 |
| 文献分析表 | 梳理方法、证据、局限与来源范围 |
| 正文—文献映射表 | 说明每个主张由什么记录支持、实际读了哪些证据 |
| 修改记录与风格档案 | 记录实质变化和测量/学习得到的格式特征 |
| 工作版/审阅版 DOCX 与备份 | 保留原文件，验证可编辑引用 |
| 验证报告与 PDF 预览 | 记录检查结果、未解决事项和实际排版效果 |

这些是工作流按需产出的材料，并不是运行任意一个脚本就会全部自动生成。

## 命令行辅助脚本

以下示例均在**仓库根目录**执行，输入文件和输出路径请按本地任务替换。Python 脚本读取输入并输出报告，不修改 EndNote 库或源 DOCX。每个脚本均支持 `--help` 和 `-o`/`--output`；省略 `-o` 时打印到终端。

4 个脚本均在**报告生成成功时返回 0，捕获到错误时返回 2**。返回 0 不代表元数据完整、引用正确或文档已通过验收，必须继续检查报告内容。

### 检查元数据

```powershell
python scripts/validate_metadata.py records.json -o build/endnote-helper/metadata.json
```

输入范围和限制：

| 格式 | 说明 |
|---|---|
| JSON | 单条对象、对象数组，或包含 `records`、`items`、`references` 数组的对象 |
| CSV | UTF-8/UTF-8-BOM 编码，逗号分隔，首行为字段名 |
| RIS | 基础标签记录；简易解析器要求结束行为 `ER  - `，包括连字符后的空格；不完整支持多行续写 |
| BibTeX | 基础花括号或引号字段；不完整支持嵌套花括号、宏和复杂条目 |

代码虽然接受 `.tsv` 后缀，但仍按逗号分隔解析。真正的制表符文件应先转为 CSV。这两个元数据相关脚本**不直接解析 EndNote XML、`.enl`、PDF 或 Excel 文件**，需要先通过其他工作流导出/转换。

下例仅用于演示报告格式，属于合成数据，不能作为学术文献引用：

```json
[
  {
    "record": "demo-web-1",
    "type": "webpage",
    "author": "Example Organization",
    "year": "2024",
    "title": "Demonstration web record — not a research source",
    "url": "https://example.org/reference-example",
    "access_date": "2024-02-01"
  }
]
```

建议使用扁平字符串字段。常见别名包括 `authors → author`、`journal/conference/booktitle → venue`、`record_number → record`；作者数组不会被深度解析。

离线检查要求 title/year/author，并对学术记录检查 doi/venue，对网页记录检查 url/access_date。“有 URL、无 DOI”的记录也会按网页处理。这只是完整性启发规则，不是通用书目规范。报告包含 `input`、`count`，以及每条记录的 `missing_fields` 和 `status`。状态为空不代表文献真实存在或元数据正确；出版方核验、PDF 附件和更完整的字段审查仍需另外执行。

### 检测疑似重复记录

```powershell
python scripts/detect_duplicate_references.py records.json --title-threshold 0.92 -o build/endnote-helper/duplicates.json
```

使用与元数据脚本相同的输入解析器。DOI 相同或规范化标题相同会列为高置信度候选；标题相似且年份或第一作者一致会列为中等置信度候选。输出包含 `duplicate_candidates`、记录 ID、标题、置信度和判定原因。

算法逐对比较，大规模文献库可能耗时较长。脚本只报告，不删除、不合并；会议版与期刊扩展版等情况仍需人工判断。

### 检查 DOCX 中的引文字段标记

```powershell
python scripts/check_citations.py manuscript_reviewed.docx -o build/endnote-helper/citations.json
```

仅扫描 **`word/document.xml`**，报告 EndNote 标记出现次数、临时引文、超链接元素、书签、内部锚点及 DOI/URL 文本样例。

这属于 XML 文本启发式检查，不能当作 Word 的精确字段计数：`ADDIN EN.CITE` 也可能匹配 `EN.CITEDATA`，被 XML 分段拆开的文字可能漏检。脚本不检查页眉、页脚、脚注、尾注或超链接关系文件。存在 DOI 字符串或超链接元素，不代表链接能点击、目标正确或能访问。真实字段和所需跳转仍要在 Word 中验证。

### 提取基础语言风格统计

```powershell
python scripts/extract_style_profile.py sample.docx -o build/endnote-helper/style.md
python scripts/extract_style_profile.py sample.txt --json -o build/endnote-helper/style.json
```

接受 TXT、Markdown 和 DOCX，输出字符数、近似句长/段长、引文密度、连接词和高频词。DOCX 只做粗略正文提取，不测量字体、页边距、栏数、标题样式或视觉布局。PDF 样稿需要单独提取并渲染检查；字符统计不等同于语言学意义上的词数统计。

## 进阶：Word 与 CWYW 双栏排版

[`scripts/format_ral_cwyw.vbs`](scripts/format_ral_cwyw.vbs) 是针对**可替换工作副本**的专用排版脚本，也提供 `TYPE_E_CANTLOADLIBRARY` / `0x80029C4A` 场景下的 late-bound COM 路径。它不是通用的 Word 格式保真工具，也不是期刊官方模板认证工具。

运行前需要：

1. 备份原文并创建独立的工作 DOCX。脚本会保存和修改传入文件，不会自行创建备份。
2. 打开正确的 EndNote 库，核实真实记录编号，并预先选择需要的 Output Style。
3. 确认桌面 Word、CWYW 插件以及 Windows Script Host/VBScript 可用，并符合当前环境的权限要求。
4. 使用绝对输入/输出路径，保证输出目录存在，并确认下述固定版式适合当前任务。

```powershell
$workingDoc = (Resolve-Path 'build/working.docx').Path
$previewPdf = Join-Path (Split-Path $workingDoc) 'preview.pdf'
cscript.exe //nologo scripts/format_ral_cwyw.vbs $workingDoc $previewPdf 8
```

参数说明：

| 位置 | 参数 | 含义 |
|---|---|---|
| 1 | 工作 DOCX | 必填；会被原地修改 |
| 2 | 预览 PDF | 必填；完成排版后导出 |
| 3 | 预期引用字段数 | 可选，默认 0；正数代表**最低字段数**，不是精确数量，也不是去重后的文献篇数 |
| 4 | `clear-footers` | 可选；清空页脚内容。省略时保留页脚。使用该项时必须给出第 3 项，可填 `0` |

脚本依次尝试 `Project.EndNoteMacros.ENFormatBibliography`、`EndNoteMacros.ENFormatBibliography` 和 `ENFormatBibliography`。若仍有临时引文、没有引用字段、字段数低于设定下限或没有参考文献字段，则判为失败。脚本不会选择文献库/输出样式、判断证据、生成语义映射或修复元数据。

CWYW 格式化成功后应用的固定设置：

| 设置 | 值 |
|---|---|
| 纸张 | US Letter，612 × 792 pt |
| 上/下/左/右页边距 | 54 / 45 / 48 / 48 pt |
| 分栏 | 等宽双栏，栏间距 18 pt |
| 正文 | Times New Roman，10 pt，两端对齐，单倍行距 |
| 正文段落 | 首行缩进 12 pt，段前/段后均为 0 pt |
| 参考文献 | Times New Roman，8 pt，悬挂缩进 14 pt |
| 引文显示 | 行内显示，10 pt |

**脚本会处理所有节，并重置正文的加粗、斜体和上标格式。** 若第一段以 `I.` 开头，会应用特定标题格式。复杂论文、公式、混合版式或需要保留原有强调格式的文档，应采用更精细的局部排版流程。脚本不保证页数或期刊合规性；按样稿学习的工作流可以使用其他参数，但该脚本不会自动学习这些参数。

成功后打印文件路径、所用宏、引用/Word 字段数、段落数和页数。退出码：`0` 表示成功，`1` 表示捕获并处理的自动化/检查失败，`2` 表示缺少必填参数；未捕获的脚本运行时错误可能返回其他非零值。发生失败后应检查工作文件，不能假定脚本会自动回滚已保存的修改。

## 结果验收

完成引用任务时，应为适用项目留下实际检查依据：

- 引用记录确实属于允许的 Library/Group，并支持相邻主张。
- 元数据和发表状态已核实，或明确列为未核实。
- 存在真实 `ADDIN EN.CITE` 字段；需要参考文献表时存在 `ADDIN EN.REFLIST`。
- 临时引文已清除；分别核对引用位置数与去重后的文献数。
- EndNote 可以更新工作副本；要求的正文跳转和 DOI/URL 链接可用。
- 修改只发生在确认范围内，无关内容得到保留。
- 渲染后的 PDF 文字可读、版式正确、没有裁切遮挡。
- 原文件可恢复，交付文件与已审阅副本一致。

可使用[引用检查表](templates/citation_checklist.md)与[质量检查说明](references/09_quality_control.md)。没有完成桌面端验证时，应写明未验证，不能用 XML 标记检查替代完整 CWYW 验收。

## 常见问题

| 问题 | 建议处理 |
|---|---|
| 找不到 skill | 检查安装目录、`SKILL.md` 和重复安装，必要时开启新会话 |
| 导出文件解析失败 | 使用扁平 JSON/CSV 或基础 RIS/BibTeX，核对上述解析限制 |
| RIS 报告为 0 条记录 | 检查 `TY`/`ER` 结构，以及简易解析器要求的 `ER` 行末空格 |
| Python 脚本拒绝 `.enl`、XML 或 PDF | 先通过适当流程导出/转换，直接改后缀无效 |
| 无法确认文献库或组 | 提供准确 Library 和 Group，不应静默扩大到全库 |
| 找不到 EndNote 格式化宏 | 检查桌面 CWYW 插件，先确认 Word 中能够手动格式化引用 |
| 临时引文没有转成字段 | 核对当前库中的真实编号、作者/年份歧义和插件状态 |
| XML 与 Word 中的引用数量不同 | 区分标记出现次数、实际字段数、组合引用和唯一文献数 |
| 文后标点或 DOI 显示不正确 | 修正元数据/Output Style 后重新格式化，不手改字段结果 |
| `TYPE_E_CANTLOADLIBRARY` / `0x80029C4A` | 只有固定版式适合任务时，才采用 late-bound VBS 路径 |
| VBScript 被禁用或不可用 | 使用环境允许的其他 Word 操作方式，不全局关闭安全限制 |
| 版式或强调格式被改变 | 从备份恢复，改用局部排版流程而非整篇双栏预设 |
| 非 ASCII 字符导致终端编码错误 | 使用 `-o` 写文件，或仅对当前进程/会话设置 `PYTHONIOENCODING=utf-8` |

不要在原始文档上反复运行失败的格式化脚本。应说明失败步骤、已经完成的工作和剩余人工操作。

## 隐私与本地数据

公开示例使用通用名称与保留的 `example.org` 域名，不对应真实论文、真实 EndNote 记录编号或贡献者的个人文献库。

- `.enl`/`.Data` 文献库、论文、PDF、导出数据、备份和任务报告应保存在用户工作区，不进入公开 skill 包。
- `.gitignore` 已排除常见研究文件、构建/报告目录和凭据文件，但不能保护已被 Git 跟踪的文件、全部可能的命名形式或历史提交。
- 脚本报告可能包含输入路径、文献标题、DOI/URL 和提取的词语。分享 issue 或调试包前应单独脱敏。
- 4 个离线 Python 脚本没有联网/上传步骤。Agent 使用浏览器、云服务或文档工具时，仍取决于该环境的权限；运行本地脚本不等于所有 Agent 工作流都离线。
- 发布前检查 `git diff --cached --name-only`、`git diff --cached` 及提交作者/提交者身份。测试使用合成数据，按需采用 GitHub noreply 邮箱。
- 不提交 API Key、Token、Cookie、私钥或真实应用日志。`.gitignore` 只是辅助措施，不能替代内容审查。

## 仓库文件说明

| 文件或目录 | 用途 |
|---|---|
| [SKILL.md](SKILL.md) | Agent 入口、任务范围、流程与约束 |
| [00 — 任务配置](references/00_interactive_task_configuration.md) | 参数收集、确认与任务状态 |
| [01 — 导入](references/01_endnote_import.md) | 来源核实与 EndNote 导入流程 |
| [02 — 元数据](references/02_metadata_validation.md) | 字段完整性、发表状态和版本检查 |
| [03 — 文献组](references/03_endnote_group_workflow.md) | 组成员关系与证据范围 |
| [04 — 相关工作](references/04_related_work_writing.md) | 文献综述与组织逻辑 |
| [05 — CWYW](references/05_cwyw_citations.md) | 真实引用字段和 EndNote 格式化 |
| [06 — 超链接](references/06_word_hyperlinks.md) | 跳转、DOI 和 URL 检查 |
| [07 — 风格学习](references/07_style_learning.md) | 语言特点与风格档案 |
| [08 — Word 格式](references/08_word_formatting.md) | 保留文档样式及受保护内容 |
| [09 — 质量检查](references/09_quality_control.md) | 交付验收 |
| [10 — 示例](references/10_examples.md) | 使用提示与任务配置行为示例 |
| [11 — 语义引用](references/11_existing_text_semantic_citation.md) | 逐句证据匹配与有限修改 |
| [12 — 双栏工作流](references/12_ral_two_column_semantic_cwyw.md) | 样稿检查、CWYW 转换、排版及复核 |
| [scripts/](scripts/) | 4 个离线 Python 脚本与可选 VBS 排版脚本 |
| [templates/](templates/) | 文献分析表、综述大纲、风格档案、引用检查表与双栏任务配置表 |

## 参与维护

保持说明可移植，示例使用合成数据。提交变更时说明影响的是 Agent 行为、离线报告还是桌面自动化。使用小规模非私有数据验证修改过的脚本，检查 Markdown 链接和 `git diff --check`。修改 CWYW 流程时，注明实际测试的 Word/EndNote 环境，以及在工作副本上验证了什么。语法检查或模拟 DOCX 测试不能证明真实桌面集成可用。

EndNote 和 Microsoft Word 均为外部产品。仓库不提供其软件、许可证、输出样式或期刊官方模板。
