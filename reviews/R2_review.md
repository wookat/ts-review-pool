# 8 份匿名时间序列手稿独立评审报告

评审人立场：ICML/NeurIPS 资深审稿人（Reviewer 2 风格），按 ~25% 录取率的真实顶会标准打分。对比池：`wookat/ts-review-pool`（101 篇 2024–2026 顶会论文，7 个子领域）。所有判断均给出手稿内证据（章节/表格/数字）。

材料说明：
- M1、M2、M3、M4 来自 GitHub 仓库中的编译版 `main.pdf`（M2 的 PDF 初始被 .gitignore 误排除，后由作者方推送 commit 9d610e2 补齐，本版评审基于该完整 6 页 PDF；稿件仍自标 "Anonymous submission (working draft)"）；
- M5–M7 取自 xu-1 `a1-norm2/` 各 paper 目录中时间戳最新的 ICML 匿名版 `main.pdf`；M8 取自 `a2-irregular/paper/icml2026/tapr.pdf`。仅做只读拷贝，未修改远端任何文件。

---

## M1 — LiveTS: A Leakage-Proof, Continuously-Refreshed Benchmark for Zero-Shot Time Series Forecasting

### Summary
提出一个"持续刷新、防泄漏"的零样本 TSFM 基准：6 个公开实时域、205 条日频序列、3 个滚动 cutoff（2025-01/2025-07/2026-01）、50,284 个预测窗口，以模型发布日期（HF 仓库创建日）做 as-of 门控，评估 6 个开源 TSFM + 5 个监督/统计基线，报告 MASE/CRPS/WQL 几何平均、bootstrap CI、Holm 校正的 DM 检验（§3.2–3.3, Table 1–2）。

### Strengths
1. 评测纪律在池内属上乘：future-only 窗口、发布日期门控、几何平均聚合、DM-HLN+Holm、1,000 次 bootstrap（§3.3），统计报告完整度超过多数基准论文。
2. 结论有信息量：TSFM 在季节性域显著优于 seasonal naive，但 crypto/FX 域多数模型打不过 naive（Table 2）——这是对 TSFM 泛化叙事的有效泼冷水。
3. 局限性一节（§6）诚实：承认日频单变量、公共域偏置、发布日期不确定性。

### Weaknesses
1. **规模与代表性不足以支撑 "benchmark" 声明**：205 条序列且极不均衡（weather 80 vs energy 6，§3.1）；仅日频、仅单变量、horizon 固定 14。与池内 TFB（25 个域、多频率、8,068 条序列）和 ProbTS 相比，覆盖面差一个数量级，"comprehensive" 类结论无法外推。
2. **发布日期 = HF 仓库创建日是一个粗糙代理**（§3.2）：模型可能在 HF 上传前已用近期数据训练（arXiv 版本、内部 checkpoint），门控只防"发布后数据"而非"训练截止后数据"；这正是 M2 类污染审计想解决的问题，本文未讨论该缺口。
3. **主表不可比**：TimesFM-2.5 因发布晚仅 820 个窗口，其 geo-MASE 0.789 与其他模型的 ~50k 窗口不在同一样本上（Table 1），却被排在榜首叙述；应报告共同窗口子集上的配对比较。
4. **监督基线可信度低**：iTransformer geo-MASE 4.220、DLinear 1.597（Table 1）远差于 seasonal naive，明显是"单一通用配置、零调参"（§6 自认）导致的稻草人；由此得出的"TSFM 优于监督基线"证据无效。
5. 增量性：思路是 LLM 界 LiveBench 到 TS 的直接移植；池内 "This Time is Different"（observability 视角评 TSFM）已提供了 leakage-aware 的 TSFM 评测，本文的净新增是滚动 cutoff 机制本身。

### Questions
1. 各模型在"共同干净窗口交集"上的配对 DM 结果如何？TimesFM-2.5 的优势是否只是窗口子集效应？
2. 若用 arXiv 首版日期或训练数据截止日代替 HF 创建日，有多少窗口的"干净"标签会翻转？
3. 监督基线做常规调参（lookback/lr 网格）后结论是否仍成立？

### Scores
novelty 4 | significance 6 | soundness 6 | clarity 7 | **overall 5 — borderline（偏 weak reject）**

