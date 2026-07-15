# Head & Neck Imaging Research Skills

面向头颈部影像研究课题组的 Codex 科研 Skills 项目。以 Nature Skills 和 Radiology Skills 为通用底座，加入鼻咽癌及其他头颈肿瘤 MRI、PET/CT、PET/MRI、CT/DECT、超声、影像组学、影像生境、多模态 AI、虚拟影像和临床预测研究所需的领域约束。

项目覆盖完整科研链条：文献检索与精读、研究设计、DICOM/NIfTI 数据治理、配准与标注、影像组学、生境分析、深度学习、放射基因组学、统计、科研绘图、论文/PPT、投稿预审、审稿回复、基金申请、实验记录和专利撰写。

> 本项目服务于科研设计、分析、复现和学术写作，不提供个体患者诊断或治疗建议，也不能替代医院伦理、信息安全、生物统计或临床专业审查。

## 目录

- [快速安装](#快速安装)
- [Skills 如何工作](#skills-如何工作)
- [一分钟选择入口](#一分钟选择入口)
- [22 个顶层 Skills](#22-个顶层-skills)
- [Radiology 的 22 个内部模块](#radiology-的-22-个内部模块)
- [按科研阶段使用](#按科研阶段使用)
- [常见课题组合](#常见课题组合)
- [建议输入格式](#建议输入格式)
- [输出、执行与能力边界](#输出执行与能力边界)
- [数据安全](#数据安全)
- [更新、验证与故障排查](#更新验证与故障排查)

## 快速安装

### 方法一：Git 克隆

Windows PowerShell：

```powershell
git clone https://github.com/wxY-55-HUB/-codex-skills.git
Set-Location '.\-codex-skills'
powershell -ExecutionPolicy Bypass -File .\scripts\install.ps1
```

macOS/Linux：

```bash
git clone https://github.com/wxY-55-HUB/-codex-skills.git
cd ./-codex-skills
bash scripts/install.sh
```

### 方法二：下载 ZIP

1. 在 GitHub 仓库页面选择 **Code → Download ZIP**；
2. 解压 ZIP；
3. 在解压后的项目根目录运行对应安装脚本；
4. 安装完成后重新打开一个 Codex 任务，使 Skills 被重新发现。

### 安装位置

安装脚本默认复制到：

- 已设置 `CODEX_HOME`：`$CODEX_HOME/skills`
- 未设置 `CODEX_HOME`：`~/.codex/skills`

指定其他安装目录：

```powershell
.\scripts\install.ps1 -Destination 'D:\codex\skills'
```

```bash
bash scripts/install.sh --destination /path/to/codex/skills
```

预览安装行为：

```powershell
.\scripts\install.ps1 -DryRun
```

```bash
bash scripts/install.sh --dry-run
```

备份旧版本并覆盖安装：

```powershell
.\scripts\install.ps1 -Force
```

```bash
bash scripts/install.sh --force
```

覆盖时，旧版本会进入带时间戳的 `skills-backups/` 目录。

## Skills 如何工作

Skill 是一组供 Codex 按需读取的专业工作流、参考资料、脚本和模板。它不会一直把全部内容放进上下文，而是分层加载：

1. Codex 根据 Skill 名称和描述判断是否触发；
2. 触发后读取对应 `SKILL.md`；
3. 仅在需要时继续读取 `references/`、执行 `scripts/` 或使用 `assets/`。

### 自动触发

可以直接用自然语言描述任务：

```text
请检索近五年鼻咽癌 MRI 影像组学预测远处转移的研究，并整理证据表。
```

Codex 会根据任务选择文献检索、头颈领域和放射学模块。

### 显式触发

在 Skill 名称前加 `$`，可以明确要求使用某个顶层 Skill：

```text
使用 $hn-imaging-research，为 NPC MRI-PET 项目建立从文献到投稿的完整路线。
```

```text
使用 $nature-paper2ppt，把这篇论文制作成中文组会 PPTX。
```

### 顶层 Skill 与内部模块

安装后可直接显式调用的是 `skills/` 下具有 `SKILL.md` 的 22 个顶层目录。

`radiology-skills/modules/` 下的 22 个模块属于内部专业模块，应通过 `$radiology-skills` 调用：

```text
使用 $radiology-skills，加载 radiology-radiomics、radiology-stats 和
radiology-figure 内部模块，分析我的生境特征表并生成论文图。
```

不要把内部模块误写成独立安装的 `$radiology-radiomics`。完整安装 `skills/` 目录，不要只复制单个 `SKILL.md`，否则 `_shared/` 领域资料和内部引用会丢失。

### 使用原则

- 优先调用能完成任务的最小 Skill 组合；
- 复杂项目先用 `$hn-imaging-research` 路由；
- 影像技术任务优先用 `$radiology-skills`；
- 输入真实数据、表格、稿件或结果，不让 Skill 猜测研究事实；
- 缺失信息使用 `AUTHOR_INPUT_NEEDED`，不虚构患者数、参数、伦理号、指标、P 值或引用；
- “最新”“当前指南”“期刊要求”应联网核验官方来源。

## 一分钟选择入口

| 你要完成的任务 | 推荐入口 |
|---|---|
| 不知道该用哪个 Skill，或任务跨多个阶段 | `$hn-imaging-research` |
| 头颈影像技术、影像组学、AI、统计或投稿 | `$radiology-skills` |
| 文献检索、MeSH、引文管理 | `$nature-academic-search` |
| 系统综述/范围综述 | `$hn-imaging-evidence-review` |
| 全文精读和中英对照 | `$nature-reader` |
| 论文制作组会 PPTX | `$nature-paper2ppt` |
| 研究方案、队列和验证设计 | `$hn-imaging-study-design` |
| DICOM/NIfTI、去标识和数据发布 | `$hn-imaging-data-governance` |
| 统计分析与报告 | `$nature-statistics` 或 `$radiology-skills` |
| 论文框架和段落写作 | `$nature-writing` |
| 英文学术润色 | `$nature-polishing` |
| 论文绘图 | `$nature-figure` 或 `$radiology-skills` |
| 投稿前模拟审稿 | `$nature-reviewer` 或 `$radiology-skills` |
| 审稿意见逐点回复 | `$nature-response` 或 `$radiology-skills` |
| 基金/课题申请 | `$nature-proposal-writer` 或 `$radiology-skills` |
| 论文转中国发明专利 | `$nature-paper-to-patent` |

## 22 个顶层 Skills

### 头颈部影像专用入口

| Skill | 功能 | 典型请求 |
|---|---|---|
| `hn-imaging-research` | 课题组总路由；协调文献、设计、影像技术、统计、论文、PPT、投稿和数据共享 | “为 NPC MRI-PET 项目制定完整科研路线” |
| `hn-imaging-study-design` | 固定临床问题、队列角色、患者级划分、终点、样本量假设和验证策略 | “设计多中心 NPC 远处转移预测研究” |
| `hn-imaging-evidence-review` | 系统综述/范围综述、检索框架、筛选、提取、偏倚评估和证据综合 | “系统评价 MRI 影像组学预测 NPC 预后” |
| `hn-imaging-data-governance` | DICOM/NIfTI 去标识、患者链接、权限、共享、发布 QC 和追溯 | “建立头颈影像数据发布流程” |

### 文献、阅读与引用

| Skill | 功能 | 典型请求 |
|---|---|---|
| `nature-academic-search` | PubMed/Crossref/arXiv/Scopus/ScienceDirect 多源检索、MeSH、去重和引用文件管理 | “检索 2021—2026 年虚拟 PET 研究并导出 RIS” |
| `nature-literature-pipeline` | 文献发现、评分、精读、格式化推送和归档的持续工作流 | “建立每周 NPC 影像 AI 文献监测” |
| `nature-downloader` | 合法开放获取或机构授权全文下载、文献整理和 PDF 读取 | “通过学校授权整理这批论文全文” |
| `nature-reader` | 全文中英对照、图表感知、段落级来源锚定的论文阅读 | “精读这篇 PET/MRI 论文并保留图表位置” |
| `nature-citation` | 为论文主张查找支持文献、建立主张—引用映射并导出引用文件 | “给讨论部分补充 Nature/CNS 体系引用” |
| `nature-ref-verifier` | 核对 DOI/PMID、作者、期刊、年份及正文主张是否被引用真正支持 | “检查参考文献是否真实且引用位置正确” |

### 写作、PPT、审稿与产出

| Skill | 功能 | 典型请求 |
|---|---|---|
| `nature-writing` | 从结果、图表和作者笔记起草标题、摘要、引言、方法、结果、讨论和结论 | “根据真实结果起草 Methods 和 Results” |
| `nature-polishing` | 学术英语翻译、润色、重构及 LaTeX 排版问题处理 | “将中文讨论润色为投稿级英文” |
| `nature-paper2ppt` | 从论文/PDF 制作中文学术汇报 PPTX，并执行图像和溢出 QA | “把这篇论文做成 20 页组会 PPT” |
| `nature-reviewer` | 投稿前从审稿人角度生成多份评审报告和综合结论 | “模拟审稿并找出方法学硬伤” |
| `nature-response` | 编辑决定和审稿意见分类、逐点回复、修改定位和证据闭环 | “生成大修回复信和 revision tracker” |
| `nature-figure` | Python/R 论文图、多面板图、SVG/PDF/TIFF 导出和质量检查 | “绘制论文 ROC、校准和 DCA 图” |
| `nature-statistics` | 统计方案、检验选择、结果复核和投稿级报告 | “审查这份预测模型统计分析” |
| `nature-data` | 数据/代码可用性声明、仓库选择、FAIR 和数据引用 | “撰写 Data Availability 和 Code Availability” |
| `nature-proposal-writer` | 证据优先、论证优先的科研计划书/基金申请写作与修订 | “将现有结果整理成基金申请框架” |
| `nature-experiment-log` | 将图、文字和实验记录整理为带 YAML frontmatter 的标准日志 | “把今天的配准实验整理成可追溯日志” |
| `nature-paper-to-patent` | 从论文、代码和图表提取可专利点，生成中文发明专利材料 | “将虚拟 PET 方法转成中国发明专利初稿” |

### 放射学总入口

| Skill | 功能 | 典型请求 |
|---|---|---|
| `radiology-skills` | 放射学/医学影像 AI 总路由，包含 22 个内部模块 | “审计 MRI-PET 配准、影像组学、AI 验证和投稿材料” |

## Radiology 的 22 个内部模块

这些模块由 `$radiology-skills` 按任务加载，无需单独安装。

| 内部模块 | 主要用途 |
|---|---|
| `radiology-frontier` | 前沿方向、创新缺口、近年高水平证据模式 |
| `radiology-design` | 数据可行性、临床问题、终点和内/外部验证设计 |
| `radiology-search` | 放射学文献、DOI/PMID 和公共数据集检索 |
| `radiology-annotation` | ROI/VOI/mask、读者协议、ICC/Dice/HD 和几何 QC |
| `radiology-data` | DICOM 去标识、存储库、数据/代码共享和 FAIR |
| `radiology-ethics` | 伦理、知情同意、隐私、数据使用和再识别风险 |
| `radiology-radiomics` | IBSI/CLEAR 影像组学预处理、特征提取、选择和泄漏审计 |
| `radiology-deep-learning` | CNN/Transformer/基础模型、训练、解释、不确定性和 OOD |
| `radiology-radiogenomics` | 影像—基因组/转录组/单细胞/空间组学整合 |
| `radiology-stats` | AUC/DeLong、ICC、MRMC、校准、DCA、生存和高维统计 |
| `radiology-figure` | 放射学论文图、影像面板和可编辑矢量导出 |
| `radiology-reporting` | CLAIM、CLEAR、IBSI、TRIPOD+AI、STARD、PROBAST 等 |
| `radiology-writing` | Radiology/Nature/npj/Lancet/European Radiology 稿件写作 |
| `radiology-polishing` | 放射学论文英语、统计表达和目标期刊文风 |
| `radiology-reader` | 放射学论文全文精读和结构化阅读笔记 |
| `radiology-citation` | 放射学主张检索、引用核验和引用文件导出 |
| `radiology-prereview` | 投稿前方法学、报告和可复现性模拟审查 |
| `radiology-journal` | 期刊匹配、投稿梯队和补强建议 |
| `radiology-response` | 影像研究审稿回复、补分析和修改追踪 |
| `radiology-translation` | 读者研究、前瞻验证、阈值、部署和临床效用 |
| `radiology-grant` | 国自然、省级及国际基金的影像 AI 项目写作 |
| `radiology-paper2ppt` | 放射学论文中文组会 PPT 规划与 QA |

## 按科研阶段使用

### 1. 选题与证据地图

```text
使用 $hn-imaging-research，为“多参数 MRI 预测 NPC 诱导化疗疗效”建立
证据地图。先用文献检索确认研究现状，再给出临床问题、创新缺口、可行方法、
验证要求和最大风险。不要把流行模型直接等同于创新。
```

推荐组合：

```text
hn-imaging-research
→ nature-academic-search / radiology-search
→ radiology-frontier
→ hn-imaging-study-design / radiology-design
```

### 2. 文献综述与精读

```text
使用 $hn-imaging-evidence-review，设计 MRI 影像组学预测 NPC 远处转移的
系统综述。输出 PICO/检索式、数据库、纳排标准、提取表、偏倚工具和综合方案。
```

```text
使用 $nature-reader，精读这篇 PET/MRI 论文，生成中英对照 Markdown；
保留图表位置，并提取队列、序列、配准、分割、模型、验证和局限性。
```

### 3. 研究设计与统计分析计划

```text
使用 $hn-imaging-study-design，设计多中心 NPC MRI 预测复发的研究。
明确患者级纳排、主要终点、分析单位、训练/验证角色、样本量假设、
时间/地域外部验证和数据泄漏控制。未知信息标记 AUTHOR_INPUT_NEEDED。
```

输出应至少包含研究卡、可行性判断、主要分析、验证计划、样本量问题和下一步数据收集优先级。

### 4. DICOM/NIfTI 数据治理

```text
使用 $hn-imaging-data-governance，为头颈 MRI/PET/CT 数据建立去标识、
匿名 ID 映射、权限分层、DICOM→NIfTI、mask 版本、质控和对外发布流程。
```

重点检查 DICOM header、burned-in PHI、头面部再识别、时间信息、ROI/RTSTRUCT/DICOM-SEG 和患者链接表。AI 不应直接接触未授权原始患者信息。

### 5. 配准、标注和分割

```text
使用 $radiology-skills，审查 NPC MRI-PET/CT bridge 配准。
明确 fixed/moving 图像、变换顺序、插值、mask 传播、鼻咽腔/咽旁间隙/
颅底/淋巴结局部 QC、失败标准和输出溯源。
```

```text
使用 $radiology-skills，加载 radiology-annotation，建立原发灶和淋巴结
三维标注 SOP，包括读者资历、盲法、复核、几何检查和重复性分析。
```

### 6. 影像组学与生境分析

```text
使用 $radiology-skills，加载 radiology-radiomics、radiology-stats 和
radiology-reporting，审查 PyRadiomics 流程。重点检查重采样、强度标准化、
bin width、ICC、LASSO、患者级划分、训练折内处理和外部验证。
```

生境分析需要区分两种模式：

- 已有生境特征表：进入统计、建模、作图和写作；
- 只有原始影像：先完成配准、ROI、生境构建、标签对齐、稳定性和特征提取。

完整说明：[docs/habitat-analysis-skills-guide.md](docs/habitat-analysis-skills-guide.md)。

### 7. 深度学习和多模态 AI

```text
使用 $radiology-skills，加载 radiology-deep-learning。为 MRI+PET+临床变量
预测 NPC 远处转移设计 3D 模型。给出公平基线、输入与融合、缺失模态、
患者级划分、嵌套调参、外部验证、校准、不确定性、OOD 和失败病例分析。
```

重要原则：模型容量由患者数、事件数和外部验证决定；切片数不能代替患者数；测试集不能用于阈值、特征或 checkpoint 选择。

### 8. 放射基因组学与多组学

```text
使用 $radiology-skills，加载 radiology-radiogenomics 和 radiology-stats。
设计 NPC MRI 影像表型联合 bulk RNA-seq 研究。先计算 matched_n，建立
样本—影像—时间映射，再规定组学 QC、批次/中心协变量、FDR、通路分析、
独立验证和生物学验证。
```

只有影像和组学同时合格的交集样本量才是有效样本量。组织取材与影像区域/时间不匹配时，只能作受限关联解释，不能直接声称机制。

### 9. 统计与论文图

```text
使用 $radiology-skills，加载 radiology-stats 和 radiology-figure。
读取真实预测结果，计算 AUC及95%CI、DeLong、校准、Brier 和 DCA；
生成 SVG/PDF 和 300 dpi PNG/TIFF，并给出 Methods/Results 句子。
```

如果调用 `$nature-figure`，而用户尚未指定 Python 或 R，应先选择后端。图表只能来自真实数据；示例数据必须明确标注为模拟。

### 10. 论文起草、引用和润色

```text
使用 $nature-writing，根据研究卡、图表、统计结果和作者笔记起草论文。
先建立一条核心论证线和 claim–evidence map，再写摘要、引言、方法、结果和讨论。
```

```text
使用 $nature-citation，为讨论部分逐段寻找支持文献；随后使用
$nature-ref-verifier 核对 DOI、PMID、题录和主张支持关系。
```

```text
使用 $nature-polishing，将已完成的英文稿润色为目标期刊风格，不改变数字、
结论边界或引用含义。
```

### 11. PPT 与组会汇报

```text
使用 $nature-paper2ppt，把这篇论文制作成中文组会 PPTX。
听众为头颈影像课题组，重点讲临床问题、队列、序列/配准/分割、模型、
结果、局限和可复用思路；生成演讲备注并检查图像清晰度与文本溢出。
```

如果是放射学论文，可通过 `$radiology-skills` 加载 `radiology-paper2ppt`，强化影像技术和方法学审查。

### 12. 投稿前审查、选刊与返修

```text
使用 $nature-reviewer，对整篇稿件做投稿前模拟审稿，生成三份审稿报告和
跨审稿综合；重点检查创新性、队列、泄漏、外部验证、统计和过度声称。
```

```text
使用 $radiology-skills，加载 radiology-journal 和 radiology-prereview，
给出投稿梯队、匹配理由、阻断问题和投稿前补强计划。
```

```text
使用 $nature-response，将编辑信和审稿意见整理成逐点回复。
每条包括评论、回应策略、完成的修改、稿件位置、证据和仍需作者确认的信息。
```

### 13. 基金、实验日志与专利

```text
使用 $nature-proposal-writer，把现有 NPC 虚拟 PET 数据、初步实验和文献证据
组织为基金申请。先形成科学问题、假设、目标和工作包，再写正文。
```

```text
使用 $nature-experiment-log，把今天的配准参数、病例、失败原因、输出路径和
下一步计划整理为标准实验日志。
```

```text
使用 $nature-paper-to-patent，从论文、代码和流程图中提取可专利技术特征，
建立权利要求—来源证据映射，并生成中文专利初稿。
```

## 常见课题组合

### NPC MRI-PET 配准与虚拟 PET

```text
$hn-imaging-research
→ radiology-skills（design + annotation + deep-learning + stats）
→ nature-figure
→ nature-writing / nature-citation
→ nature-reviewer / nature-response
→ nature-data
```

提示词：

```text
使用 $hn-imaging-research，建立 NPC MRI→虚拟 PET 项目全流程。
将真实 PET 合成质量、MRI-PET 空间配准误差、代谢定量误差和临床预测价值
分开评估；设计患者级训练/验证、外部验证、消融、失败病例、图表和论文结构。
```

### DECT/影像组学预测 Ki-67 或病理表型

```text
$radiology-skills
→ radiology-design
→ radiology-annotation
→ radiology-radiomics
→ radiology-stats
→ radiology-reporting
→ radiology-writing
```

### 诱导治疗前后 MRI 生境预测远处转移

```text
$radiology-skills
→ radiology-design / annotation / radiomics
→ radiology-stats / figure
→ radiology-reporting / writing
→ radiology-prereview
```

详见[生境分析指南](docs/habitat-analysis-skills-guide.md)。

### 头颈影像 AI 系统综述

```text
$hn-imaging-evidence-review
→ nature-academic-search
→ nature-downloader / nature-reader
→ nature-statistics
→ nature-writing / nature-citation
→ nature-figure
```

### 论文返修

```text
$nature-response 或 $radiology-skills（radiology-response）
→ radiology-stats / figure / reporting（需要补分析时）
→ nature-polishing
→ nature-ref-verifier
```

## 建议输入格式

复杂任务建议先提供一张研究卡：

```yaml
project: 项目名称
disease_and_subsite: 鼻咽癌/口咽癌/喉癌/甲状腺/其他
clinical_question: 诊断/分期/疗效/复发/生存/生成
cohort:
  design: 回顾性/前瞻性
  centers: 待填
  patients: 待填
  events: 待填
  dates: 待填
imaging:
  modalities: [MRI, PET-CT]
  sequences_or_phases: [T1WI, T2WI, DWI, CE-T1WI]
  scanners_and_protocols: 待填
reference_standard: 待填
analysis_unit: 患者/检查/原发灶/淋巴结
annotation_and_qc: 待填
endpoint_and_horizon: 待填
split_and_validation: 待填
available_artifacts:
  - protocol
  - deidentified_table
  - code
  - results
  - manuscript
requested_output: 方案/代码/统计/图/PPT/论文/审稿回复
target_venue_or_audience: 待填
```

### 文件输入建议

- 文献任务：PDF、DOI/PMID、检索式、RIS/BibTeX/NBIB；
- 统计任务：去标识化 CSV/XLSX、数据字典、分析单位和终点定义；
- 影像任务：匿名 NIfTI/示例影像、mask、配准日志、参数配置和 QC 报告；
- 论文任务：稿件、图表、补充材料、期刊要求和参考文献；
- 返修任务：编辑信、逐条审稿意见、原稿和修改稿；
- PPT 任务：论文 PDF、听众、时长、语言和希望重点讨论的问题。

## 输出、执行与能力边界

### Skills 可以做什么

- 读取课题文件并构建研究合同；
- 检索和核验公开文献/数据源；
- 设计研究、统计和验证方案；
- 审查 DICOM/NIfTI、mask、代码、表格、模型和稿件；
- 编写、修改并在环境允许时运行 Python/R/命令行脚本；
- 生成表格、SVG/PDF/TIFF、PPTX、DOCX、Markdown 等产物；
- 建立报告清单、投稿材料和审稿回复闭环。

### Skills 不等于什么

- 不等于已经安装 ANTs、MONAI、PyTorch、PyRadiomics、R 或特定 GPU 环境；
- 不等于无需人工 QC 的“一键医学影像平台”；
- 不会在没有数据时生成真实实验结果；
- 不会自动获得医院 PACS、受控数据库或期刊账户权限；
- 不替代影像科医师、生物统计师、生信工程师、伦理委员会和信息安全人员；
- 不保证论文接收、基金中标、专利授权或临床可用。

### 推荐交付结构

```text
project/
├── 00_protocol/
├── 01_data_dictionary/
├── 02_deidentified_data/
├── 03_preprocessing_qc/
├── 04_splits/
├── 05_analysis/
│   ├── configs/
│   ├── scripts/
│   └── environment.yml
├── 06_results/
│   ├── tables/
│   └── figures/
├── 07_manuscript/
├── 08_submission/
└── 09_logs/
```

每项结果尽量保留输入来源、脚本版本、参数、随机种子、日志、运行时间和失败记录。

## 数据安全

禁止向公开仓库或未授权 AI 环境提交：

- 姓名、住院号、身份证号、手机号和完整出生日期；
- 可直接回溯患者的完整检查日期和链接表；
- 未去标识的 DICOM header、burned-in pixel PHI 或可重建面部信息；
- 未获授权的临床表、病理报告、基因组数据、ROI/mask；
- 数据库密码、访问令牌、API key、SSH 私钥和机构账户；
- 伦理、DUA 或知情同意不允许共享的文件。

建议：

1. 在医院批准环境中完成去标识化；
2. 使用随机研究 ID；
3. 将身份映射表与科研数据物理隔离；
4. 影像共享前检查 header、像素文字和头面部再识别风险；
5. 公开发布前由数据管理员、伦理和信息安全部门复核。

## 更新、验证与故障排查

### 更新项目

```bash
git pull
```

随后重新运行安装脚本；需要替换旧版本时使用 `-Force` 或 `--force`。

### 验证仓库

```bash
python scripts/validate_skills.py
python -m compileall -q skills
```

当前项目包含：

- 22 个可直接调用的顶层 Skills；
- `radiology-skills` 内部 22 个专业模块；
- 总计 44 个 `SKILL.md`；
- 共享头颈部领域词典、证据、报告和技术 QC 资料。

### Skill 没有被识别

依次检查：

1. 是否已重新打开 Codex 任务；
2. Skill 是否位于 `$CODEX_HOME/skills/<skill-name>/SKILL.md`；
3. 是否完整安装了 `_shared/` 和 `radiology-skills/modules/`；
4. 是否误把内部模块当成独立 `$skill` 调用；
5. 运行 `python scripts/validate_skills.py` 查看结构和相对链接。

### 旧版没有被覆盖

默认安装会跳过同名目录。先执行 DryRun，确认后使用：

```powershell
.\scripts\install.ps1 -Force
```

或：

```bash
bash scripts/install.sh --force
```

### 能设计但不能运行代码

检查任务需要的软件、Python/R 包、外部可执行文件、GPU/CUDA、模型权重和数据访问是否已经安装或授权。Skill 提供工作流和代码能力，但不会凭空创建运行环境。

### 输出中出现占位符

`AUTHOR_INPUT_NEEDED` 表示该事实只能由课题组提供，例如真实患者数、扫描参数、事件数、伦理号、软件版本或统计结果。补充事实后再次调用相同 Skill 完成定稿。

## 进一步说明

- [头颈部影像适配说明](docs/head-neck-imaging-adaptation.md)
- [生境分析 Skills 使用说明](docs/habitat-analysis-skills-guide.md)
- [第三方许可与来源](THIRD_PARTY_NOTICES.md)

## 许可证

本仓库整体按照 [GPL-3.0](LICENSE) 发布。第三方组件继续适用其原始许可证和署名要求，详见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。

## 许可证与来源

本仓库整体按 [GPL-3.0](LICENSE) 发布。第三方组件继续保留其原始许可证和署名，详见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
