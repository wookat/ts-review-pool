# 8 份匿名时序手稿独立评审报告（ICML/NeurIPS 资深审稿人标准）

**评审设定**：对照池为 wookat/ts-review-pool 的 101 篇 2024–2026 顶会论文（7 个子领域）。所有评分按真实顶会录取标准（平均录取率 ~25%）给出；分数含义：overall ≥7 = 明确录取区，6 = 边缘偏收，5 = 边缘偏拒，≤4 = 拒稿区。所有判断均引用手稿内证据（章节/表格/数字）。

**修订说明（v2）**：M2 的最终编译版 PDF（paper/main.pdf, commit 9d610e2）后补推送；本版基于完整 PDF（6 页，含 Fig. 1–3 与可复现性附录）重评 M2 并更新总排序，其余 7 份评审不变。

**手稿对应**：
| ID | 标题（简） | 来源 | 子领域（池） |
|---|---|---|---|
| M1 | LiveTS: Leakage-Proof Continuously-Refreshed Benchmark | livets-bench/paper/main.pdf | 4 基准与评测 |
| M2 | Auditing Pretraining-Data Contamination in TSFMs | tsfm-contamination-audit/paper/main.pdf（commit 9d610e2 补推） | 5 数据污染 |
| M3 | How Calibrated Are Zero-shot TSFMs? Online Conformal Audit | conformal-tsfm/paper/main.pdf | 6 共形预测 |
| M4 | Placebo Text Is All You Need? Audit of Textual Gains in MM-TSF | mmtsf-ablation/paper/latex/main.pdf | 7 多模态 |
| M5 | Normalization Under Structural Breaks (ShiftExtrap-D) | a1-norm2/paper | 1 非平稳/归一化 |
| M6 | When Can Normalization Reliability Be Estimated? (L0–L3 hierarchy) | a1-norm2/paper2 | 1/2 交叉 |
| M7 | Gating Under Delay: Minimax Theory + Lazy-FTL Gate | a1-norm2/paper3 | 2 在线/TTA |
| M8 | TAPR: Time-Adaptive Patch Routing for IMTS | a2-irregular/paper (tapr.pdf) | 3 不规则采样 |

---

## M1 — LiveTS: A Leakage-Proof, Continuously-Refreshed Benchmark for Zero-Shot TSF

### Summary
提出"future-only / as-of"评测：只对模型权重公开日期之后产生的数据打分，使预训练污染与社区过拟合"结构性不可能"。含 PIT 采集语义、快照哈希、预注册协议（三 cutoff、MASE/CRPS/WQL、几何均值 + bootstrap CI、DM-HLN + Holm），并给出 205 条日频序列 × 3 cutoff × 50,284 窗口上 6 个开源 TSFM 与统计/监督基线的"干净分数"（Table 1–2），以及月度刷新 live leaderboard 设计（§5）。

### Strengths
1. **问题真实且时机好**。TSFM 零样本评测的污染问题已被池内多篇论文（Meyer et al. 引用于 §1；GIFT-Eval/fev-bench 只能做去重）指出但没有结构性解法；LiveTS 是 LiveBench（ICLR'25 Spotlight，池内 4 号子领域）范式向时序的首个系统移植，§2 对此定位诚实。
2. **统计方法学在时序基准类论文中属上乘**：预注册协议（§3.3，冻结日期 2026-08-09 + 修订政策）、geo-mean + 跨序列 bootstrap、DM-HLN + Holm、点预测模型不强行插补分位数——这些细节超过 TFB/ProbTS 的常规做法。
3. **结论有信息量且反流行叙事**：Table 2 显示 crypto/FX 上无一 TSFM（三 cutoff）打败 seasonal naive；Fig. 3 显示中段排名跨 cutoff 重排——直接证明静态单一划分会误排名。§4.4 与配套污染审计的交叉印证是加分项。
4. **release-date gating 的可见化**（Table 1 中 TimesFM-2.5 只有 1 个干净 cutoff、单独标注 †）体现了协议自觉。

### Weaknesses
1. **规模与覆盖有限**：205 条纯日频、单变量序列（§3.1），六域中 traffic 仅 7 条、energy 仅 6 条——按域分解的结论（Table 2）在这两域上统计上很脆弱。相比 GIFT-Eval（24 数据集、多频率）与 fev-bench（100 任务、含协变量），corpus 广度差一个量级；论文自己承认这是 trade-off（§2），但审稿人需指出：**结构保证 + 目前的覆盖面，尚不足以取代静态基准，只能作补充**。
2. **主要证据是 historical simulation 而非真 live 轮次**（§4 vs §5）：历史模拟的"exact"前提是权重在 release date 后冻结（§3.2），对多次悄然更新 checkpoint 的模型（HF repo 创建日期 ≠ 当前权重日期）该假设可被违反，文中用"保守取更早日期"缓解但方向恰好相反——更早的 release date 会让**更多**窗口被算作干净，这与 §3.2"保守=更少干净窗口"的表述矛盾（release 越早、cutoff>release 的窗口越多）。这是协议叙述上的一个实质性瑕疵。
3. **监督基线弱设置**：iTransformer 以 univariate 模式运行、MASE 4.22（Table 1）明显是 misconfiguration 级别的数字，虽然 §4.2 声明"报告为下界不事后调参"，但把它放进主表与 TSFM 并排，会让"TSFM 优于监督模型"的表观结论被质疑。DLinear 1.60 同样偏离文献常值。
4. **贡献 4（live benchmark）目前是承诺而非结果**：Round-0 尚未跑出任何 live 数字；月度刷新能否长期维持（free-tier 源限流，§6 自认）无法在审稿时验证。
5. 8 页正文较薄，附录 A–D 均指向仓库文件而非随稿提交，双盲评审下无法核验预注册文本的真实冻结时间。

