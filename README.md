# Head & Neck Imaging Research Skills

面向头颈部影像研究课题组的 Codex 科研 Skills 项目。覆盖文献检索、研究设计、DICOM 数据治理、MRI/PET/CT/DECT、配准、标注、放射组学、深度学习、统计、论文写作、PPT、投稿预审和审稿回复。

本项目由 Nature Skills、Radiology Skills 和头颈影像专用领域层组成。所有专业建议均用于科研设计与报告，不提供个体患者诊断或治疗建议。

## 快速安装

### Windows / PowerShell

```powershell
git clone https://github.com/wxY-55-HUB/-codex-skills.git
Set-Location '.\-codex-skills'
powershell -ExecutionPolicy Bypass -File .\scripts\install.ps1
```

如果已经存在同名 Skills，可先查看将执行的操作：

```powershell
.\scripts\install.ps1 -DryRun
```

确认后备份旧版本并更新：

```powershell
.\scripts\install.ps1 -Force
```

### macOS / Linux

```bash
git clone https://github.com/wxY-55-HUB/-codex-skills.git
cd ./-codex-skills
bash scripts/install.sh
```

覆盖更新前可执行 `bash scripts/install.sh --dry-run`，确认后执行 `bash scripts/install.sh --force`。安装完成后请开启新的 Codex 会话。

也可以直接在 GitHub 页面选择 **Code → Download ZIP**，解压后运行相应安装脚本。

## 使用入口

复杂任务优先调用总路由：

```text
使用 $hn-imaging-research，基于NPC MRI-PET数据设计完整研究，
包括文献证据、队列设计、配准QC、模型验证、统计和论文框架。
```

影像技术任务优先调用：

```text
使用 $radiology-skills，审查MRI-PET配准、肿瘤/淋巴结标注、
放射组学泄漏、外部验证和临床转化证据。
```

常用专项入口：

| 任务 | Skill |
|---|---|
| 文献检索与 MeSH | `nature-academic-search` |
| 系统综述与偏倚评估 | `hn-imaging-evidence-review` |
| 全文中英文精读 | `nature-reader` |
| 论文生成 PPTX | `nature-paper2ppt` |
| 研究方案与样本量假设 | `hn-imaging-study-design` |
| DICOM 去标识与数据共享 | `hn-imaging-data-governance` |
| 配准/标注/放射组学/深度学习 | `radiology-skills` |
| 统计设计与报告 | `nature-statistics` / `radiology-stats` |
| 论文起草与英文润色 | `nature-writing` / `nature-polishing` |
| 投稿前模拟审稿 | `nature-reviewer` / `radiology-prereview` |
| 审稿意见逐点回复 | `nature-response` / `radiology-response` |

## 技能架构

```text
hn-imaging-research                 课题组统一入口
├── hn-imaging-*                    头颈影像专用研究、综述、设计和治理
├── radiology-skills                医学影像技术总路由
│   └── modules/                    22个放射学专业内部模块
├── nature-*                        17个科研产出工作流
└── _shared/head-neck-imaging/      术语、证据、报告和影像QC规范
```

领域层重点约束：

- 区分患者、检查、原发肿瘤、颈部层区和单个淋巴结。
- 明确固定/移动影像、CT bridge、变换组合与局部配准 QC。
- 防止切片级、病灶级和预处理/特征选择造成的数据泄漏。
- 区分内部、时间、地域、设备和真正独立的外部验证。
- 分开报告图像相似性、空间准确性、预测、校准和临床效用。
- 不虚构队列、参数、伦理号、结果、引用、实验或合规状态。

详细说明见 [docs/head-neck-imaging-adaptation.md](docs/head-neck-imaging-adaptation.md)。

## 更新

```bash
git pull
```

随后重新运行安装脚本。使用 `--force` / `-Force` 时，安装器会先把已有同名技能移动到带时间戳的备份目录，再安装新版本。

## 本地验证

验证全部 Skills 和相对引用：

```bash
python scripts/validate_skills.py
python -m compileall -q skills
```

项目当前包含 44 个 `SKILL.md`，包括 17 个 Nature 工作流、4 个头颈影像专用 Skills、1 个 Radiology 总路由及其 22 个内部模块。

## 数据安全

不要向仓库提交患者 DICOM、NIfTI、ROI、临床表格、访问密钥或含可识别信息的截图。去标识化脚本和 Skills 不能替代医院伦理、信息安全和数据出境/共享审核。

## 许可证与来源

本仓库整体按 [GPL-3.0](LICENSE) 发布。第三方组件继续保留其原始许可证和署名，详见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
