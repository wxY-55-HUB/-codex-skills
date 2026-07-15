# 生境分析 Skills 使用说明

本说明面向头颈部肿瘤 MRI、PET/CT、PET/MRI、CT/DECT 的影像生境（habitat）研究，覆盖课题设计、纵向配准、亚区定义、特征提取、统计建模、论文图表、写作和投稿审查。

参考案例：[《如何用 radiology-skills 完成生境分析及实例结果展示》](https://mp.weixin.qq.com/s/Y3Hvc2HwT10Ogbfil68x0Q)。案例的关键前提是**已经具备生境特征表格**；Skills 再完成统计、建模、作图和论文交付。若只有原始影像，仍需先实现配准、分割、生境划分和特征提取代码。

## 1. 正确调用方式

生境分析能力位于 `radiology-skills` 的内部模块中。安装后从 `$radiology-skills` 总入口调用，并在提示词中指定需要加载的模块。

```text
使用 $radiology-skills，加载 radiology-design、radiology-annotation、
radiology-radiomics、radiology-stats 和 radiology-figure 内部模块，
完成鼻咽癌 MRI 纵向生境分析。
```

复杂的全流程项目也可先调用 `$hn-imaging-research`，由其路由到 `$radiology-skills`。内部模块不是单独安装的顶层 Skill，不要把 `$radiology-radiomics` 等写成独立 Skill 名称。

## 2. 各模块在生境分析中的作用

| 环节 | 内部模块 | 交付物 |
|---|---|---|
| 研究问题与队列 | `radiology-design` | 研究卡、临床终点、队列角色、验证设计 |
| 肿瘤及亚区标注 | `radiology-annotation` | ROI/VOI SOP、读者协议、几何 QC、重复性方案 |
| 配准与影像技术审查 | `radiology-skills` 头颈适配层 | fixed/moving 定义、变换链、局部 QC、插值审计 |
| 生境特征工程 | `radiology-radiomics` | IBSI 参数、特征表、稳定性与泄漏审计 |
| 模型与统计 | `radiology-stats` | 分布比较、AUC/DeLong、校准、DCA、生存模型、CI |
| 深度生境模型 | `radiology-deep-learning` | 模型输入、融合、训练、外部验证、解释与不确定性 |
| 论文图表 | `radiology-figure` | 生境图、热图、Sankey、ROC、DCA、KM、贡献图 |
| 报告规范 | `radiology-reporting` | CLEAR、IBSI、TRIPOD+AI、REMARK/STROBE 合规审计 |
| 论文与返修 | `radiology-writing`、`radiology-prereview`、`radiology-response` | 稿件、模拟审稿和逐点回复 |

## 3. 生境研究卡

运行分析前先固定以下事实；缺失项写 `AUTHOR_INPUT_NEEDED`，不要猜测。

```yaml
disease: 鼻咽癌/口咽癌/喉癌/其他
clinical_decision: 远处转移/疗效/复发/生存/分子表型
analysis_unit: 患者/原发灶/淋巴结
imaging:
  modalities: [MRI]
  sequences: [T1WI, T2WI, DWI, CE-T1WI]
  timepoints: [Pre, Post]
  fixed_image: 待填
  moving_image: 待填
  registration_chain: 待填
segmentation:
  target: 原发灶/淋巴结
  readers_and_blinding: 待填
habitat:
  construction_method: k-means/GMM/模糊聚类/超体素/深度模型/其他
  input_maps: 待填
  number_of_habitats: 待填并说明依据
  label_alignment_rule: 待填
cohort:
  training_n: 待填
  internal_test_n: 待填
  external_validation_n: 待填
  centers: 待填
endpoint:
  definition: 待填
  landmark: 待填
  horizon: [3年, 5年]
  events: 待填
target_venue: 待填
```

## 4. 两种使用模式

### 模式 A：已有生境特征表

这是参考文章采用的模式。最少需要：

- `patient_id`：去标识化患者编号；
- `cohort`：training/internal_test/external_validation；
- `outcome`：分类标签或生存结局；
- `habitat`：生境编号及一致的生物学定义；
- `timepoint`：Pre/Post 或具体扫描时点；
- 特征列：体积分数、强度、纹理、形状、delta 或生境评分；
- 生存任务：随访时间、事件和 landmark 定义；
- 必要协变量：年龄、性别、分期、治疗等。

推荐提示词：

```text
使用 $radiology-skills，加载 radiology-stats 和 radiology-figure。
读取我提供的生境特征表、数据字典和患者级划分文件。先审计患者重叠、
缺失值、异常值、特征定义和结局编码，再在训练集完成特征筛选与建模，
冻结流程后评估内部测试集和外部验证集。输出统计表、绘图脚本、SVG/PDF、
300 dpi PNG/TIFF、Methods/Results 草稿和风险清单。不要虚构结果。
```

### 模式 B：只有原始影像、配准影像或肿瘤掩膜

Skills 可以设计和审查流程，也可以在依赖环境齐备时编写/运行代码，但不能仅凭提示词自动得到可靠生境。应先完成：

1. DICOM 去标识化与序列核验；
2. 纵向/多模态配准及局部 QC；
3. 肿瘤 ROI/VOI 标注和几何一致性检查；
4. 影像预处理、重采样、强度标准化和离散化；
5. 体素/超体素表征与生境划分；
6. 跨患者、跨时间点的生境标签对齐；
7. 生境特征提取和重复性筛选；
8. 生成患者级特征表后再进入模式 A。

推荐提示词：

```text
使用 $radiology-skills，加载 radiology-design、radiology-annotation 和
radiology-radiomics。为治疗前后 NPC MRI 设计生境生成流程。明确 fixed/moving
图像、变换与插值、肿瘤掩膜传播、局部配准 QC、预处理参数、聚类输入、
生境数选择、标签对齐、稳定性分析和输出表结构。先给方案和风险，不使用
未经检查的原始 DICOM 直接建模。
```

## 5. 标准全流程

### 5.1 固定临床问题和验证结构

- 先明确生境指标要支持的临床决策，而不是先聚类再寻找显著结局。
- 规定主要终点、landmark、预测时间窗、主要比较和协变量。
- 所有数据划分必须在患者级完成。
- 训练、内部测试和外部验证的角色在分析前固定。
- 事件数而非患者总数通常是生存模型的限制因素。

### 5.2 配准与掩膜 QC

纵向生境分析必须记录 fixed/moving 图像、刚性/仿射/形变配准顺序、变换组合和插值方式。对头颈部至少检查鼻咽腔、咽旁间隙、颅底、椎体以及咽后和颈部淋巴结区域。

- 图像插值可用连续插值；标签/掩膜必须使用标签安全的最近邻等方法。
- 配准误差不能被解释为治疗后生境变化。
- 若存在 PET/CT bridge，必须分别报告 MRI→CT 和 CT→PET 的误差与失败病例。
- 检查 spacing、origin、direction、slice order 及掩膜是否越界。

### 5.3 生境构建

必须预先说明：

- 输入是原始序列、参数图、delta 图、滤波图还是深度特征；
- 以体素、超体素、病灶还是患者为聚类单位；
- 聚类算法、距离、初始化、随机种子和生境数选择依据；
- 是否在训练集学习中心/原型，再应用于测试集；
- 如何处理肿瘤大小、部分容积、坏死和空洞；
- 如何使 Habitat 1/2/3 在患者、中心和时间点之间具有一致含义。

聚类编号本身没有生物学顺序。不能因为软件输出为 1/2/3，就直接命名为“高危核心区、中间区、外围区”。命名必须来自可复现的特征定义、空间关系、结局关联或独立生物学证据。

### 5.4 特征提取与稳定性

- 记录重采样、强度标准化、固定 bin width/count、滤波器和软件版本。
- 分别报告每个生境的体积、体积分数、位置和放射组学特征。
- 纵向研究可计算 `Post − Pre`、相对变化或预设的 delta 指标，但须统一分母和零值规则。
- 用重复标注、扰动或测试—重测评估生境和特征稳定性。
- ICC、方差过滤、相关性过滤、标准化、缺失值填补、ComBat、LASSO/mRMR 等必须只在训练数据或训练折内拟合。

### 5.5 建模与验证

推荐最小模型阶梯：

1. 临床基线模型；
2. 常规全肿瘤影像组学模型；
3. 生境模型；
4. 临床 + 生境联合模型；
5. 仅在样本量和事件数允许时增加复杂融合模型。

分类任务报告 AUC、敏感度、特异度、校准和 DCA；生存任务报告 C-index、时间依赖 AUC、校准、DCA、KM/风险人数，并检验 Cox 假设。所有主要估计量给出 95% CI。模型比较须与设计匹配，并在外部队列使用冻结流程。

## 6. 参考案例的九类图及所需数据

文章展示的是局部晚期鼻咽癌诱导治疗前后 MRI 生境特征预测远处转移风险。可复用的图件结构如下；文章中的数值仅属于该案例，不能复制到新课题。

| 图件 | 科学问题 | 最少输入 |
|---|---|---|
| 生境影像示意图 | 亚区在哪里、如何定义 | 去标识化影像、mask、生境标签图、窗宽窗位 |
| 分组箱线/散点图 | 各队列中指标是否稳定关联结局 | 特征、结局、cohort、患者 ID |
| Pre/Post 体积分数堆叠图 | 各生境随治疗如何变化 | 时间点、生境体积/分数、结局 |
| 标准化特征热图 | 特征是否形成连续梯度或分群 | 训练集特征矩阵和排序评分 |
| Sankey 重分类图 | 生境能否补充传统临床分层 | 临床风险、生境风险、联合分层、cohort |
| 3/5 年 ROC | 生境是否提高时间点预测 | 生存时间、事件、预测值、cohort |
| 决策曲线 | 增量信息是否产生净获益 | 真实结局、预测概率、时间窗 |
| Kaplan–Meier + 风险表 | 风险分层是否区分 DMFS/PFS/OS | 时间、事件、风险组、cohort |
| 蜂群/贡献图 | 哪些特征提高或降低评分 | 特征值和可解释模型贡献值 |

作图时要求统一患者集、终点、时间窗和模型版本；导出可编辑 SVG/PDF 以及 ≥300 dpi 位图，并检查图中人数、AUC、P 值和风险表与统计结果逐项一致。

## 7. 推荐数据结构

```text
habitat-project/
├── 00_protocol/
│   ├── study_card.yaml
│   └── statistical_analysis_plan.md
├── 01_images_deidentified/
├── 02_masks/
│   ├── whole_tumour/
│   └── habitats/
├── 03_registration_qc/
├── 04_features/
│   ├── habitat_features.csv
│   └── feature_dictionary.csv
├── 05_clinical/
│   └── clinical_deidentified.csv
├── 06_splits/
│   └── patient_splits.csv
├── 07_analysis/
│   ├── configs/
│   ├── scripts/
│   └── environment.yml
├── 08_results/
│   ├── tables/
│   └── figures/
└── 09_manuscript/
    ├── supplement/
    └── availability.md
```

建议特征表至少包含：

```text
patient_id, cohort, center, timepoint, habitat_id, outcome,
followup_time, event, feature_name/value 或宽表特征列
```

另建 `feature_dictionary.csv` 说明单位、计算公式、Pre/Post 方向、delta 定义、缺失值编码和软件版本。

## 8. 可直接复制的完整提示词

```text
使用 $radiology-skills 完成头颈部 MRI 生境分析。加载内部模块
radiology-design、radiology-annotation、radiology-radiomics、radiology-stats、
radiology-figure、radiology-reporting 和 radiology-writing。

研究问题：[填写]
疾病与队列：[填写]
序列和时间点：[填写]
生境定义/现有输入：[填写]
终点和预测时间窗：[填写]
训练、内部测试、外部验证：[填写]
目标期刊：[填写]

先完成研究卡和数据完整性审计，重点检查患者级划分、配准误差、mask 几何、
生境标签一致性、训练集外信息泄漏、事件数和外部验证。若已有生境特征表，
生成可复现的统计代码、结果表和九类候选图件；若只有影像，先输出生境生成、
QC、特征提取和数据表方案。最终提供 Methods/Results 骨架、CLEAR/IBSI/
TRIPOD+AI 或 REMARK/STROBE 合规清单。只使用真实数据，未知项标记
AUTHOR_INPUT_NEEDED，不得生成示例数值冒充研究结果。
```

## 9. 投稿前红线

- 不得把切片、病灶或时间点随机拆到不同患者级数据集。
- 不得用全队列确定聚类原型、标准化参数、筛选特征或最优阈值后再声称独立验证。
- 不得把配准伪影或肿瘤缩小造成的边界变化直接解释为生物学异质性。
- 不得让 Habitat 1/2/3 在不同患者或时间点代表不同含义。
- 不得只报告 AUC 或显著 P 值而不报告 CI、校准、临床效用和失败病例。
- 不得把统计关联升级为因果机制。
- 不得复制参考案例中的人数、AUC、P 值、DMFS 或图形数据。
- 不得向仓库或模型提交含直接标识符的 DICOM、截图、临床表或访问凭据。

## 10. 最小交付清单

- 预注册式研究卡、终点和患者级划分；
- fixed/moving 图像、变换链、插值和局部配准 QC；
- ROI/VOI 与生境构建 SOP；
- 生境数选择及跨患者/时间点标签对齐规则；
- IBSI 参数文件、特征字典和软件版本；
- 稳定性、ICC、泄漏和事件数审计；
- 临床、全肿瘤、生境和联合模型的公平比较；
- 带 95% CI 的内部和外部验证、校准及 DCA；
- 图件脚本、SVG/PDF、≥300 dpi 位图及图—源数据对应表；
- Methods、Results、Supplement、Data/Code Availability 和报告清单。