### Questions
1. release date 取更早时间戳如何与"保守 = 更少干净窗口"一致？请给出形式化定义。
2. Chronos-Bolt 系列 Series=207 与其他 205 的差异来源？
3. 若模型方在 HF 上覆盖更新权重（同 repo），协议如何检测？
4. crypto/FX 的 MASE 用 period=1 的 naive 做分母，等价于随机游走基线；建议报告 RMSSE 或对数收益率上的 sMAPE 以避免 near-martingale 域的尺度病态。

### 打分
- novelty: **6**（范式移植 + 工程组合，非方法原创）
- significance: **7**（若社区采用，影响大；D&B track 定位恰当）
- soundness: **6**（统计学到位，但 release-date 逻辑瑕疵 + 弱监督基线 + live 部分未验证）
- clarity: **7**
- **overall: 6 — weak accept**（更适合 NeurIPS D&B track；ICML 主会作为 benchmark 论文偏薄）

### 池内定位
子领域 4（基准与评测，11 篇）。估计位于该子领域**前 35–45%**。
- 低于 **LiveBench（ICLR'25 Spotlight）**：同一范式的原创提出者，且以 LLM 领域的规模和影响力执行；LiveTS 是其时序转译。
- 低于 **TFB（VLDB'24）/ ProbTS（NeurIPS'24 D&B）**：覆盖面与方法宽度（TFB 覆盖多领域全方法族）明显更大。
- 高于/接近 **Zero-shot forecasting of chaotic systems（ICLR'25）**：LiveTS 的协议工程与统计严谨性更强，但后者有 135 个系统的独特科学角度。与 **fev-bench（池外参考，未收录）** 相比：设计理念更严格（结构性防污染），覆盖不及。

---

## M2 — Auditing Pretraining-Data Contamination in Time-Series Foundation Models

### Summary
黑盒（仅预测 API）TSFM 污染审计：L1–L4 污染分类法 + 五语料成员数据库（含 Time-300B ⊃ Chronos TSMixup 的传递性污染链）；P1 扰动敏感度 / P2 代理延拓 / P3 跨模型技能差三类探针，数据集级置换检验 + Holm + Cliff's δ；核心贡献是**受控注入试验台**（3M 参数 TinyTSFM 从头预训练、已知注入污染），给出探针的 ground-truth ROC：L1 可检（AUC 0.82），L3 基本不可检（AUC ≈0.51）。实测 8 TSFM × 14 数据集：方向一致（TimesFM-2.0 skill δ=0.85, 未校正 p=0.010）但无一通过 Holm 校正。

### Strengths
1. **注入试验台是该子领域的真空白**。池内 17 篇污染/成员推断论文（Min-K%、ConStat、LLM Dataset Inference 等）全部面向 LLM 且缺乏 ground-truth 验证；Das et al. "blind baselines beat MIA" 的批评（§2 引用）正是本稿用注入实验回应的——"先验证探针本身"的思路方法学上正确且在 TSFM 领域是首创（concurrent TSFMAudit 为灰盒，Table 1 对比清楚）。
2. **L1 可检 / L3 盲区**的能力边界（§5：L1 AUC 0.82 vs L3 0.51）是可操作的重要负结果，直接告诫社区"同源不同段"的污染黑盒不可审计。
3. 统计纪律出色：窗口级 p≈10⁻⁶¹ vs 数据集级 p≈0.36 的对比（§4）定量揭穿了文献中窗口级检验的反保守性；小控制组效应量膨胀的演示（δ 0.44→0.17，§4）同样有方法学价值。
4. 传递性污染链（Time-300B 目录含 Chronos 全部训练语料，§3）是可验证的实证发现。

### Weaknesses
1. **篇幅与呈现深度不足以支撑主会**：编译版为 6 页（正文约 4 页 + 参考文献 + 复现清单），标题页自注 "working draft"。三张图（Fig. 1 ROC、Fig. 2 效应量、Fig. 3 控制组扩展）呈现了核心证据，但 8×14 审计矩阵的数值主表仍指向仓库文件 `results/main_table.md`（§6）而非随稿呈现；每个探针的定义只有一行公式（§4），无消融（如 P1 的噪声水平 σ、P2 的 surrogate 构造敏感性）；试验台细节（A/B 变体、曝光 regime）压缩在一段内（§5）。以 ICML 主会 8 页标准，这是**完成度约 60% 的短文/workshop 形态**，而非未完成草稿——较此前 tex-only 评估上调，但仍不达主会形态要求。
2. **主审计全为 null result**（min Holm p=0.24，§5）。作者辩称这是诚实协议在 n=14 下的必然——论点成立，但顶会录取需要的是"要么显著发现、要么把 null 变成定理/边界"；目前能力边界只在 3M 参数试验台上成立，向真实 TSFM 的外推被自己承认无效（§5 末"validates probe validity, not real-model memorization strength"）。
3. n=14 数据集、其中仅 2 个 post-cutoff 保证非成员：检验功效问题作者自知，但没有给出功效分析或扩大 n 的路径。
4. 探针设计（扰动敏感度、代理延拓）是 LLM MIA 思路的直接移植，方法新颖度中等。

### Questions
1. 为什么不将非成员集扩展到几十个 post-cutoff 数据集（Open-Meteo 可以批量生成）以恢复功效？
2. TinyTSFM 的 L1 检出依赖"固定 4096 窗口池、30k 步反复曝光"——这与真实 TSFM 一个 epoch 见一次的曝光模式差距多大？可否在试验台上做曝光次数扫描？
3. P3 需要一对已知成员/非成员模型，实际审计闭源 API 时如何选取？

