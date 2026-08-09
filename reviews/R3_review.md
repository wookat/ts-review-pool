# 8 份匿名时间序列手稿 · AC 级独立盲评报告

**评审人视角**：资深时间序列顶会 Area Chair，侧重领域影响力与定位。
**对比池**：wookat/ts-review-pool（101 篇 2024–2026 顶会论文，7 个子领域）。
**评审纪律**：完全盲评，只依据手稿内容；按 ~25% 录取率的真实顶会标准打分；每个判断给出手稿内证据。

**材料状态说明（必须公开）**：M1、M3、M4 取自各 GitHub 仓库 paper/（或 paper/latex/）下的 main.pdf；M5–M8 取自 xu-1 上各论文目录的最新 ICML 匿名版 PDF（M5: a1-norm2/paper/icml2026/main.pdf；M6: paper2/icml2026/main.pdf；M7: paper3/icml2026/main.pdf；M8: a2-irregular/paper/icml2026/tapr.pdf）。M2 的编译版 main.pdf 初次因 .gitignore 误排除，后已补推（commit 9d610e2）；本版报告中 M2 的评审已基于完整 PDF 重做，其余 7 份不变。

---

## 汇总排序表（从强到弱）

| 排名 | 手稿 | 标题（简） | 子领域 | Nov | Sig | Snd | Clr | Overall | 决策档 | 池内百分位（同子领域） |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | M4 | Placebo Text Is All You Need? | 多模态/文本条件化 | 7 | 7.5 | 7 | 7 | **6.9** | accept（弱） | ~70–75% |
| 2 | M7 | Gating Under Delay（minimax + lazy-FTL） | 在线/TTA/延迟反馈 | 7 | 6 | 7.5 | 7 | **6.5** | weak accept | ~60–70% |
| 3 | M1 | LiveTS：防泄漏持续刷新基准 | 基准与评测方法学 | 6.5 | 6.5 | 6.5 | 7 | **6.2** | weak accept | ~55–65% |
| 4 | M3 | TSFM 校准审计 + 在线共形工具箱 | 共形/UQ | 5.5 | 6 | 7 | 7 | **5.8** | borderline（偏正） | ~45–55% |
| 5 | M5 | Normalization Under Structural Breaks | 非平稳/归一化 | 6 | 5.5 | 6.5 | 6 | **5.5** | borderline | ~40–50% |
| 6 | M2 | TSFM 预训练污染审计 | 污染/记忆化 | 6 | 5.5 | 6 | 6.5 | **5.3** | borderline（偏负） | ~30–40% |
| 7 | M6 | 归一化可靠性估计的时间边界层级 | 在线/TTA/延迟反馈 | 5.5 | 5 | 6 | 6 | **5.2** | borderline（偏负） | ~35–45% |
| 8 | M8 | TAPR：IMTS 的自适应 patch 路由 | 不规则采样/IMTS | 5 | 4.5 | 6.5 | 7 | **5.0** | weak reject | ~30–40% |

注：百分位指"在对比池同子领域已发表论文中，若与它们同场竞争，本稿大致处于第 X 百分位"。池内论文全部已被录取，因此 50% 左右即意味着达到录取线附近。

---

## M1 — LiveTS: A Leakage-Proof, Continuously-Refreshed Benchmark for Zero-Shot Time Series Forecasting

**Summary**。构建一个持续刷新的零样本 TSF 评测基准：6 个公开实时域、205 条日频序列，每源至少两个冗余数据通道、PIT 语义与 SHA-256 manifest（§3.1）；核心机制是按模型公开 release date 做逐模型时间门控，只在评测截止晚于该模型发布日的窗口上打分（§3.2）。历史模拟覆盖 3 个滚动 cutoff（2025-01-01/07-01/2026-01-01）、50,284 个预测窗口，报告 MASE/CRPS/WQL 与 cross-series bootstrap CI、DM-HLN+Holm（§3.3）。主结果（§4.2 表1）：TimesFM-2.5 geo-MASE 0.789 [0.711,0.873]（但仅 820 窗口/单一 clean cutoff）、Chronos-Bolt ~0.89、PatchTST 1.099、seasonal naive 1.181；表2 显示 crypto/FX 上几乎无模型胜过 seasonal naive，而季节性域上 TSFM 有 10–40% MASE 提升。