### 池内定位（benchmark/eval 子领域）
约 **35–45 百分位**。低于 TFB（覆盖面、公平调参协议全面占优）与 ProbTS（概率评测深度）；与 "This Time is Different" 相比动机重叠而工程完成度更低；高于池内纯 leaderboard 型基准，因其统计协议更严谨。

---

## M2 — Auditing Pretraining-Data Contamination in Time-Series Foundation Models（基于补齐后的完整 PDF 重审）

### Summary
提出 TSFM 预训练数据污染的严格黑盒审计协议：四级污染分类（L1 exact / L2 transformed / L3 same-source / L4 global-event）+ 覆盖五大语料的机器可读 membership 数据库（含 Time-300B ⊃ Chronos TSMixup 的传递链，§3）；三种探针（P1 扰动敏感性、P2 代理续写边际、P3 跨模型技能差）配数据集级置换检验 + Holm + Cliff's δ（§4）；~3M 参数 TinyTSFM 受控注入测试床给出探针的 ground-truth ROC 与能力边界（L1: P1/skill AUC 0.82；L3: 全探针 AUC≈0.51，Fig. 1，§5）；8 开源 TSFM × 14 数据集实证审计（TimesFM-2.0 最强信号：skill 未校正 p=0.010、δ=0.85；无一过 Holm，min Holm p=0.24，§6）。

### Strengths
1. 问题重要且池内空缺：池内污染检测均针对 LLM（WikiMIA、Min-K%++、Infilling Score、ConStat），TSFM 侧尚无系统黑盒审计；与并行 TSFMAudit 的差异表（Table 1：黑盒 vs 灰盒、ground-truth ROC vs 纯文档证据）划界清楚。
2. **方法论贡献是实的且经 ground truth 验证**：受控注入测试床给出黑盒污染探针的首个 ROC 验证及明确能力边界（L1 可检 / L3 盲区，Fig. 1）——即使实地审计为阴性，这一边界本身是可复用的科学结论。
3. 统计纪律在八份手稿中最自觉：演示窗口级检验的反保守性（p≈1e-61 vs 数据集级 p≈0.36，五个数量级虚假自由度，§4）与小对照集的效应量膨胀（Time-MoE P2 δ 从 0.44 缩到 0.17，Fig. 3）——两者对整个 TSFM 评测文献是有公共价值的校准。
4. 自建 post-cutoff 保证非成员对照（weather/aqi_post2025）并主动标注其 L3/L4 边界风险（§3）；可复现性清单完整（附录 A）。

### Weaknesses
1. **确证性结论为零**：实地审计无一检验过 Holm（min p=0.24，§6）；作者辩称这是 n=14 数据集下诚实协议的预期结果，可接受，但这意味着论文对"哪些 TSFM 被污染"只能提供方向性证据（TimesFM-2.0 δ=0.85 未校正 p=0.010），"机制解读"自认是 hypothesis 而非因果结论（§6）。功效分析仍缺席：需要多少数据集才能在 Holm 下确证 δ=0.8 级效应，论文没有回答——而这正是其"建议"部分的可操作性所系。
2. **对最重要的污染类型无检测力**：L3 same-source AUC≈0.51（§5）；实践中语料-基准重叠恰以 L3/L2 为主（L1 逐序列包含相对易查文档），协议对高发场景近乎失明，且 L2 的 ROC 未在测试床中系统给出（§8 仅说 "degrade"）。
3. **testbed 外推性弱**：3M 参数、4096 窗口重复曝光 30k 步的注入强度远超真实 TSFM 训练中单序列的曝光量（§5 自认"validates probe validity, not real-model memorization strength"）——L1 AUC 0.82 是探针有效性的上界演示，不能反推真实模型上的 TPR。
4. 体量与完成度：正文 4 页 + 参考文献/附录共 6 页、约 2,000 词，无独立讨论节；membership 数据库依赖论文表格 + 目录枚举的文档性证据（§3），其准确性未做抽样核验。仍自标 working draft。