### 打分
- novelty: **7**（注入试验台 + 传递链是真贡献）
- significance: **6**（能力边界结论重要，但目前证据规模小）
- soundness: **7**（统计协议是全稿最强处；试验台外推性受限已自曝）
- clarity: **6**（编译版结构清楚、图证据到位；主表外置 + 细节压缩仍扣分）
- **overall: 5 — weak reject（ICML 主会）**；作为 workshop/短文轨是明确 accept。扩到 8 页（主表入正文、探针消融、功效分析）+ 扩大 post-cutoff 非成员集后可达 borderline–weak accept。

### 池内定位
子领域 5（17 篇）。当前形态位于**约前 55–65%（中位偏下）**；思想（注入式 ground-truth 验证）在池内独特，完成度拖累定位。
- 低于 **ConStat（NeurIPS'24）**、**Proving Test Set Contamination（ICLR'24 Oral）**：两者都给出了可用的统计保证 + 显著实证结果。
- 低于 **LLM Dataset Inference（NeurIPS'24）**：同样强调聚合与统计检验，但那篇有显著结论与完整实验。
- 高于（思想上）**若干纯 MIA 变体**（如 SPV-MIA 的时序对应物尚不存在）：本稿是池内唯一面向 TSFM 的黑盒审计尝试，填的坑是真的。

---

## M3 — How Calibrated Are Zero-Shot Time Series Foundation Models?

### Summary
对三类 TSFM 概率头（Chronos-T5 采样路径 / Chronos-Bolt 分位数头 / Moirai 混合分布头）× 10 数据集 × 2 horizon 做原生区间校准审计：名义 0.80 下 T5 实际覆盖 0.30–0.62（Table 1–2），且在 leakage-free GIFT-Eval BizITObs 子集上更差；提出以模型自身分位数为 CQR 先验的 cold-start 在线共形协议，系统比较 split CP / NexCP / ACI / PID / SPCI-QRF；max-score 联合带恢复整路径覆盖 ≈0.80；给出 NexCP 覆盖差界在该设定下的实例化（Thm 1 / Prop 1）。

### Strengths
1. **审计填补真实空白**：池内 16 篇共形论文没有一篇系统审计 TSFM 原生区间的零样本校准；"3 种头型 × 泄漏受控子集 × 条件覆盖分层"的设计完整。T5 H=96 覆盖 0.30（BizITObs, Table 2）是值得社区知道的数字。
2. **泄漏方向论证聪明**（§5.1/§7）：污染只会让审计结论显得更乐观，因此发现是保守的——这个单向偏差论证干净利落。
3. **实验纪律好**：Holm 校正配对 Wilcoxon（Table 3）、诚实负结果（SPCI-QRF 在 ≤50 窗历史下全面 under-cover，§5.2）、cold-start 曲线（§5.4）、条件覆盖显示失败集中在 level shift 而非波动率（Table 4）——最后一点（"regime-shift risk 而非 volatility risk"）是有洞察的发现。
4. 理论虽轻但用得对：Thm 1 是 Barber et al. Thm 2a 的实例化，解释了为何分位数先验分数使 δ 小、冷启动快（§6），理论与 §5.4 实验呼应。

### Weaknesses
1. **方法新颖度有限**：CQR 分数 + NexCP/split CP 都是现成组件；核心方法学贡献 = "用 TSFM 原生分位数做分数先验"，是一个（正确的）工程选择而非新算法。max-score 联合带也是标准做法的最小实例（§4 自认"minimal online-compatible instance"）。与 Closest work Achour et al. 的四点差异（§2）成立，但每一点都是增量。
2. **主基准仍是 ETT/Exchange/Electricity/Traffic**——正是可能污染的数据集；虽然有 BizITObs 子集兜底，但主表结论的绝对数值仍受污染不确定性影响。BizITObs 仅 3 个数据集，泄漏受控子集本身统计上薄。
3. Theorem 1 的 Assumption 1（miscalibration drift 线性有界）没有在任何数据上被检验（δ 未估计）；理论对"部分恢复"的解释（§6 末）是定性的。
4. 只用 small 变体模型（§7 承认），TimesFM 有分位数头却未纳入审计（§2 提及但未测）——三家族覆盖声明略打折。
5. 格式为 ICLR 2025 模板，与其他手稿目标会议不一致（对内容无影响，仅提示投稿定位）。

### Questions
1. 能否在真实序列上估计 Assumption 1 的 δ 并对比 Thm 1 预测的覆盖差与实测差？
2. joint max band 宽度 ~2×（§1 RQ4）在下游决策中是否可用？建议加一个库存/风险 case study。
3. TimesFM-2.x 的分位数头为何缺席？

### 打分
- novelty: **5**
- significance: **6**（审计数字 + 开源库对从业者直接有用）
- soundness: **7**（多种子、Holm、诚实负结果、泄漏方向论证）
- clarity: **7**
- **overall: 6 — weak accept**

### 池内定位
子领域 6（16 篇）。估计**前 40–50%**。
- 低于 **Adaptive Conformal Inference by Betting（ICML'24）**、**NexCP 类理论工作**：那些有真正的方法/理论原创。
- 低于 **Conformal prediction for multi-dim TS by ellipsoidal sets（ICML'24 Spotlight）**：方法贡献实质。
- 高于/接近 **ResCP（ICLR'26）**、**KOWCPI（ICLR'25）** 的定位方式：M3 的胜点在审计的系统性与实用价值，而非算法；作为"audit + toolkit"论文与 **Exploring the Noise Robustness of Online CP（NeurIPS'25）** 同一档。

---