**领域定位与差异化**。属子领域4（基准/评测）。差异化真实成立：池内 TFB（VLDB 2024）、ProbTS（NeurIPS 2024 D&B）、TAB（VLDB 2025）都是静态基准，无法防止 TSFM 预训练截止之后的重叠；LiveBench（ICLR 2025 Spotlight）证明了 live/refreshed 评测在 LLM 领域的价值，但没有时间序列版本。M1 是把 LiveBench 范式移植到 TSF 并加上 release-date 门控的第一个系统尝试（手稿 §2 也诚实列出了 GIFT-Eval/fev-bench 作为静态对照）。评测型论文引用与跟进潜力高：若基础设施真能持续运行，它会成为 TSFM 论文的标配对照。

**主要风险**。(1) 目前提交的证据仍是"历史模拟"而非真正上线运行的 live 服务——防泄漏保证的价值在于持续运营，而免费数据源 rate-limit、区域覆盖偏差已被 §6 自认；基准论文的生命力取决于维护承诺，这在匿名评审中无法验证。(2) 规模偏小：单变量、日频、205 条序列、6 域，与 GIFT-Eval 数千数据集相比覆盖面窄，"TSFM 在 crypto/FX 不敌 naive"的结论可能被批评为域选择效应。(3) 最引人注目的数字（TimesFM-2.5 0.789）恰恰基于最少的窗口（820, 单 cutoff），§4.4 的 member-skill 效应 Cliff's δ 0.47–0.85 在 Holm 后不显著——手稿自己也承认统计功效不足。(4) supervised baselines 只用 generic 配置（§6），"TSFM 优于 supervised"部分被削弱为 lower-bound 对比。

**打分**：novelty 6.5 / significance 6.5 / soundness 6.5 / clarity 7 → **overall 6.2，weak accept**。

**池内定位**：子领域4内约 **55–65 百分位**。高于 TAB（VLDB 2025，静态扩展性质的增量）：M1 解决的是该子领域公认且尚无人解决的泄漏问题。低于 LiveBench（ICLR 2025 Spotlight）：LiveBench 上线即有大规模社区采用与月度刷新记录，M1 还只有一次性历史模拟；也低于 "This Time is Different / BOOM"（NeurIPS 2025）级别的资源规模。与 ProbTS（NeurIPS 2024 D&B）大致同档——方法学贡献清晰但覆盖面有限。

---

## M2 — Auditing Pretraining-Data Contamination in Time-Series Foundation Models: Black-Box Probes, Validated on Ground Truth（基于补推的编译版 PDF 重评）

**Summary**。提出 TSFM 预训练数据污染的严格黑盒审计协议（仅需 predict API）：(i) 四级污染分类（L1 exact / L2 transformed / L3 same-source / L4 global-event）加跨 5 个公开语料库的机器可读成员数据库，含传递污染链（§3 的关键发现：Time-300B 目录内含 Chronos 全部 TSMixup 训练语料，Time-MoE 因此传递暴露于所有 Chronos in-domain 数据集）；(ii) 三探针族（P1 扰动敏感度/P2 代理延续 margin/P3 跨模型 skill 差）配保守的 dataset 级统计协议（§4：窗口级 p≈1e-61 vs 数据集级 p≈0.36 的反保守性演示；小非成员控制集膨胀效应量——Time-MoE P2 δ 从 0.44 缩到 0.17，Fig. 3）；(iii) 受控注入试验台（~3M 参数 TinyTSFM，A/B 互补变体×2 seeds）给出首个黑盒污染探针的 ground-truth ROC 与能力边界（§5：L1 P1/skill AUC 0.82、TPR 0.70–0.85 @ FPR≤0.25；L3 全探针 AUC≈0.51）。实证审计 8 TSFM × 14 数据集（含 2 个 post-cutoff 保证非成员，§6）：TimesFM-2.0 最强（skill 未校正 p=0.010、δ=0.85），Timer（仅同源）弱（δ≤0.25），方向与能力边界一致；但**无检验在 Holm 后显著**（最小 Holm p=0.24），作者明确把机制解读降为"evidence-consistent hypothesis"。附与并发 TSFMAudit 的逐项对比表（Table 1）与可复现性清单。