### Questions
1. 数据集级置换检验在 δ=0.85、Holm 家族规模不变时的功效曲线？n 要到多少才能确证 TimesFM 级信号？
2. L2（TSMixup 变换）的测试床 ROC 是多少？这是 Chronos 系语料的主要污染形态。
3. 注入曝光强度降到真实训练量级（单 epoch、无重复池）时，L1 AUC 还剩多少？

### Scores
novelty 6 | significance 6 | soundness 6 | clarity 6 | **overall 4.5 — weak reject（诚实但结论力不足；补功效分析 + L2 ROC + 扩数据集后可到 borderline/weak accept）**

### 池内定位（contamination/memorization 子领域）
约 **30–40 百分位**。低于 Min-K%++/Infilling Score（有确证检测力的完整方法）与 ConStat（可直接落地的基准污染统计）；高于纯文档式泄漏 catalogue（其引用的 Meyer 2025 类工作）——ground-truth ROC 验证与统计校准演示是净增量；与 "Proving Test Set Contamination" 相比，后者有可证明保证而本文只有经验边界。TSFM 场景先发优势明显，但需正结果支撑。

---

## M3 — How Calibrated Are Zero-Shot Time Series Foundation Models? An Online Conformal Audit and Calibration Toolkit

### Summary
审计 3 个 TSFM 家族 × 10 数据集 × 2 horizon 的原生分位数校准：原生覆盖率严重不足（Chronos-T5 名义 0.80 下实测 0.30–0.62；泄漏无关子集更差，H=96 低至 0.301，Table 2），再以冷启动在线 conformal 协议比较 split CP / NexCP / ACI / conformal PID / SPCI 及 max-score 联合带；给出 NexCP 覆盖间隙的一个定理（Thm 1）与联合带扩展（Prop 1）。

### Strengths
1. 审计发现有实质价值且反直觉：泄漏无关子集上原生校准**更差**（T5 H=24: 0.401，Table 2）——把"TSFM 分位数可直接用"的常见操作证伪，并把泄漏与校准两个议题连接起来。
2. 实验协议干净：滚动起点、非重叠窗口、零目标域训练、按 model×horizon 做 Holm 校正的配对检验（§3, Table 3），分 regime-shift 强度的失败模式分析（Table 4: Moirai 0.840→0.657）。
3. 方法比较公平地报告了负结果：SPCI 显著更差（Table 1/3），联合带宽度翻倍的代价如实给出。

### Weaknesses
1. **方法学新颖性薄**：所有 CP 方法均为现成（NexCP、ACI、PID、SPCI）；Thm 1 是有界漂移假设下 NexCP 标准结果的直接改写，Prop 1 是 union-bound 级扩展。贡献本质是"审计 + 工具包"，在 ICML 主赛道对 novelty 的要求下偏弱。
2. **与池内最新在线 CP 方法未比**：池内 2025 线（Error-quantified CI、KOWCPI、decaying step-size OCP）均缺席；排除 HopCPT/CopulaCPTS 的理由（需训练/校准集）成立，但 decaying-step ACI 与本文协议完全兼容，缺席削弱"toolkit 完整性"声明。
3. 每数据集仅 50 窗口（§3），Table 4 分层后子样本更小，高-shift 层的覆盖估计置信区间未报告。
4. 仅小规格模型变体（§6 自认），"TSFM 校准差"的结论对大模型是否成立未知。

### Questions
1. decaying step-size ACI（池内 2024）加入后结论是否改变？
2. 50 窗口下 Table 4 分层覆盖率的 CI 是多少？高/低 shift 差异是否显著？
3. 原生欠覆盖多大程度是分位数头训练目标（如 Chronos 的 token 化）造成的？有无按模型族的机制分析？

### Scores
novelty 4 | significance 6 | soundness 7 | clarity 7 | **overall 6 — weak accept（更适合 benchmark/dataset track）**

### 池内定位（conformal/UQ 子领域）
约 **40–50 百分位**。低于 Error-quantified Conformal Inference 与 ACI-by-betting（方法学原创）；作为审计类工作高于池内单纯应用型 CP 论文；与 KOWCPI 相比方法弱、实证面宽。

---

## M4 — Placebo Text Is All You Need? A Rigorous Audit of Textual Gains in Multimodal Time Series Forecasting