## M4 — Placebo Text Is All You Need? A Rigorous Audit of Textual Gains in Multimodal TSF

### Summary
对 Time-MMD/MM-TSFlib 的文本条件化增益做输入级安慰剂审计：8 域 × 3 backbone × 5 文本条件 × 5 种子 = 1,200 runs。发现：real 文本从未显著优于 shuffle（0/48，Holm；中位 |Δ|=0.047%，§4.1）；多模态相对单模态的增益真实（40/48，中位 3.2%）但归因分解（§3.2 式）显示中位 84% 来自与文本内容无关的 prior_history 数值通道，其余可被常数句复现（§4.2）；Economy 域上置零文本嵌入反而显著提升 28–31%（§4.3）；错配域文本中位只改变 0.03%（§4.4）；Climate 域 100% 违反 as-of 原则（1982/1984 条事实晚于目标期，Table 3，附 1983 目标配 2021 NOAA 报告的可验证实例）。给出 fair evaluation protocol（§6）。

### Strengths
1. **发现尖锐、可核验、影响大**。这是 Tan et al.（NeurIPS'24 Spotlight，"Are LMs useful for TSF?"）明确搁置的多模态问题的正面回答，且审计的是官方 MM-TSFlib 代码路径（§1"exercises exactly the published code path"）——不是稻草人。prior_history 混杂通道的发现（§3.1"A subtlety of MM-TSFlib matters greatly"）是真正的 gotcha：ts_only−zero 差值分离出与语言无关的数值通道贡献，实验设计教科书级。
2. **归因分解式（§3.2）是方法贡献**：把"多模态增益"拆成数值先验/任意分支正则化/内容变化/对齐语义四项，每项对应一个可执行干预——比 Text Collapse 的表示级证据（§2 承认其为 closest）更进一步，且适用于黑盒提示系统。
3. **Climate 100% as-of 违规**（Table 3）是独立于安慰剂结果的实证贡献，可复核（嵌入日期确定性解析），并诚实指出这些泄漏文本"stale 而非 prophetic"、危险在构建流程（§5）。
4. 统计到位：配对 t + Wilcoxon、Holm over 192 comparisons、Cohen's d（§3.3）；结论措辞克制（"我们不声称文本无用"，Abstract 末）。

### Weaknesses
1. **审计范围与结论口径的差距**：实际只审了 MM-TSFlib 的一种融合方式（frozen GPT-2, prompt_weight=0.1）+ 3 个非 LLM backbone。TaTS、TimeCAP、From News to Forecast 等在 §1 被点名，但没有一个被实际审计；标题与摘要的口吻（"textual gains have not been separated from confounds"）比证据覆盖面更宽。若 TimeCAP 类 LLM-agent 管线在安慰剂下存活，本稿主张将大幅收窄。
2. 篇幅偏短（正文约 6 页当量，2,800 词），Table 2 存在明显排版/数据瑕疵："Algriculture" 拼写错误、∆(r−s) 列出现 "-999%"、"312%" 等未解释的异常值、列头与 5 个条件的对应关系混乱（none/shuffle/static/zero/ts_only 与正文条件命名不一致）——主表的可读性问题在顶会评审中会被扣分。
3. prompt_weight=0.1 是官方默认，但没有对该权重做敏感性分析：若调大文本权重，语义信号是否出现？目前无法区分"文本无信息"与"该融合架构无法利用文本"。§4.3 Economy 结果其实暗示后者。
4. 弱基线担忧的镜像问题：backbone 全为轻量模型，未覆盖 LLM-based forecaster（Time-LLM 类），而那正是文本条件化叙事的主战场。

### Questions
1. 对 TimeCAP 或 CiK Direct Prompt 这类黑盒提示方法跑 shuffle/mismatch（你们声称协议适用，§3.1），结果如何？哪怕 1 个方法 × 2 域也能极大加强普适性主张。
2. prompt_weight 扫描（0.1→1.0）下 real−shuffle 差是否仍为零？
3. Table 2 的 ∆(r−s) 异常值（-999%）如何产生？

### 打分
- novelty: **7**（问题非原创但归因分解 + as-of 审计是新的）
- significance: **8**（直接冲击一个活跃方向的证据基础；协议可被社区采纳）
- soundness: **7**（干预设计与统计强；覆盖面窄是主要折扣）
- clarity: **6**（主表瑕疵、篇幅薄）
- **overall: 7 — accept**（修表格、补 1 个提示式方法审计后有 spotlight 潜力）

### 池内定位
子领域 7（16 篇）。估计**前 20–30%**。
- 低于 **Are Language Models Actually Useful for TSF?（NeurIPS'24 Spotlight）**：那是本稿范式的开创者，规模与覆盖（多方法族消融）更大。
- 高于 **Time-VLM（ICML'25）、CALF（AAAI'25）** 等正向方法论文（就"证据可靠度/领域推动力"而言）：M4 的审计直接质疑这些方法赖以立足的评测；作为 negative-evidence 论文其单位信息量高于池内多数增量融合方法。
- 与 **Context is Key（ICML'25）** 互补且不冲突（CiK 构造了文本确实必要的任务；M4 表明 Time-MMD 不是这样的任务）——这个对照恰好说明 M4 打的是基准而非概念。

---

## M5 — Normalization Under Structural Breaks: Diagnosing and Extrapolating Non-Stationary Statistics