**领域定位与差异化**。属子领域5（污染/记忆化）。差异化方向正确且现在论证完整：池内 Min-K%/Min-K%++/Infilling Score/LLM Dataset Inference/ConStat 全部针对 LLM 文本，TSFM 黑盒污染审计几乎空白；与并发 TSFMAudit 的区分（黑盒 vs 灰盒、ground-truth 验证 vs 文档证据）在 Table 1 中交代清楚。"用受控注入给探针自身提供 ROC 验证"直接回应了 Das et al. "blind baselines beat MIA"的批评，方法论严谨度高于多数 LLM MIA 工作；传递污染链与成员数据库是可复用的社区资产。与 M1 的 leakage 议题互补，引用潜力真实存在。

**主要风险**。(1) 核心实证结论仍是全面 null：真实 TSFM 审计无一 Holm 后显著，论文的硬贡献因此收缩为"能力边界 + 方法论警示"；n=14 的功效分析仍缺失，null 是"无污染"还是"无功效"仍分不开（§8 仅一句带过）。(2) 3M 参数、重复暴露的试验台到 200M–500M 级真实 TSFM 的外推有效性未被论证（§5 自认"validates probe validity, not real-model memorization strength"）——而记忆强度随规模增长是 LLM 文献的共识，这意味着能力边界本身可能不外推。(3) L3 盲区（AUC 0.51）意味着实践中最常见的污染形态不可检测，工具的实际效用受限。(4) 成稿形态：正文仅 ~4 页、自标"working draft"，且 Reproducibility 附录直接列出非匿名 GitHub 仓库 URL——若以此版投双盲会构成匿名违规，必须修正。

**打分**：novelty 6 / significance 5.5 / soundness 6 / clarity 6.5 → **overall 5.3，borderline（偏负）**。相对前一版评审的主要上调来自：完整 PDF 确认了图表（Fig. 1–3）、TSFMAudit 对比、post-cutoff 控制集扩展实验与可复现性清单都已到位，成熟度折扣大部分解除；未变的是 null 主结果、试验台尺度裂缝与功效分析缺失，这些决定它在 25% 线下方。若补功效分析 + 一个中等规模（≥50M）验证模型，可升至 weak accept。

**池内定位**：子领域5内约 **30–40 百分位**。低于 LLM Dataset Inference（NeurIPS 2024）与 ConStat（NeurIPS 2024）：两者同样强调推断严谨性，但交付了可得出阳性判定的可复用流程；M2 的阳性主张止步于未校正趋势。高于把 Min-K% 直搬时间序列的平移式工作：四级分类、传递污染链与 ground-truth 注入验证是真实方法学增量，也高于仅有文档证据的 leakage 盘点类工作（其引用的 Meyer 2025）。最接近 Healthcare FM Memorization Risk（NeurIPS 2025），但后者有阳性发现与后果论证，M2 尚无；与并发 TSFMAudit 互有长短（M2 黑盒 + ground-truth 验证，对方标签规模更大）。

---

## M3 — How Calibrated Are Zero-Shot Time Series Foundation Models? An Online Conformal Audit and Calibration Toolkit

**Summary**。对 3 个 TSFM 家族 × 10 数据集 × 2 horizon 做零样本区间校准审计：nominal 0.80 下 Chronos-T5 原生覆盖率仅 0.62/0.50（H=24/96），在 leakage-free 子集上更差（0.40/0.30），Bolt/Moirai 约 0.77–0.78（§5.1）；随后证明轻量 split CP/NexCP 即可恢复 0.81–0.86 覆盖，并给出 joint max band（M_t=max_h s_t^h）把 joint coverage 从 per-step 的 0.02–0.31 提到 ~0.80（§5.1–5.2，代价是宽度显著增大，如 33.37）。附带负结果：SPCI-QRF 在 50 窗口历史下只有 0.68–0.79（§5.2）；level-shift 分位分析显示 Moirai 高位移四分位覆盖 0.657 vs 低位移 0.840（§5.3）。§6 给出 quantile-prior 分数下 NexCP 的 drift bound。