### Summary
对 Time-MMD/MM-TSFlib 生态的文本增益做安慰剂对照审计：8 域 × 3 backbone × 5 文本条件 × 5 seeds（1,200 runs）。发现真实文本在 0/48 个设置中显著优于 shuffle 文本（Holm 校正，192 次比较）；多模态分支确有收益（40/48，中位 3.2%）但 84% 归因于文本分支夹带的数值先验历史通道（§3.1 的分解，§3.2 公式）；Climate 文本 100% 违反 as-of 纪律（1,982/1,984 条事实晚于目标期，如 1983 年目标配 2021 年 NOAA 文本）。

### Strengths
1. **设计是八份中最锐利的**：placebo 条件组（real/shuffle/static/zero/ts-only/mismatch）+ 可加性分解把"文本增益"拆成可检验的机制项（§3.2），这是池内多模态论文（Time-MMD、TimeCAP、ChatTime、Time-VLM）普遍缺失的对照。
2. 发现具有领域级冲击力：若成立，Time-MMD 系多模态收益主要是数值通道混杂 + 时间泄漏——直接动摇池内一个活跃子领域的证据基础；Economy H=12 上"删掉文本反而降 MSE 27.9–30.7%"（§5）是强证据。
3. as-of 审计（Climate 100% 违规）是可独立复核的硬事实，统计处理（配对检验 + Holm 192 次）规范。

### Weaknesses
1. **范围严重受限而标题过度声明**：审计只覆盖 MM-TSFlib 官方融合路径 + 冻结 GPT-2 嵌入 + prompt weight 0.1（§3.3）；TaTS/TimeCAP/CiK 等更强融合机制的审计明确"pending"（§7 的 TODO 列表）。"Placebo Text Is All You Need?" 的普适性暗示未被证据覆盖——更强的 LLM 编码器或 instruction-tuned 融合完全可能利用真实文本。
2. 手稿自认未完成项过多（§7：LLM spoiler 探针"awaits API budget"、缺失值处理未完全 as-of）——以 7 页 2,814 词的体量，更像一篇高质量 workshop/短文而非完整主会论文。
3. 弱 backbone（DLinear/iTransformer/PatchTST + 冻结 GPT-2）可能本身无法利用文本；"文本无用"与"模型用不了文本"未区分（mismatch 条件 0.03% 变化同时兼容两种解释）。

### Questions
1. 换用可训练文本编码器或更大 LLM 后，real vs shuffle 是否仍 0/48？
2. Time-MMD 之外（如 CiK 的事件驱动数据）as-of 违规率如何？
3. 数值先验通道移除后，剩余 16% 增益的来源是什么？

### Scores
novelty 7 | significance 7 | soundness 7 | clarity 7 | **overall 6.5 — weak accept（若补 1–2 个融合机制的审计可到 accept）**

### 池内定位（multimodal/text 子领域）
约 **60–70 百分位**。作为审计/批判性工作高于 ChatTime、S2IP-LLM 等增量方法论文（其证据基础正是本文质疑对象）；低于 Time-MMD（奠基数据集）与 Context is Key（更完整的评测框架）；与 "Time-LLM 类是否真用到语言先验"的池内质疑线（如 LLM-ablation 类论文）同档但对照设计更严格。

---

## M5 — Normalization Under Structural Breaks: Diagnosing and Extrapolating Non-Stationary Statistics for LTSF

### Summary
诊断 + 方法论文：SynShift 合成套件按 break 位置分层归因（RevIN 在 break 窗口 16–23× 误差，§3.3）；证明 lookback 内 break 可修（Prop 1 的偏差-方差切换）而 horizon 内 break 对确定性归一化不可辨识（Prop 2）；提出 ShiftExtrap-D（参数无关 CUSUM 检测门控 + GRU 统计外推头 + RevIN 锚定初始化）及概率化 v2/v2.1。标准 6 数据集 ×3 backbone×2H×5 seeds：v2.1 27/36 胜 RevIN，平均 −2.51%；break 密集的国债收益率 6/6 胜、平均 −10.0%（Table 2）；系统性报告 ETTm2-H336 失败案例与 4 种可靠性估计的负结果（§7）。