### Summary
诊断 + 方法论文。SynShift 合成套件 + 两层归因（lookback 尾部断点 vs horizon 内断点）显示 RevIN 在断点窗口误差 16–23×（§3.3）；观察到 horizon 层上所有确定性归一化不可区分（"不可能性观察"，Prop 2 支持）；提出 ShiftExtrap-D：参数无关 CUSUM 式检测 → 门控 → GRU 统计外推头（<10k 参数），RevIN 锚定初始化；概率扩展 v2 + 因果持续性先验 v2.1 为最佳配置：标准 6 数据集 × 3 backbone × 2 horizon 上 27/36 胜 RevIN、平均 −2.51%（Table 1）；break-dense 的美债收益率上 6/6、平均 −10.0%（Table 2）；ETTm2-H336 一致回退（+27%）被诊断为 train→test 可预测性偏移，四种可靠性估计变体（三种失败）的负结果研究（§7）。

### Strengths
1. **诊断协议是全稿最有价值的部分**：两层归因把"平均 MSE"拆成失败模式（§3.2），16–23× 的量化 + "horizon 层所有方法 ≈0.33 不可区分"（§3.3）配合 Prop 1/2 的初等但正确的偏差–方差/不可辨识分析，把 folklore 变成了测量结论。这是池内归一化论文（SIN/FAN/DDN）都没做的。
2. 工程安全性设计合理：RevIN 锚定零初始化 + 检测调制门控保证无断点时退化为 RevIN（§4.2–4.3），并有消融支撑（gate_t 扫描，§6）。
3. **诚实度显著高于该子领域平均水平**：主动报告 ETTm2 H336 +27% 失败并给出三探针根因分析（§7）；v2.2/v3/v3b 三个负结果完整报告（§4.5, §7）；跨 torch 版本审计（Reproducibility）。
4. 美债收益率扩展（Table 2, −23.3% @H336）确实展示了目标 regime 下的收益。

### Weaknesses
1. **标准基准上的增益太小**：−2.51% 平均、且 Table 1 中多数行与 RevIN 差在第三位小数（etth2 96: 0.2390→0.2350）。在池内 FAN/DDN/SAN 已存在的拥挤赛道，这个量级不足以支撑"方法"贡献；真正的大增益只出现在自选的 fred_rates（作者自建扩展集）与 Exchange（众所周知的 near-random-walk，−19.9% 很可能主要来自水平外推而非可迁移的方法优势）。
2. **版本动物园问题**：v0/v1/v1.1/v1.2/v1.3/v2/v2.1/v2.2/v3/v3b 十个变体贯穿正文，主张的"最佳配置"（v2.1）是在同一测试集上的事后选择——虽然每个变体的对比是多种子配对检验，但**配置选择本身没有做 held-out**，这正是本稿自己在 §7 批评的做法（val 不能预测 test）。
3. 检测器在真实数据上 93–100% 的窗口都触发（§7 探针 1）：CUSUM 门控实际上退化为 always-on，"detection-gated"的卖点在真实数据上名存实亡——作者自己承认安全性来自零初始化头而非检测器。这削弱了方法叙事的核心。
4. 合成套件上方法并不总赢：trend_break 上 RevIN 更好（0.0066 vs 0.0118，§5），mean_shift 上 v1.2 有 2/5 种子门控虚开——方法在自己设计的诊断套件上不稳。
5. 相关工作引用大量匿名化/占位（"WDAN authors"、"DJ-Norm authors"、"LD/LCD authors"），多为 arXiv 未发表工作，基线对齐存疑（WDAN 重实现 6/36 被自己标注 alignment caveat）。

### Questions
1. v2.1 作为"最佳配置"是否在任何 held-out 意义上被选出？若只看 v2（无先验），结论如何变化？
2. Exchange 的增益在对数差分预处理后还剩多少？
3. fred_rates 未见于任何先前基准，能否在池内已有 break-dense 公共数据（如 ILI、M4 金融子集）上复现 −10% 量级？

### 打分
- novelty: **5**（诊断协议新；方法是检测 + 统计预测的组合，SAN/FAN 已做统计预测）
- significance: **5**（诊断部分有引用价值；方法增益 regime 特定）
- soundness: **6**（多种子 + 配对检验 + 诚实负结果，但配置事后选择与检测器退化问题）
- clarity: **5**（版本编号密集，主线被变体淹没）
- **overall: 5 — borderline，倾向 weak reject**（ICML 主会；重写为"诊断 + 一个方法"的紧凑稿件后可上一档）

### 池内定位
子领域 1（12 篇）。估计**中位附近，约前 50–60%**。
- 低于 **SIN（ICML'24）**、**FAN（NeurIPS'24）**、**TimeBridge（ICML'25）**：这些有更干净的单一方法叙事与更大的相对增益。
- 高于 **U-Mixer（AAAI'24）**、**DERITS（IJCAI'24）** 的证据严谨度（多种子配对检验 + 失败模式归因是它们没有的）。
- 与 **IN-Flow（KDD'25）** 相比：IN-Flow 方法原创性更高，M5 诊断严谨性更高；总体略低于 IN-Flow。

---

## M6 — When Can Normalization Reliability Be Estimated? A Temporal-Boundary Hierarchy for TTA

### Summary
把"自适应归一化何时该信"形式化为按预测起点可用信息集索引的可靠性估计层级 L0（训练先验）/L1（held-out）/L2（在线前缀标签）/L3（在线成熟全 horizon 标签），在严格时间边界协议下映射各层失败模式（11 数据集 × 3 backbone × 5 种子，990 最终 runs）：L0/L1 检测不到 train→test 可预测性偏移；L2 保住增益但对远 horizon 伤害结构性失明（timescale dichotomy）；L3 首次修复 ETTm2-H336（−4~−11.5%）但在 drift 主导序列上保留 +1~6% 残余伤害；用 label-delay regret floor（非正式定理：Ω(min(a,b)·H) per switch）解释残余，门控轨迹测量（0.29–0.64×H 的符号段）支持理论。给出选择规则：默认 L2-fast，仅在 break-dense 长 horizon 启用 L3。