**领域定位与差异化**。属子领域6（共形/UQ）。定位是"审计 + 工具箱"而非新 CP 算法：所用方法（split CP、NexCP、SPCI、max-band）均为已有；理论是已有 bound 在特定 score 下的实例化。差异化的真实增量在于**对象**——池内 ACI-betting、KOWCPI、Error-quantified CI、ResCP 等都在自训练预测器上做在线 CP，没有人系统回答"零样本 TSFM 的原生分位数到底多不准、多便宜能修好"。结合 leakage-free 子集控制预训练重叠（§3.3）是聪明且必要的设计，与污染议题挂钩。这类审计对从业者有直接价值，会被 TSFM 应用论文引用。

**主要风险**。(1) 方法学新颖性低是硬伤：CP 社区评审可能判为"application paper"，joint max band 是标准技巧且付出的宽度代价（33.37）在实用上接近无信息。(2) 覆盖面：univariate、小模型变体、每数据集仅 50 窗口（§7 自认），HopCPT/CopulaCPTS 未入主表——与池内 MultiDimSPCI（ICML 2024 Spotlight）等相比实验强度不占优。(3) 核心发现（T5 校准差、Bolt 尚可）有多少来自模型本身 vs 数据泄漏的混杂，只在 BizITObs 一个子集上分离。

**打分**：novelty 5.5 / significance 6 / soundness 7 / clarity 7 → **overall 5.8，borderline（偏正）**。

**池内定位**：子领域6内约 **45–55 百分位**。低于 KOWCPI（ICLR 2025）与 Error-quantified CI（ICLR 2025）：两者有真正的新估计器/新遗憾界；也低于 MultiDimSPCI（Spotlight 级实验+理论）。高于池内偏应用端的 Conformalized TS with Semantic Features（NeurIPS 2024）一类：M3 的审计对象（零样本 TSFM）更及时、负结果与泄漏控制更严谨。与 Distribution-informed Online CP（ICLR 2026）大致同档。

---

## M4 — Placebo Text Is All You Need? A Rigorous Audit of Textual Gains in Multimodal Time Series Forecasting

**Summary**。对多模态 TSF 的"文本增益"做安慰剂对照审计：8 个 Time-MMD 域 × 3 backbone × 5 种文本条件 × 5 seeds，共 1,200 runs（§3）。设计核心是 5 个 placebo 条件（real/shuffle/static/zero/ts-only 等）加一条干净的归因恒等式（§3.2：ts_only−real 分解为四段）。发现：real text 在 48 个设置中 **0 个** Holm 显著优于 shuffled text（中位相对 MSE 差 0.05%）；多模态优于单模态的 40/48 增益中 **84% 可归因于 numeric prior history 通道**而非文本语义；Economy 域 zero embedding 反而比 real 好 27.9–30.7%；Climate 文本 1,982/1,984 条事实晚于目标期结束，**100% 行违反 as-of 约束**（§5，给出 1983 年目标配 2021 年 NOAA 报告的实例）。

**领域定位与差异化**。属子领域7（多模态/文本条件化）。这是一篇对整个子领域的"负结果 + 泄漏揭露"论文，直接打击池内 Time-MMD（NeurIPS 2024 D&B）生态与 MM-TSFlib 上的一批跟进工作。方法论上是 "Are Language Models Actually Useful for Time Series Forecasting?"（NeurIPS 2024 Spotlight）的直接精神续作，但把 ablation 升级为 placebo 对照 + 加性归因恒等式 + as-of 泄漏审计，设计更接近因果实验标准。Climate 100% 泄漏这一发现单独就有领域级影响：Time-MMD 是该子领域的事实标准基准，会迫使后续论文重做实验。此类论文引用可预期很高（debunking + 基准缺陷揭露双重属性）。