### Strengths
1. 诊断框架有真实增量：两层归因（lookback vs horizon break）+ Prop 1/2 把"归一化能修什么"讲清楚了，池内 SIN/FAN/DDN 均无此失败模式分解；"所有方法在 horizon 层不可区分（≈0.33 MSE）"是有洞察的实证 + 理论呼应。
2. 实验规模与透明度突出：5,300+ runs、JSONL 日志、跨 torch 版本审计（偏差 0.89%）、单环境复跑验证（−1.95% vs −1.98%）；负结果（v2.2、v3/v3b held-out 门控）如实报告并给出机制解释（§7）。
3. 安全性设计（RevIN 锚定、检测调制门控）解决了这类方法"稳定数据上倒退"的通病。

### Weaknesses
1. **标准基准上的效应量太小**：−2.51% 平均、且主要由 Exchange（−19.9%）单点拉动（Fig. 2）；Exchange 长期被认为接近随机游走，任何靠近 last-value 行为的方法都会在其上大胜——需要排除"方法只是学会了在 Exchange 上做 naive drift"的解释。ETTm2 H336 +27% 的回退表明方法在真实数据上有实质风险。
2. **基线不全**：正文 Table 1 缺 DDN、SIN、TimeBridge（池内同子领域代表）；WDAN 自认对齐失败（§5）。与 2024–2025 SOTA 归一化的头对头证据不足。
3. §7 自曝检测门控在真实数据上 93–100% 常开（ETTm2 99.4%）——即"检测"组件在真实数据上不起检测作用，方法实际是 always-on 统计外推 + 学习门控；这与标题的 "structural breaks / detection" 叙事有落差，卖点与机制不一致。
4. fred_rates 的 −10.0% 是方法的最强证据，但仅 1 个 break 密集数据集（+1 个平稳对照），"break-dense regime 是目标场景"的普适性单点支撑。

### Questions
1. 在 Exchange 上与 naive drift/last-value 基线比较，v2.1 的增益还剩多少？
2. DDN、SIN 加入 Table 1 后排名如何？
3. 若检测门控在真实数据上常开，移除检测组件（纯 v1.1+v2）损失多少？检测的净贡献是否只在合成数据？

### Scores
novelty 6 | significance 6 | soundness 7 | clarity 6 | **overall 6 — weak accept / borderline**

### 池内定位（normalization/nonstationarity 子领域）
约 **50–60 百分位**。诊断严谨性高于 FAN/DDN（这两篇无失败模式分析），但方法净效应弱于它们各自报告的增益；低于 SIN（ICML，理论选择准则更优雅）；高于池内纯增量归一化变体（WDAN/MPN 档）。

---

## M6 — When Can Normalization Reliability Be Estimated? A Temporal-Boundary Hierarchy for TTA in Long-Horizon Forecasting

### Summary
把"归一化可靠性从哪些数据可估计"组织成 L0（train priors）–L1（held-out）–L2（在线前缀标签）–L3（在线成熟标签）层级；11 数据集 ×3 backbone×5 seeds（990 最终 runs）映射各层失败模式：L0/L1 无法检测 train→test 可预测性漂移，L2 对远视界伤害盲（timescale dichotomy），L3 首次修复 ETTm2-H336（−4 至 −11.5%）但在 drift 主导序列上留 +1–6% 残余伤害；用非正式的 label-delay regret floor（Thm 1, Ω(min(a,b)·H)/switch）解释残余；给出选择规则。

### Strengths
1. 概念组织有价值：L0–L3 信息集层级 + prefix/full-horizon 二分是清晰的思维框架，池内 TTA 论文（Proceed、SOLID、TAFAS）无人显式刻画"可靠性可估计性"这一维度。
2. 负结果学科化：四个被淘汰的估计器家族各配完整网格（§3），比典型"只报成功"的 TTA 论文诚实。
3. Fig. 1 的门控轨迹测量（sign segment 0.29–0.64×H）把理论与残余伤害的位置对上了。