### Strengths
1. **问题形式化有概念价值**："可靠性从哪层数据可估"是 TTA 文献（TAFAS、SOLID、Proceed）没有明确提出的问题；信息集层级 + 严格 maturation 边界（t+H 才可用）是干净的组织框架，与 FAC 并发形式化（§2 自注）说明问题时机对。
2. **四个否定结果形成系统性证据**（train-EMA、held-out pinball、held-out MSE A/B、prefix self-gated 各淘汰一族估计器，§3 L1 段）："连 held-out 上真实 MSE A/B 都失败"（+2.56%，ETTm2 +32–43%）是有说服力的强否定。
3. timescale dichotomy（前缀优势 ≠ 全 horizon 优势，Remark 1）是简单但此前未被指出的观察，Fig. 1 的成熟优势 EMA 轨迹（符号段 0.29–0.64×H）把理论与数据接上了。
4. Tier-2 对照（OneNet/FSNet 在自身协议下 Exchange 0.11 MSE，§5.1）处理得当：明确协议分层而非不公平对比。

### Weaknesses
1. **主要定理是"informal"**（§4 自认，A1–A4 风格化假设 + proof sketch）：作为一篇以理论解释为卖点的论文，核心下界不完整——而这个下界的严格版本正是 M7（paper3）的 Theorem 1。两稿并投时 M6 的理论贡献被 M7 掏空，独立评审下 M6 的定理部分只能算"motivating argument"。
2. **对前作（L0 = M5 的 v2.1）依赖过深**：整个网格以 L0 方法为基线与载体，失败模式（ETTm2 H336）也是前作的失败；独立读者需要读完前作才能理解设置。作为独立投稿自足性不足。
3. **最终数字对 L3 不利**：Table 1 汇总 L3-soft 9/48、+8.38%，L3-regret 9/48、+5.35%——层级中"最高层"在平均意义上是净伤害，"修复 ETTm2"是 48 个设置中约 6 个设置的靶向收益。"selection rule 下组合系统支配 L0"（§5）是按数据集族的事后策略声明，作者自己承认"no single configuration achieves this"——这在方法论上与 M5 的事后配置选择同病。
4. 990 最终 runs 中 L2/L3 的方差、显著性检验没有像 M5 那样给出配对 p 值（Table 1 只有均值）。
5. 无独立方法贡献：L2-fast/L3 门控都是简单 EMA/LCB 机制，价值全押在"地图 + 解释"上。

### Questions
1. 选择规则的诊断量（train-split persistence fraction / level-shift density）能否在不看测试集的前提下正确分类 11 个数据集？请给出该规则的 leave-one-dataset-out 验证。
2. Theorem 1 的严格陈述与证明（或引用 M7）？
3. L3 的 +1~6% 残余伤害与 floor 预测的量级是否定量吻合（而不只是定性）？

### 打分
- novelty: **5**（层级框架新；机制与理论均借力）
- significance: **5**（对 TTA 从业者的指导价值真实但窄）
- soundness: **5**（否定结果扎实；informal 定理 + 事后选择规则 + 缺显著性检验）
- clarity: **6**（写作密度高但组织清楚）
- **overall: 5 — borderline，倾向 weak reject**（与 M7 合并成一篇强论文是明显更优的投稿策略）

### 池内定位
子领域 2（11 篇）。估计**前 50–70% 之间，中位偏下**。
- 低于 **Proceed（KDD'25）**、**TAFAS（AAAI'25）**：两者有独立方法贡献 + 正向主结果。
- 低于 **Fast and Slow Streams（ICLR'25）**：同样是协议反思型论文，但 DSOF 附带了完整方法。
- 高于 **Ada-ReAlign（NeurIPS'24）** 之类在证据诚实度上（M6 的失败地图更系统），但综合竞争力不及池内中位。

---

## M7 — Gating Under Delay: Minimax Theory and a Parameter-Free Lazy-FTL Gate

### Summary
把 TTA 机制的开/关门控形式化为延迟反馈在线学习：证明 minimax 下界 S[½min(a,b)H + ⅛min(a,b)⌊σ²/2Δ²⌋]（Le Cam + Pinsker）与匹配上界（delayed window test），得 minimax regret Θ̃(S(H+σ²/Δ²))；门控 vs always-on 二分定理（Thm 3：有害 regime 驻留 > ≈H+检测时间才值得门控）；提出参数无关 lazy-FTL 门（切换成本 λt = H·|Â|t，O(C) 状态）；66 设置 × 5 种子（1,980 runs）：lazy-FTL −3.45% vs base、−6.46% vs always-on，是最佳可部署策略，且与使用更丰富前缀反馈的 L2-fast 统计不可区分；两个测量陷阱（单位错配、事后参数 hindsight bias）各自足以翻转结论（+2.1% vs −3.5%）。

### Strengths
1. **三部曲中最完整的一篇**：问题（M6 遗留的"残余伤害是否可修"）→ 下界（Thm 1，两项分解：延迟层 + 检测层）→ 匹配上界（Thm 2）→ 可操作二分（Thm 3）→ 参数无关算法（Alg 1）→ 大网格验证 + 仿真定量核对（§6 末"×18 / ×2 matching Theorem 3"）。理论–算法–实验的闭环完整。
2. **Thm 3 是可检查的部署判据**：把"要不要门控"从默认假设变成可用历史驻留时间检验的条件——这是池内 TTA 论文（TAFAS/SOLID/Proceed 全部默认要适应）没有的决策论视角。
3. **两个测量陷阱（§5）有独立方法学价值**：归一化优势信号与 MSE regret 单位错配、以及成熟标签下事后参数的 hindsight bias（"causally legal but biased"），各自被定量为 +2.1% vs −3.5% 的翻转；这类坑任何做延迟门控的人都会踩。
4. 实验诚实：h∗ 变体作为 null result 报告（§6"identical to 4 decimals"）；oracle 差距被正确解读为 floor 的测量而非 headroom；FRED 上 lazy-FTL 不如 L3-soft 的行（Table 2, FRED PT 336: 0.3214 vs 0.3159）没有被隐藏。