**主要风险**。(1) 范围限定：只审计了 MM-TSFlib 官方 fusion + frozen GPT-2 + Time-MMD（§7 自认）；辩护者会说 TimeCAP/CiK/TaTS 等更强的文本利用机制未被审计，结论只能是"该流行 pipeline 的文本增益是安慰剂"，不能外推到"文本无用"。手稿标题的问号保留了分寸，但评审会要求至少一个非 GPT-2、非 MM-TSFlib 的对照。(2) §7 承认 TaTS/TimeCAP/CiK 扩展、causal imputation 重跑、LLM spoiler probe 均"planned/未运行"——若成稿仍留这些 TODO，完整性会被顶会诟病。(3) 负结果论文的 Holm-192 校正虽严谨，但也可能掩盖小而真实的效应；等价检验（TOST）缺失。

**打分**：novelty 7 / significance 7.5 / soundness 7 / clarity 7 → **overall 6.9，accept（弱）**——8 稿中唯一一篇我预期"即使有缺陷，程序委员会也会想收"的论文，条件是补齐至少一个替代 fusion 机制的对照。

**池内定位**：子领域7内约 **70–75 百分位**。高于 Time-MMD 本身与 ChatTime/CALF 等建模型论文：M4 改变的是子领域的评测规范而非增加一个模型。与 "Are LLMs Actually Useful for TSF?"（NeurIPS 2024 Spotlight）同一血统，目前完成度略低于它（该文覆盖多个方法族），但泄漏审计部分是其没有的增量。低于 MAESTRO（NeurIPS 2025 Spotlight）这类"正向且大规模"的贡献仅在建设性维度，批判性维度反而更强。

---

## M5 — Normalization Under Structural Breaks: Diagnosing and Extrapolating Non-Stationary Statistics for Long-Term TSF

**Summary**。论证 RevIN 类归一化在结构断点下失效并部分修复之。贡献链：(a) SynShift 诊断——断点窗口误差是稳定窗口的 16–23×（§3.3：DLinear+RevIN 稳定 MSE 0.0088 vs 断点窗口 0.166）；(b) 两层归因：lookback-tail break（统计量过时，可修）vs horizon-internal break（理论上确定性归一化不可辨识，§4.6 给出 irreducible excess MSE p(1−p)δ²）；(c) 方法 ShiftExtrap v2.1：检测门控 + 统计量外推 + causal persistence prior。主网格 9 数据集 × 3 backbones × 7 归一化 × 2 horizons × 5 seeds：v2.1 27/36 胜 RevIN、平均 −2.51%，Exchange H336 −19.9%，FRED 国债收益率 6/6、平均 −10.0%（最大 −23.3%）；但 ETTm2 H336 恶化 +26.7–29.9%（§5, §8）。可复现性一节（cross-torch 版本审计、540-run 重跑）异常扎实。

**领域定位与差异化**。属子领域1（非平稳/归一化）——池内最拥挤的赛道之一（SIN/FAN/DDN/TimeBridge/IN-Flow/PULSE…）。M5 的差异化主张是"诊断 + 归因 + 不可能性"而非又一个归一化模块，这个定位在该子领域确实缺位：池内 12 篇没有一篇系统区分 lookback break 与 horizon break。诚实度突出（负结果 v3/v3b 全网格报告，§9 自认 horizon 层未解）。但作为方法论文，其绝对增益（平均 −2.51%）在这个以"每年 −1~3% 刷榜"闻名的子领域里不具区分度，真正亮眼的只有 Exchange 与 FRED——恰是持久性先验天然占优的金融数据。

**主要风险**。(1) ETTm2 H336 +27–30% 的系统性失败被留给续作（M6/M7）修，单看本稿方法不安全，评审会问"一个 27/36 但可能 +30% 爆炸的插件谁敢用"。(2) 理论贡献（§4.6）本质是均值切换下的 bias-variance 基本事实，不可辨识性"定理"的顶会含金量有限。(3) 与 FAN/DDN/SAN 的对比中这些基线互有胜负（§5 LD 21/36, −0.47%），未能证明对 2025/2026 新方法（WDAN/LD/DJ-Norm，均已在其引用列表）的一致优势。(4) 收益集中于金融/收益率数据，泛化叙事偏窄。