### Weaknesses
1. **交付物是地图不是方法**：headline 数字为 L2-fast 相对 L0 仅 −0.12%（"parity"），L3-soft +8.38%、L3-regret +5.35%（Table 1 注）——即论文提出的新配置**平均是有害的**，只在窄 regime（ETTm2 H336、FRED）有效；"combined system dominates L0"是**按数据集族事后选配置的 policy 级声明**（§5 自认 "no single configuration achieves this"），选择规则本身未做前瞻性验证，有 per-dataset 选择的过拟合嫌疑。
2. **Thm 1 自认 informal**（§7），且与 M7 的 Thm 1 基本同一结果——两稿间理论贡献重复，本稿的独立理论增量近零。
3. 强依赖前作：L0 即 M5 的 v2.1，网格、splits、参照数均"from prior verified grids"；作为独立投稿的自足性弱，三部曲切香肠痕迹明显（M5 结尾"TTA 是 principled fix"→ 本稿；本稿 floor → M7 定理化）。
4. 与池内在线 TSF 方法主要以"协议不同"划界（OneNet/FSNet 列 Tier-2，§5.1），回避了直接性能竞争；TAFAS 仅重实现其 core，未跑官方全法。

### Questions
1. 选择规则若在 held-out 数据集族上前瞻性验证（规则定于一半数据集、测于另一半），还成立吗？
2. L3 修复 ETTm2 H336 后（0.2538–0.2687）仍差于 RevIN（0.2264–0.2411，Table 1）——为何不把"直接回退 RevIN"作为 L 层级之外的对照策略？
3. Thm 1 与姊妹稿的定理关系如何？若同会投稿，理论归属如何划分？

### Scores
novelty 5 | significance 5 | soundness 6 | clarity 6 | **overall 4.5 — weak reject**

### 池内定位（TTA/online 子领域）
约 **25–35 百分位**。低于 TAFAS（有普适正增益的方法）与 Proceed/SOLID（完整方法 + 广泛验证）；概念框架优于池内增量 TTA 变体，但"平均有害、靠事后选择规则兜底"的主结果在方法子领域难以过线。

---

## M7 — Gating Under Delay: Minimax Theory and a Parameter-Free Lazy-FTL Gate for TTA in Long-Horizon Forecasting

### Summary
把 TTA 开/关门控形式化为延迟反馈在线学习：证明 minimax 下界 S[½min(a,b)H + ⅛min(a,b)⌊σ²/2∆²⌋]（Le Cam + Pinsker）、匹配上界（延迟窗口检验），得 Θ̃(S(H+σ²/∆²))；给出 gate-vs-always-on 二分定理（Thm 3：负 regime 驻留 ≲H 时无策略胜 always-on）；提出参数无关 lazy-FTL 门（切换成本 λt=H·|Â|t）。66 设置 ×5 seeds（1,980 runs）：lazy-FTL −3.45% vs base、−6.46% vs always-on，与用更强前缀反馈的 L2-fast 统计不可区分；另报告两个测量陷阱（单位不匹配、事后参数 hindsight bias）。

### Strengths
1. **三部曲中理论最实**：下界/上界/二分构成完整 minimax 刻画，且"延迟与标签视界绑定 + 与固定平凡策略比较的二分"在延迟在线学习文献中有可辩护的新意（§7 的定位诚实）；数值仿真定量匹配 Thm 3（×18 / ×2，§6）。
2. 算法与理论咬合：λt=H·|Â|t 直接由下界的切换成本导出，参数无关、O(C) 状态、默认 off；Fig. 1 的 St/±λt 轨迹展示机制。
3. 两个测量陷阱（normalized-signal 使 −3.5% 变 +2.1%；hindsight 参数同样翻转结论）是对整个 delayed-gating 子领域有公共价值的方法论贡献，且以完整网格量化。
4. 诚实报告 h* 变体为 null result（§6）。