### Weaknesses
1. **理论技术含量为"组装"级**：下界 = Le Cam 两点 + Pinsker（标准）；上界 = 窗口均值检验（标准）；lazy switching = Herbster–Warmuth/KV 模板（§4 自引）。新颖处在于把延迟绑定到标签 horizon、比较者随环境切换的设定组合，以及二分定理的表述——组合有价值，但单项技术无新意，ICML 理论审稿人会明确指出这一点。
2. **环境模型强风格化**：分段常数两水平 {+a,−b}、高斯观测噪声、独立 ε——真实优势过程（Fig. 1 轨迹）显然不是两水平分段常数。理论与实验之间的桥是定性的（符号段长度 vs H），没有在真实数据上检验 Θ̃ 率。
3. **实证增益的绝对值小**：−3.45% vs base，且"与 L2-fast 不可区分"（t=−0.7）意味着如果部署方有前缀标签（多数预测系统有），lazy-FTL 的实用增量为零；其价值窄化为"只有全 horizon 延迟标签"的场景。Table 2 中 lazy-FTL 在 66 设置里仅 26 胜 base。
4. 与 M6 相同的依赖问题：base=v2.1、参照策略来自"prior verified grids"（§6），独立可复核性打折。
5. σ² i.i.d. 高斯噪声假设下的 m0 检测项在异方差真实序列上如何取值未讨论。

### Questions
1. 能否在合成环境族之外，用真实数据的优势轨迹拟合 (a,b,σ,L) 并检验 regret 预测的定量吻合？
2. λt = H·|Â|t 中的 H 是否应为"有效延迟"（考虑前缀可见部分）？与 Corollary 1 的 h∗ 全或无结果如何调和？
3. 当优势过程有趋势（非分段常数）时 lazy-FTL 的失败模式？

### 打分
- novelty: **6**（设定组合与二分定理新；技术组件标准）
- significance: **6**（决策论框架对 TTA 社区有引用价值；实用增量窄）
- soundness: **7**（定理完整、上下界匹配、仿真核对、陷阱量化）
- clarity: **7**（三部曲中写得最好的一篇）
- **overall: 6 — weak accept**

### 池内定位
子领域 2（11 篇）。估计**前 35–45%**。
- 低于 **Online TSF with Theoretical Guarantees（NeurIPS'25）**：后者在更一般设定下给出保证，理论深度更高。
- 高于 **Ada-ReAlign（NeurIPS'24）**、**ACCUP（KDD'25）**：M7 的理论–实验闭环与诚实度更强。
- 与 **Proceed（KDD'25）** 相比：Proceed 方法影响面更广（主动适应主结果为正且大），M7 提供的是 Proceed 类方法"何时不该用"的对偶视角；综合略低于 Proceed。

---

## M8 — TAPR: Time-Adaptive Patch Routing for Forecasting Irregularly Sampled Multivariate Time Series

### Summary
ODE-free、graph-free 的 IMTS 预测 transformer：每通道等观测数自适应 patch、可学习 Fourier 时间编码 + patch 元数据、时间距离/通道对加性注意力偏置、任意时间戳 cross-attention 解码。在官方 t-PatchGNN 基准（PhysioNet/USHCN/Activity，5 种子配对 t 检验）上 USHCN（t=−9.6）与 Activity（t=−2.9）显著优于 t-PatchGNN，参数量 28–39%；PhysioNet 落后 4.5%。消融显示自适应 patching 在稀疏气候数据上关键、解码器增益来自深度；报告多尺度金字塔与 IMTS 上实例归一化等负结果，及规则网格退化研究（§4.5）。

### Strengths
1. **假设检验式的立意好**："普通 transformer + 正确的 tokenization 能否匹配图/ODE 模型"是有价值的简化问题，与池内 An Analysis of Linear TSF Models（ICML'24）同类的"简单性检验"精神。等观测数（采样自适应而非内容自适应）patching 是一个干净的想法，且消融支持（fixed K: 5.43 vs 5.31 / 0.4817 vs 0.4786，§4.2）。
2. 实验纪律好：官方 pipeline/evaluator/splits（§4 Protocol），基线由作者在同协议下重跑，5 种子配对 t，与 Hi-Patch 引用数字的可比性用其独立复现的 t-PatchGNN 数字交叉校验（脚注 1）——引用他人数字时这是正确做法。
3. 诚实报告突出：PhysioNet 落后 4.5% 放进 Abstract；金字塔探针失败（5.68 vs 5.31，§4.3）、IMTS 上实例归一化有害（§4.2）、规则网格上"竞争力而非优越性"（§4.5：ETTh1 0.442 vs PatchTST 0.397）全部如实。
4. 效率主张有数据（USHCN 训练 1.8× 快、2.9× 省内存，§4.4），且 PhysioNet 上内存反超也如实报告。