**打分**：novelty 6 / significance 5.5 / soundness 6.5 / clarity 6 → **overall 5.5，borderline**。

**池内定位**：子领域1内约 **40–50 百分位**。高于 U-Mixer/DERITS（AAAI/IJCAI 档的模块化增量）：M5 的诊断协议与失效归因是它们没有的方法学贡献。低于 FAN（NeurIPS 2024）与 TimeBridge（ICML 2025）：两者在同样的基准上给出更大、更均匀的增益且无 ETTm2 级别的爆炸失败。与 SIN（ICML 2024）比较：SIN 的"选择哪个统计量"框架与 M5 的"统计量何时失效"互补，完成度与安全性上 SIN 占优。

---

## M6 — When Can Normalization Reliability Be Estimated? A Temporal-Boundary Hierarchy for Test-Time Adaptation

**Summary**。M5 的直接续作（以 M5 方法为 L0）。提出按预测原点可用信息集分层的可靠性估计层级 L0–L3（训练先验/held-out 验证/在线前缀标签/在线成熟全 horizon 标签），在严格时间边界（窗口标签 t+H 才可用）下映射每层失效模式：L0/L1 无法检测 train→test 可预测性漂移（连 held-out forecast-MSE A/B 门控也失败，+2.56%，ETTm2 +32–43%）；L2 前缀门控保住 L0 全部收益（−0.12%, 26/48）但对远 horizon 伤害结构性失明（timescale dichotomy）；L3 是第一个修复 ETTm2-H336 的层级（−4~−11.5%，FRED 最多 −16.2%）但在 ETTh1/Exchange 留 +1~6% 残余伤害。用 label-delay regret floor（非正式定理：Ω(min(a,b)·H) per switch）解释残余，Figure 1 的 gate 轨迹显示 advantage 符号段长 0.29–0.64×H 恰在残余伤害处。990-run 最终网格 + 4 个 disciplined 负结果。

**领域定位与差异化**。属子领域2（在线/TTA/延迟反馈）。概念贡献（信息集层级 + prefix/full-horizon 二分）在池内确实没有对应物：OneNet/FSNet/PROCEED 走即时标签的 continual 协议，TAFAS/FAC 引入了 matured/partial-label 协议但未刻画"可靠性从哪层数据可估计"。协议区分（§5.1 把 OneNet 单列一档并解释其 Exchange 0.11 是 delay=1 的推论）是干净的公平性处理。但作为独立论文它的问题是：主定理"informal"（§7 自认 regime 模型 stylized），最优主张是政策级的（"selection rule 下组合系统支配 L0"——按数据族选配置，无单一配置达成，§5），阳性结果集中在 ETTm2-H336 + FRED 这两个 M5 遗留病灶上。它读起来更像 M5→M7 之间的过渡性系统报告。

**主要风险**。(1) 定理非正式且其正式化版本恰好是 M7 的贡献——同一投稿周期内两稿切分（M6 出 hierarchy+informal floor，M7 出 minimax+算法）会被程序委员会视作 salami slicing，合并后才是一篇强论文。(2) L3 的绝对表现差（48 设置 9/48 胜、平均 +5~8%），"targeted repair"依赖事先知道数据族属于哪个 regime——诊断规则（train-split persistence fraction）只给了一句话，未验证其判别力。(3) 所有实验共享 M5 的 11 数据集网格，外部效度未扩展。

**打分**：novelty 5.5 / significance 5 / soundness 6 / clarity 6 → **overall 5.2，borderline（偏负）**。

**池内定位**：子领域2内约 **35–45 百分位**。低于 OneNet（NeurIPS 2023 生态之后的池内 online 方法）与 PROCEED（KDD 2025）：它们提供的是可直接部署的正向方法；M6 提供的是失效地图 + 政策建议。高于纯粹的 detect-then-adapt 式增量：timescale dichotomy 是会被后续 TTA 论文引用的概念。最接近 FAC（其协议被 M6 采纳），但 FAC 有正式化协议 + 新校准方法，M6 是在其框架内的失效分析。