### Weaknesses
1. **实证增益的天花板低**：lazy-FTL 与 L2-fast "统计不可区分"（−0.5%，t=−0.7）——即新方法只是在更弱反馈下追平已有策略；相对 base 的 −3.45% 中大头来自 FRED（−27 至 −51%）单数据集族，其余多为 ≤2% 的守成。方法的实用叙事是"不更差 + 免调参"，作为 ICML 方法论文偏保守。
2. 理论模型高度风格化：两水平 {+a,−b} 分段常数 + 高斯 i.i.d. 噪声（§8 自认）；真实优势过程是连续、异方差、序列相关的，下界对真实场景的约束力打折。
3. 同 M6：base（v2.1）与固定策略参照全部来自前作网格（"Anonymous 2026a/b"，§6）——评审无法独立核对参照数字；且 base 本身在 ETTm2/合成数据上劣于 RevIN，门控是在修自家方法挖的坑（ETTm2 H336：lazy-FTL 0.2586 仍差于 RevIN 0.2264）。
4. Oracle gap（−13.1% vs −3.45%）声称"全是 floor 而非 headroom"，但未按数据集分解证明该断言——drift 序列外（如 FRED H336 iT：lazy-FTL 0.4166 vs L3-soft 0.3813）明显存在非 floor 的可及空间。

### Questions
1. 在非自家 base（如 RevIN 或 TAFAS 官方实现）上套 lazy-FTL，结论是否保持？
2. Ys 序列相关时下界如何变化？εs i.i.d. 假设在真实优势轨迹上被违反到什么程度？
3. FRED iT H336 上 lazy-FTL 明显劣于 L3-soft（0.4166 vs 0.3813）——这是 floor 还是门控迟滞过大？

### Scores
novelty 7 | significance 6 | soundness 7 | clarity 7 | **overall 6 — weak accept**

### 池内定位（TTA/online 子领域）
约 **55–65 百分位**。理论完整性高于 TAFAS/SOLID/Proceed（均无 minimax 刻画）；实证影响力低于 TAFAS（后者有普适增益）；与池内 DSOF/Ada-ReAlign 档相比，理论强、实证平。是三部曲（M5/M6/M7）中唯一能独立站住的一篇。

---

## M8 — TAPR: Time-Adaptive Patch Routing for Forecasting Irregularly Sampled Multivariate Time Series

### Summary
提出 ODE-free、graph-free 的 IMTS 预测 transformer：等观测数自适应 patching、可学习 Fourier 连续时间编码 + patch 元数据、时间距离/通道对加性注意力偏置、任意时间戳 cross-attention 解码。官方 t-PatchGNN 基准（PhysioNet/USHCN/Activity，5 seeds，配对 t 检验）：USHCN（t=−9.6）与 Activity（t=−2.9）显著优于 t-PatchGNN 且参数量仅 28–39%；PhysioNet 落后 4.5%；USHCN 上优于引用的 Hi-Patch 数字。

### Strengths
1. 主张与证据匹配、账目诚实：明确"accuracy–simplicity–efficiency trade-off，非全面占优"（§5）；PhysioNet 劣势、金字塔探针失败、regular-grid 上仅"competitiveness"（0.442 vs PatchTST 0.397，§4.5）均如实报告；Hi-Patch 数字标注为引用而非复现，并用双方 t-PatchGNN 基线一致性佐证可比性（脚注 1）。
2. "等观测数 patching"是简单而有依据的设计：ablation 显示 fixed-K 在 USHCN 退化（0.4817 vs 0.4786），且 IMTS 上实例归一化有害（PhysioNet 6.45 vs 5.47e-3）是有用的负结果。
3. 官方 pipeline/evaluator 不改动 + 统一协议自跑基线，可复现性表述完整（§6, 附录 A）。

### Weaknesses
1. **基线覆盖不足**：只自跑了 t-PatchGNN/GRU-D/mTAN 三个基线；GraFITi（AAAI'24，池内，同基准线的强方法）完全缺席，ContiFormer 仅转引数字（§5 自认）；在 2025 年 Hi-Patch/HyperIMTS 已发表的背景下，与"最接近竞品"的直接比较是靠引用数字完成的——对一篇方法论文这是硬伤。
2. **净胜面窄**：3 个数据集中 1 个落后（PhysioNet，恰是通道最多、临床最重要的），1 个与 Hi-Patch 引用值持平偏落后（Activity 2.63 vs 2.57）；真正干净的胜利只有 USHCN（5 通道）。MIMIC 缺席（凭证问题，§5）进一步压缩临床说服力。
3. 新颖性属重组式：等数量 patching、Fourier 时间编码、时间衰减注意力偏置、cross-attention 查询解码各自均有先例（mTAN/SeFT/t-PatchGNN 线），组合的净新意主要在"证明不需要图"。
4. 效率叙事不一致：卖点是轻量，但 PhysioNet 上峰值内存反超 t-PatchGNN（4.3 vs 3.5GB）且训练更慢（§4.4）。