### Weaknesses
1. **基准太窄是致命伤**：只有 3 个数据集，其中 TAPR 赢 2 输 1；而池内同赛道的 t-PatchGNN（ICML'24）、Hi-Patch（ICML'25）、HyperIMTS（ICML'25）标准评测是 4 个数据集（含 MIMIC）——MIMIC 缺席（credentialed access，§4）使得与全部近期工作的正面比较不完整。18 篇池内 IMTS 论文构成 2024–2026 最拥挤的子领域之一，3 数据集、2/3 胜的证据量不足以在其中立足。
2. **对最强并发工作是净输**：Hi-Patch 引用数字下 TAPR 仅 USHCN 占优（0.479 vs 0.494），Activity 略输（2.63 vs 2.57），PhysioNet 明显输（5.31 vs 4.86）——"更简单 + 更小"的卖点需要在准确率大体持平时才成立，PhysioNet 上 ~9% 的差距超出了"trade-off"的舒适区。
3. 方法组件全部现成（Fourier 时间编码 = mTAN/时间嵌入常规、加性衰减偏置 ≈ ALiBi 变体、cross-attention 查询解码 = mTAN/GraFITi 已有）；唯一新颖点是等观测数 patching——一个好的工程决策，撑不起方法论文的骨架。
4. USHCN 只有 5 通道、预测 1 个月，是三者中最弱的数据集；主要显著胜利（t=−9.6）恰好在最小最简单的数据集上。
5. 正文 5 页 + 附录，篇幅与证据量都低于 ICML 主会常态。

### Questions
1. MIMIC-III 的 credentialed access 是标准 PhysioNet 流程，多数同行都完成了；为何不做？这是当前最大的可信度缺口。
2. 等观测数 patching 与 t-PatchGNN 的 transformable patch 的差异能否在同一 backbone 下做受控对比（只换 patching）？
3. 41 通道的通道对偏置表 O(D²·h) 在数百通道场景如何扩展？

### 打分
- novelty: **4**
- significance: **4**（简化叙事有价值，但输给并发工作 + 基准窄）
- soundness: **6**（协议干净、检验规范、诚实；覆盖不足）
- clarity: **7**
- **overall: 4 — weak reject**（补 MIMIC + 至少再赢一个数据集、或把"简单性检验"做成系统研究后再投）

### 池内定位
子领域 3（18 篇，池内最大）。估计**后 25–35%**。
- 低于 **t-PatchGNN（ICML'24）**、**Hi-Patch（ICML'25）**、**HyperIMTS（ICML'25）**：三者是其直接对标，TAPR 综合准确率不占优。
- 低于 **GraFITi（AAAI'24）**：GraFITi 的图表述有概念贡献且全面赢基线。
- 高于 **TimeCHEAT（AAAI'25）** 一类增量组合工作（就实验纪律与诚实度而言），但准确率证据不及。

---

# 汇总表（从强到弱）

| 排名 | ID | 手稿 | novelty | signif. | sound. | clarity | overall | 决策档 | 池内定位（同子领域） |
|---|---|---|---|---|---|---|---|---|---|
| 1 | M4 | Placebo Text（多模态审计） | 7 | 8 | 7 | 6 | **7** | **accept** | 子领域7 前 20–30% |
| 2 | M1 | LiveTS（活基准） | 6 | 7 | 6 | 7 | **6** | **weak accept** | 子领域4 前 35–45% |
| 3 | M7 | Gating Under Delay（极小极大 + lazy-FTL） | 6 | 6 | 7 | 7 | **6** | **weak accept** | 子领域2 前 35–45% |
| 4 | M3 | Conformal TSFM 审计 | 5 | 6 | 7 | 7 | **6** | **weak accept** | 子领域6 前 40–50% |
| 5 | M5 | ShiftExtrap-D（断点归一化） | 5 | 5 | 6 | 5 | **5** | **borderline / weak reject** | 子领域1 约前 50–60% |
| 6 | M6 | 可靠性层级 L0–L3 | 5 | 5 | 5 | 6 | **5** | **borderline / weak reject** | 子领域2 前 50–70% |
| 7 | M2 | TSFM 污染审计 | 7 | 6 | 7 | 6 | **5** | **weak reject（主会）/ workshop accept** | 子领域5 前 55–65% |
| 8 | M8 | TAPR（IMTS transformer） | 4 | 4 | 6 | 7 | **4** | **weak reject** | 子领域3 后 25–35% |

注：M2 与 M5/M6 同为 overall 5；排序上 M2 置于 M5/M6 之后，因其证据体量（4 页正文、全 null 主结果）低于两者的完整网格，尽管其单位篇幅的思想密度更高。

## 横向观察（给作者团队）

1. **共同优点**：这批手稿的统计纪律（多种子、配对检验、Holm 校正、负结果报告、JSONL 溯源）整体高于池内 2024 年论文的平均水平——这是真实的竞争优势，应保持。
2. **共同弱点是证据规模与"故事宽度"不匹配**：M1 205 条日频序列、M2 n=14、M3 主基准可能污染、M4 只审一条管线、M8 三个数据集。顶会拒稿理由里"claims exceed evidence"会反复出现。
3. **M5/M6/M7 三部曲应重组**：M7 的严格定理 + M6 的层级/否定结果地图 + M5 的诊断协议合并成一篇（方法细节压缩进附录），是一篇明确 accept 级的论文；分开投则三篇各自平庸且互相引用匿名前作、自足性差，还有被同一审稿人识别为 salami-slicing 的风险。
4. **M4 与 M2 的互补**：M4 是完成度高的审计（收），M2 是想法最独特但呈现最压缩的审计；M2 若扩成 8 页完整稿（主表入正文 + 探针消融 + 功效分析 + 更大 post-cutoff 控制组），是下一轮周期里最有上升空间的一篇。
5. **M1 + M2 的组合叙事**（结构性预防 vs 事后检测，两稿已互引）在一篇 position/benchmark 联合投稿中可能比各自单投更强。