---

## M7 — Gating Under Delay: Minimax Theory and a Parameter-Free Lazy-FTL Gate for Test-Time Adaptation

**Summary**。把"是否开启 TTA 机制"形式化为延迟反馈在线学习并三步解决。理论：任何 matured-label 政策每次 regime 切换付 ≥ S[½min(a,b)H + ⅛min(a,b)⌊σ²/2Δ²⌋]（Le Cam + Pinsker，Thm 1），delayed-window test 达到匹配上界（Thm 2），minimax regret Θ̃(S(H+σ²/Δ²))；以及 gate-vs-always-on 二分定理（Thm 3）：负 regime 驻留 > ≈H+检测时间时门控才有利，ℓ⁻≤H/2（对称损失）时无政策胜 always-on。算法：参数无关 lazy-FTL 门（切换代价 λt=H·|Â|t，O(C) 状态，默认 off）。实验：66 设置 1,980 runs——lazy-FTL 是最佳可部署平均（vs base −3.45%、vs always-on −6.46%），在伤害数据集上残余 ≤2%（always-on +12–31%），在受益处捕获至 −51%，且仅用成熟标签就追平使用严格更丰富前缀反馈的 L2-fast（−0.5%, t=−0.7）。另报告两个测量陷阱（单位错配、hindsight 参数）各自足以翻转结论（+2.1% vs −3.5%），oracle 差距（−13.1%）被论证为 delay floor 的度量而非可达空间。

**领域定位与差异化**。属子领域2。这是 M5–M7 三部曲中最完整的一篇：问题形式化新（把 TSF-TTA 门控接入 delayed-feedback online learning，池内与引用文献中均无先例——Joulani/Weinberger 系列没有"delay=label horizon + comparator 随环境切换"的结构）、下界/上界/二分/算法/大规模实验闭环、测量陷阱一节有独立方法学价值（任何 delayed-gating 复现都会踩）。Theorem 3 把"该不该门控"变成可检验的环境条件，这是能被 TTA 后续工作直接引用的可操作结论。

**主要风险**。(1) 实用增益的绝对量小：−3.45% vs base，且与 L2-fast 统计不可区分——雪上加霜的是"从不适配"（base 本身）在多数数据集上已是最优或近优（表2 中 lazy-FTL 多数行 ≈ v2.1），批评者会说整篇论文证明了"最好的门就是基本不开门"。作者可辩护为这正是理论预言（诚实），但顶会对"负增益防御性方法"的热情有限。(2) 理论在两水平 + 高斯噪声的 stylized 模型中（§8 自认），At 分段常值假设与真实漂移差距大；上界与下界只在 a≍b 时匹配。(3) 与 M6 的切分问题同上：base、L2/L3 参照全部来自"prior verified grids"（Anonymous 2026a/b 即 M5/M6），三稿共享网格与叙事，独立性存疑。(4) 收益仍集中在 FRED/ETTm2。

**打分**：novelty 7 / significance 6 / soundness 7.5 / clarity 7 → **overall 6.5，weak accept**。

**池内定位**：子领域2内约 **60–70 百分位**。高于 Detect-then-Adapt 式工作与 M6：有正式 minimax 对与匹配算法。高于 TAFAS（AAAI 2025，机制论文无理论刻画）。低于 OneNet（NeurIPS 2023 之后该线的池内代表）在实证影响力维度——OneNet 给出大幅正增益，M7 给出的是"何时不该做"的刻画；但在理论深度上 M7 更强。与 PROCEED（KDD 2025，独立观察到 label-delay 引致漂移）互补，M7 的形式化更彻底。

---

## M8 — TAPR: Time-Adaptive Patch Routing for Forecasting Irregularly Sampled Multivariate Time Series