### Questions
1. GraFITi 在同协议下的数字？它在该基准上通常强于 t-PatchGNN，缺席使"简单模型已够"的论断不完整。
2. Hi-Patch 官方代码在你们的协议下复跑结果？
3. 41 通道时 channel-pair 表 O(D²·h) 的内存增长如何随通道数外推到 MIMIC 规模？

### Scores
novelty 5 | significance 5 | soundness 6 | clarity 7 | **overall 5.5 — borderline**

### 池内定位（irregular/IMTS 子领域）
约 **35–45 百分位**。低于 t-PatchGNN（奠基基准 + 全面胜出时的完整基线阵容）、Hi-Patch/HyperIMTS（更强数字 + 已录取）；高于池内增量 ODE 变体；与 GraFITi 相比证据面窄。"简单化"论点有价值但需补齐 GraFITi/Hi-Patch 直接比较才能到 borderline accept。

---

# 汇总排序

| 排名 | 稿件 | 标题关键词 | novelty | signif. | sound. | clarity | overall | 决策 | 池内百分位（子领域） |
|---|---|---|---|---|---|---|---|---|---|
| 1 | **M4** | 多模态文本安慰剂审计 | 7 | 7 | 7 | 7 | **6.5** | weak accept | 60–70（multimodal） |
| 2 | **M7** | 延迟门控 minimax + Lazy-FTL | 7 | 6 | 7 | 7 | **6** | weak accept | 55–65（TTA） |
| 3 | **M3** | TSFM 校准审计 + conformal 工具包 | 4 | 6 | 7 | 7 | **6** | weak accept | 40–50（conformal） |
| 4 | **M5** | 结构断裂下的归一化诊断/外推 | 6 | 6 | 7 | 6 | **6** | weak accept / borderline | 50–60（normalization） |
| 5 | **M8** | TAPR：无图无 ODE 的 IMTS transformer | 5 | 5 | 6 | 7 | **5.5** | borderline | 35–45（IMTS） |
| 6 | **M1** | LiveTS 防泄漏滚动基准 | 4 | 6 | 6 | 7 | **5** | borderline（偏 reject） | 35–45（benchmark） |
| 7 | **M2** | TSFM 污染审计（完整 PDF 重审后上调） | 6 | 6 | 6 | 6 | **4.5** | weak reject | 30–40（contamination） |
| 8 | **M6** | 可靠性估计层级 L0–L3 | 5 | 5 | 6 | 6 | **4.5** | weak reject | 25–35（TTA） |

## 横向评语（Reviewer 2 总结）

1. **审计类（M4/M3/M1/M2）整体强于方法类的通病是新颖性**：M4 靠对照设计的锐利度补足；M3/M1 的协议严谨但方法零增量；M2 问题最重要、统计纪律最好，但确证结论为零且对高发的 L3 污染无检测力，短稿体量也不足。（M2 与 M6 同为 4.5，排序上 M2 居前：其 ground-truth 验证的能力边界是独立可复用的贡献，而 M6 的主要配置平均有害、依赖事后选择规则。）
2. **M5/M6/M7 是明显的三部曲切香肠**：共享网格、数据、参照数字与理论（M6 与 M7 的 Thm 1 实质重复），且 M6/M7 都在修 M5 自家方法制造的 ETTm2 回退（修复后仍不及 RevIN 基线）。若同会投稿，程序上应向 AC 披露。独立可站住的只有 M7。
3. 按 ~25% 录取率的现实操作：**我会给 M4、M7 正分（weak accept），M3/M5 borderline 上下浮动取决于同批竞争，其余拒。**

*材料说明：M2 的 `paper/main.pdf` 初版评审时缺失（被 .gitignore 误排除），作者方以 commit 9d610e2 补齐后已基于完整 PDF 重审；本版 M2 评分（3.5→4.5）反映完整稿件中经 ground-truth 验证的能力边界与统计校准贡献，原"材料缺失"扣分已移除。*