**Summary**。ODE-free、graph-free 的 IMTS 预测 transformer：每通道等观测数自适应 patch（K=clip(⌈n_max/P_target⌉,1,K_max)，§3.1）、可学习 Fourier 时间编码 + patch 元数据（§3.2）、时间距离/通道对加性注意力偏置（§3.3）、任意时间戳查询的两层 cross-attention 解码（§3.4）。在官方 t-PatchGNN 基准（PhysioNet/USHCN/Activity，5 seeds，配对 t 检验）上：USHCN（t=−9.6）与 Activity（t=−2.9）显著优于 t-PatchGNN，参数量仅其 28–39%；USHCN 上也优于同期 Hi-Patch 的引用数字（0.479 vs 0.494）；PhysioNet 落后 4.5%。附诚实的消融与负结果（decoder bias 有害、IMTS 上 instance norm 有害、多尺度金字塔失败 §4.2–4.3）以及规则网格退化研究（ETTh1 上加 IN 后仍落后专用模型 ~10%，§4.5）。

**领域定位与差异化**。属子领域3（IMTS）——池内 18 篇、竞争最激烈的赛道。差异化主张是"简单性/效率换持平精度"：相对 t-PatchGNN 去掉每 batch 图构建、相对 Hi-Patch/HyperIMTS 去掉层级/超图。主张本身被证据支持（USHCN 1.8× 训练加速、2.9× 省内存，§4.4），实验纪律好（同协议自跑基线、配对检验、引用数字与自复现互核，脚注1）。但"简单 transformer + 好的 tokenization 能打平图方法"是一个工程结论而非概念突破，且只在 3 个小数据集上成立、其中最大最难的 PhysioNet 还输。

**主要风险**。(1) 基准太窄：3 个数据集（其一 5 通道的 USHCN），MIMIC 缺席（§5 自认 credentialed access）——而池内 2025/2026 的 IMTS 论文（Hi-Patch/HyperIMTS/ASTGI）普遍报 4–5 个数据集含 MIMIC；顶会评审第一个问题就是这个。(2) 对最强并发方法不占优：Hi-Patch 在 PhysioNet/Activity 按其引用数字更强（§5 自认），HyperIMTS 不可比。三选二的胜利在 18 篇池内竞品的赛道里不足以立住"新 SOTA 或新范式"任何一头。(3) 方法各组件（等数 patch、Fourier 时间编码、距离偏置、cross-attention 查询解码）均系已有构件的合理组装，消融显示 encoder biases within-noise（§4.2），进一步削弱架构叙事。(4) 规则网格上明确不占优（§4.5），通用性天花板可见。

**打分**：novelty 5 / significance 4.5 / soundness 6.5 / clarity 7 → **overall 5.0，weak reject**（在 ICML 主赛道；若投效率导向 venue 或作为 short paper 更合适）。

**池内定位**：子领域3内约 **30–40 百分位**。低于 t-PatchGNN（ICML 2024）与 Hi-Patch（ICML 2025）：前者开创该基准线，后者在更多数据集上更强。低于 HyperIMTS（ICML 2025）在覆盖面上。高于池内部分偏散的应用型工作，且高于典型的"再加一个图变体"投稿——负结果报告与效率论证是真实的加分项，但不足以在 25% 录取率下越线。

---

## 横向观察（供作者与程序委员会参考）

1. **M5/M6/M7 是同一研究线的三次切分**，共享数据网格、基线与匿名互引（Anonymous 2026a/b）。单独看只有 M7 过线；M6 的层级 + M7 的理论合并为一篇，会是一篇明确 accept 的强论文。程序委员会若同时收到三稿应注意 salami-slicing 问题。
2. **M1/M2/M3/M4 构成一个"评测诚实性"集群**（泄漏、污染、校准、安慰剂），方向选择与 2024–2026 社区痛点高度吻合，引用潜力普遍高于其方法论文（M5–M8）。其中 M4 证据最完整、结论最锋利；M2 方向正确但主结果为 null，且需修复匿名违规（可复现性附录含非匿名仓库 URL）。
3. 多稿依赖 Holm 校正后不显著的诚实报告（M1 §4.4、M2 §6、M4 §4）。这是好的统计实践，但也意味着这些稿件的阳性主张需要更大的样本量支撑，作者应在 rebuttal 前补功效分析。

*报告完。评分与百分位均为单一评审人的校准估计，非委员会决定。*
