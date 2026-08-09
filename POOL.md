# 时序顶会论文对比池（2024–2026）

为独立盲评构建的对比池。仅收录客观信息（标题/作者/venue/链接/客观转述摘要），不含任何主观优劣评价。

**收录范围**：ICML / NeurIPS（含 D&B track）/ ICLR / KDD / AAAI / IJCAI / VLDB，2024–2026 年。venue 均经 OpenReview、Crossref（DOI）、arXiv、PMLR 等来源核实。

## 子领域覆盖统计

| 编号 | 子领域 | 篇数 |
|---|---|---|
| 1 | 非平稳/归一化/分布漂移 | 12 |
| 2 | 在线学习/测试时适应/延迟反馈 | 11 |
| 3 | 不规则采样/IMTS | 18 |
| 4 | 基准与评测方法学 | 11 |
| 5 | 数据污染/记忆化/成员推断 | 17 |
| 6 | 共形预测/不确定性量化 | 16 |
| 7 | 多模态/文本条件化 | 16 |
| — | **合计（池内）** | **101** |

另附 2 条池外参考条目（任务指定类型但非顶会主会收录，已明确标注，不计入总数）。

## 1. 非平稳/归一化/分布漂移（12 篇）

- **SIN: Selective and Interpretable Normalization for Long-Term Time Series Forecasting**  
  Lu Han; Han-Jia Ye; De-Chuan Zhan. *ICML 2024*. [OpenReview](https://openreview.net/forum?id=cUMOVfOIve)  
  提出按数据特性选择性执行统计量平移/缩放的可解释归一化框架 SIN，以缓解长时序预测中的非平稳问题。
- **Frequency Adaptive Normalization For Non-stationary Time Series Forecasting**  
  Weiwei Ye; Songgaojun Deng; Qiaosha Zou; Ning Gui. *NeurIPS 2024*. arXiv:[2409.20371](https://arxiv.org/abs/2409.20371) · DOI:[10.52202/079017-0985](https://doi.org/10.52202/079017-0985)  
  提出频域自适应归一化 FAN，用主频成分刻画并去除非平稳性，再以预测模块重建频率变化。
- **DDN: Dual-domain Dynamic Normalization for Non-stationary Time Series Forecasting**  
  Tao Dai; Beiliang Wu; Peiyuan Liu; Naiqi Li; Xue Yuerong; Shu-Tao Xia; Zexuan Zhu. *NeurIPS 2024*. DOI:[10.52202/079017-3444](https://doi.org/10.52202/079017-3444)  
  提出时域与频域双域动态归一化 DDN，以滑动方式细粒度刻画分布变化并可插拔于多种预测模型。
- **Considering Nonstationary within Multivariate Time Series with Variational Hierarchical Transformer for Forecasting**  
  Muyao Wang; Wenchao Chen; Bo Chen. *AAAI 2024*. arXiv:[2403.05406](https://arxiv.org/abs/2403.05406) · DOI:[10.1609/aaai.v38i14.29483](https://doi.org/10.1609/aaai.v38i14.29483)  
  提出 HTV-Trans，用变分层次概率模块与 Transformer 联合建模多元时序内在非平稳性。
- **U-Mixer: An Unet-Mixer Architecture with Stationarity Correction for Time Series Forecasting**  
  Xiang Ma; Xuemei Li; Lexin Fang; Tianlong Zhao; Caiming Zhang. *AAAI 2024*. arXiv:[2401.02236](https://arxiv.org/abs/2401.02236) · DOI:[10.1609/aaai.v38i13.29337](https://doi.org/10.1609/aaai.v38i13.29337)  
  提出 Unet-Mixer 结构并加入平稳性校正机制，通过约束校正前后数据分布差异处理非平稳性。
- **Deep Frequency Derivative Learning for Non-stationary Time Series Forecasting**  
  Wei Fan; Kun Yi; Hangting Ye; Zhiyuan Ning; Qi Zhang; Ning An. *IJCAI 2024*. arXiv:[2407.00502](https://arxiv.org/abs/2407.00502)  
  提出 DERITS，在频域求导表示上学习，以缓解时序的非平稳统计特性对预测的影响。
- **TimeBridge: Non-Stationarity Matters for Long-term Time Series Forecasting**  
  Peiyuan Liu; Beiliang Wu; Yifan Hu; Naiqi Li; Tao Dai; Jigang Bao; Shu-tao Xia. *ICML 2025*. arXiv:[2410.04442](https://arxiv.org/abs/2410.04442)  
  提出 TimeBridge，区分短期片段内非平稳与长期依赖建模，分别用积分/协整注意力桥接两者。
- **Non-stationary Diffusion For Probabilistic Time Series Forecasting**  
  Weiwei Ye; Zhuopeng Xu; Ning Gui. *ICML 2025*. arXiv:[2505.04278](https://arxiv.org/abs/2505.04278)  
  提出非平稳扩散模型 NsDiff，将时变不确定性显式纳入扩散过程以进行概率预测。
- **CycleNet: Enhancing Time Series Forecasting through Modeling Periodic Patterns**  
  Shengsheng Lin; Weiwei Lin; Xinyi Hu; Wentai Wu; Ruichao Mo; Haocheng Zhong. *NeurIPS 2024 (Spotlight)*. arXiv:[2409.18479](https://arxiv.org/abs/2409.18479) · DOI:[10.52202/079017-3373](https://doi.org/10.52202/079017-3373)  
  提出可学习循环周期分量的 RCF 技术（CycleNet），显式建模数据中的周期模式以增强预测。
- **IN-Flow: Instance Normalization Flow for Non-stationary Time Series Forecasting**  
  Wei Fan; Shun Zheng; Pengyang Wang; Rui Xie; Kun Yi; Qi Zhang; Jiang Bian; Yanjie Fu. *KDD 2025*. arXiv:[2401.16777](https://arxiv.org/abs/2401.16777) · DOI:[10.1145/3690624.3709260](https://doi.org/10.1145/3690624.3709260)  
  提出实例归一化流 IN-Flow，用可逆流网络学习去除分布偏移的变换代替简单统计归一化。
- **Rethinking the Power of Timestamps for Robust Time Series Forecasting: A Global-Local Fusion Perspective**  
  Chengsen Wang; Qi Qi; Jingyu Wang; Haifeng Sun; Zirui Zhuang; Jinming Wu; Jianxin Liao. *NeurIPS 2024*. arXiv:[2409.18696](https://arxiv.org/abs/2409.18696) · DOI:[10.52202/079017-0700](https://doi.org/10.52202/079017-0700)  
  提出 GLAFF 框架，利用时间戳的全局信息与局部观测自适应融合，以增强分布漂移下的稳健预测。
- **PULSE: Generative Phase Evolution for Non-Stationary Time Series Forecasting**  
  Yangyou Liu; Zezhi Shao; Xinyu Chen; Hu Chen; Fei Wang; Yuankai Wu. *ICML 2026*. arXiv:[2605.16793](https://arxiv.org/abs/2605.16793)  
  提出 PULSE，从相位演化视角对非平稳时序进行生成式建模与预测。

## 2. 在线学习/测试时适应/延迟反馈（11 篇）

- **Proactive Model Adaptation Against Concept Drift for Online Time Series Forecasting**  
  Lifan Zhao; Yanyan Shen. *KDD 2025*. arXiv:[2412.08435](https://arxiv.org/abs/2412.08435) · DOI:[10.1145/3690624.3709210](https://doi.org/10.1145/3690624.3709210)  
  提出 Proceed，先估计概念漂移再生成参数调整进行主动模型适应，用于在线时序预测。
- **Calibration of Time-Series Forecasting: Detecting and Adapting Context-Driven Distribution Shift**  
  Mouxiang Chen; Lefei Shen; Han Fu; Zhuo Li; Jianling Sun; Chenghao Liu. *KDD 2024*. arXiv:[2310.14838](https://arxiv.org/abs/2310.14838) · DOI:[10.1145/3637528.3671926](https://doi.org/10.1145/3637528.3671926)  
  提出 SOLID 框架，检测上下文驱动的分布偏移并在测试时用邻近样本校准预测模型。
- **Battling the Non-Stationarity in Time Series Forecasting via Test-time Adaptation**  
  HyunGi Kim; Siwon Kim; Jisoo Mok; Sungroh Yoon. *AAAI 2025*. arXiv:[2501.04970](https://arxiv.org/abs/2501.04970) · DOI:[10.1609/aaai.v39i17.33965](https://doi.org/10.1609/aaai.v39i17.33965)  
  提出 TAFAS，面向预测任务的测试时自适应框架，利用部分到达的真实值校准模型应对非平稳性。
- **Fast and Slow Streams for Online Time Series Forecasting Without Information Leakage**  
  Ying-yee Ava Lau; Zhiwen Shao; Dit-Yan Yeung. *ICLR 2025*. [OpenReview](https://openreview.net/forum?id=I0n3EyogMi)  
  重新定义在线预测设定以避免信息泄漏，提出快慢双流学习框架 DSOF 进行在线更新。
- **Online Time Series Forecasting with Theoretical Guarantees**  
  Zijian Li; Changze Zhou; Minghao Fu; Sanjay Manjunath; Fan Feng; Guangyi Chen; Yingyao Hu; Ruichu Cai; Kun Zhang. *NeurIPS 2025*. arXiv:[2510.18281](https://arxiv.org/abs/2510.18281) · DOI:[10.52202/085713-4480](https://doi.org/10.52202/085713-4480)  
  在在线时序预测设定下给出具理论保证的算法与遗憾界分析。
- **Tackling Time-Series Forecasting Generalization via Mitigating Concept Drift**  
  Zhiyuan Zhao; Haoxin Liu; B. Aditya Prakash. *ICLR 2026*. arXiv:[2510.14814](https://arxiv.org/abs/2510.14814)  
  从缓解概念漂移角度改进时序预测模型的泛化能力。
- **Test-time Adaptation in Non-stationary Environments via Adaptive Representation Alignment**  
  Zhen-Yu Zhang; Zhiyu Xie; Huaxiu Yao; Masashi Sugiyama. *NeurIPS 2024*. DOI:[10.52202/079017-2999](https://doi.org/10.52202/079017-2999)  
  提出 Ada-ReAlign，在非平稳环境下通过自适应表示对齐进行持续测试时适应。
- **When Model Meets New Normals: Test-Time Adaptation for Unsupervised Time-Series Anomaly Detection**  
  Dongmin Kim; Sunghyun Park; Jaegul Choo. *AAAI 2024*. arXiv:[2312.11976](https://arxiv.org/abs/2312.11976) · DOI:[10.1609/aaai.v38i12.29210](https://doi.org/10.1609/aaai.v38i12.29210)  
  研究时序异常检测中"新常态"问题，提出基于趋势估计与自监督的测试时适应方法 M2N2。
- **CANDI: Curated Test-Time Adaptation for Multivariate Time-Series Anomaly Detection Under Distribution Shift**  
  HyunGi Kim; Jisoo Mok; Hyungyu Lee; Juhyeon Shin; Sungroh Yoon. *AAAI 2026*. arXiv:[2604.01845](https://arxiv.org/abs/2604.01845) · DOI:[10.1609/aaai.v40i17.38524](https://doi.org/10.1609/aaai.v40i17.38524)  
  提出 CANDI，在分布偏移下对多元时序异常检测进行有筛选的测试时适应。
- **Augmented Contrastive Clustering with Uncertainty-Aware Prototyping for Time Series Test Time Adaptation**  
  Peiliang Gong; Mohamed Ragab; Min Wu; Zhenghua Chen; Yongyi Su; Xiaoli Li; Daoqiang Zhang. *KDD 2025*. arXiv:[2501.01472](https://arxiv.org/abs/2501.01472) · DOI:[10.1145/3690624.3709239](https://doi.org/10.1145/3690624.3709239)  
  提出 ACCUP，用不确定性感知原型与增强对比聚类实现时序测试时适应。
- **In-Context Time Series Predictor**  
  Jiecheng Lu; Yan Sun; Shihao Yang. *ICLR 2025*. arXiv:[2405.14982](https://arxiv.org/abs/2405.14982)  
  将时序预测重构为 (context, target) 令牌序列的上下文学习任务，无需参数更新即可适应新序列。

## 3. 不规则采样/IMTS（18 篇）

- **Irregular Multivariate Time Series Forecasting: A Transformable Patching Graph Neural Networks Approach**  
  Weijia Zhang; Chenlong Yin; Hao Liu; Xiaofang Zhou; Hui Xiong. *ICML 2024*. [OpenReview](https://openreview.net/forum?id=UZlMXUGI6e)  
  提出 t-PatchGNN，将不规则采样序列转为对齐的可变时长 patch 并用时间自适应图网络建模变量间依赖。
- **Hi-Patch: Hierarchical Patch GNN for Irregular Multivariate Time Series**  
  Yicheng Luo; Bowen Zhang; Zhen Liu; Qianli Ma. *ICML 2025*. [OpenReview](https://openreview.net/forum?id=nBgQ66iEUu)  
  提出 Hi-Patch，用层级 patch 图网络逐层聚合多尺度的不规则多元时序信息。
- **TimeCHEAT: A Channel Harmony Strategy for Irregularly Sampled Multivariate Time Series Analysis**  
  Jiexi Liu; Meng Cao; Songcan Chen. *AAAI 2025*. arXiv:[2412.12886](https://arxiv.org/abs/2412.12886) · DOI:[10.1609/aaai.v39i18.34076](https://doi.org/10.1609/aaai.v39i18.34076)  
  提出通道协调策略 TimeCHEAT，局部用图卷积嵌入、全局用 Transformer 学习不规则采样序列表示。
- **GraFITi: Graphs for Forecasting Irregularly Sampled Time Series**  
  Vijaya Krishna Yalavarthi; Kiran Madhusudhanan; Randolf Scholz; Nourhan Ahmed; Johannes Burchert; Shayan Jawed; Stefan Born; Lars Schmidt-Thieme. *AAAI 2024*. DOI:[10.1609/aaai.v38i15.29560](https://doi.org/10.1609/aaai.v38i15.29560)  
  将不规则采样带缺失的多元时序表示为稀疏二部图，将预测转化为边权补全问题。
- **Knowledge-Empowered Dynamic Graph Network for Irregularly Sampled Medical Time Series**  
  Yicheng Luo; Zhen Liu; Linghao Wang; Junhao Zheng; Binquan Wu; Qianli Ma. *NeurIPS 2024*. DOI:[10.52202/079017-2144](https://doi.org/10.52202/079017-2144)  
  提出 KEDGN，利用变量文本医学知识构建变量语义感知的动态图网络处理不规则采样医疗时序。
- **Amortized Control of Continuous State Space Feynman-Kac Model for Irregular Time Series**  
  Byoungwoo Park; Hyungi Lee; Juho Lee. *ICLR 2025 (Oral)*. arXiv:[2410.05602](https://arxiv.org/abs/2410.05602)  
  将不规则时序建模为连续状态空间 Feynman-Kac 模型并提出摊销控制的推断方法。
- **Trajectory Flow Matching with Applications to Clinical Time Series Modeling**  
  Xi Zhang; Yuan Pu; Yuki Kawamura; Andrew Loza; Yoshua Bengio; Dennis L. Shung; Alexander Tong. *NeurIPS 2024 (Spotlight)*. arXiv:[2410.21154](https://arxiv.org/abs/2410.21154) · DOI:[10.52202/079017-3404](https://doi.org/10.52202/079017-3404)  
  提出轨迹流匹配以无需仿真方式训练神经 SDE，应用于不规则临床时序建模。
- **Stable Neural Stochastic Differential Equations in Analyzing Irregular Time Series Data**  
  YongKyung Oh; Dong-Young Lim; Sungil Kim. *ICLR 2024 (Spotlight)*. arXiv:[2402.14989](https://arxiv.org/abs/2402.14989)  
  提出三类稳定的神经随机微分方程以稳健分析不规则采样时序数据。
- **IVP-VAE: Modeling EHR Time Series with Initial Value Problem Solvers**  
  Jingge Xiao; Leonie Basso; Wolfgang Nejdl; Niloy Ganguly; Sandipan Sikdar. *AAAI 2024*. arXiv:[2305.06741](https://arxiv.org/abs/2305.06741) · DOI:[10.1609/aaai.v38i14.29534](https://doi.org/10.1609/aaai.v38i14.29534)  
  提出 IVP-VAE，用可并行的初值问题求解器直接近似状态演化以高效建模 EHR 不规则时序。
- **Probabilistic Forecasting of Irregularly Sampled Time Series**  
  Vijaya Krishna Yalavarthi; Randolf Scholz; Stefan Born; Lars Schmidt-Thieme. *AAAI 2025*. DOI:[10.1609/aaai.v39i20.35494](https://doi.org/10.1609/aaai.v39i20.35494)  
  提出对带缺失的不规则采样时序进行条件归一化流概率预测的方法。
- **Adaptive Time Encoding for Irregular Multivariate Time-Series Classification**  
  Sangho Lee; Kyeongseo Min; Youngdoo Son; Hyungrok Do. *NeurIPS 2025*. DOI:[10.52202/085713-3407](https://doi.org/10.52202/085713-3407)  
  提出自适应时间编码用于不规则多元时序分类。
- **HyperIMTS: Hypergraph Neural Network for Irregular Multivariate Time Series Forecasting**  
  Boyuan Li; Yicheng Luo; Zhen Liu; Junhao Zheng; Jianming Lv; Qianli Ma. *ICML 2025*. arXiv:[2505.17431](https://arxiv.org/abs/2505.17431)  
  提出 HyperIMTS，用超图统一建模不规则观测点的时间与变量间交互进行预测。
- **Learning Recursive Multi-Scale Representations for Irregular Multivariate Time Series Forecasting**  
  Boyuan Li; Zhen Liu; Yicheng Luo; Qianli Ma. *ICLR 2026*. arXiv:[2602.21498](https://arxiv.org/abs/2602.21498)  
  为不规则多元时序预测学习递归多尺度表示。
- **ASTGI: Adaptive Spatio-Temporal Graph Interactions for Irregular Multivariate Time Series Forecasting**  
  Xvyuan Liu; Xiangfei Qiu; Hanyin Cheng; Xingjian Wu; Chenjuan Guo; Bin Yang; Jilin Hu. *ICLR 2026*. arXiv:[2509.23313](https://arxiv.org/abs/2509.23313)  
  提出自适应时空图交互网络 ASTGI 处理不规则多元时序预测。
- **Bridging Time and Frequency: A Joint Modeling Framework for Irregular Multivariate Time Series Forecasting**  
  Xiangfei Qiu; Kangjia Yan; Xvyuan Liu; Xingjian Wu; Jilin Hu. *ICML 2026*. arXiv:[2602.00582](https://arxiv.org/abs/2602.00582)  
  提出时域-频域联合建模框架处理不规则多元时序预测。
- **LAST SToP for Modeling Asynchronous Time Series**  
  Shubham Gupta; Thibaut Durand; Graham Taylor; Lilian W. Białokozowicz. *ICML 2025*. arXiv:[2502.01922](https://arxiv.org/abs/2502.01922)  
  提出面向异步事件时序的 LLM 提示式建模框架 LASTS 与可学习软提示调优。
- **Beyond Observations: Reconstruction Error-Guided Irregularly Sampled Time Series Representation Learning**  
  Jiexi Liu; Meng Cao; Songcan Chen. *AAAI 2026*. arXiv:[2511.06854](https://arxiv.org/abs/2511.06854) · DOI:[10.1609/aaai.v40i28.39545](https://doi.org/10.1609/aaai.v40i28.39545)  
  以重构误差为引导学习不规则采样时序表示。
- **Interacting Diffusion Processes for Event Sequence Forecasting**  
  Mai Zeng; Florence Regol; Mark Coates. *ICML 2024*. arXiv:[2310.17800](https://arxiv.org/abs/2310.17800)  
  提出交互扩散过程生成模型，对标记时间点过程的事件序列进行长程预测。

## 4. 基准与评测方法学（11 篇）

- **TFB: Towards Comprehensive and Fair Benchmarking of Time Series Forecasting Methods**  
  Xiangfei Qiu; Jilin Hu; Lekui Zhou; Xingjian Wu; Junyang Du; Buang Zhang; Chenjuan Guo; Aoying Zhou; Christian S. Jensen; Zhenli Sheng; Bin Yang. *VLDB 2024*. arXiv:[2403.20150](https://arxiv.org/abs/2403.20150) · DOI:[10.14778/3665844.3665863](https://doi.org/10.14778/3665844.3665863)  
  提出覆盖多领域数据与多类方法的时序预测自动化基准 TFB，统一评测流程以消除偏置。
- **The Elephant in the Room: Towards A Reliable Time-Series Anomaly Detection Benchmark**  
  Qinghua Liu; John Paparrizos. *NeurIPS 2024 D&B*. DOI:[10.52202/079017-3437](https://doi.org/10.52202/079017-3437)  
  构建 TSB-AD 基准与可靠评测协议，系统评估时序异常检测方法。
- **ProbTS: Benchmarking Point and Distributional Forecasting across Diverse Prediction Horizons**  
  Jiawen Zhang; Xumeng Wen; Zhenwei Zhang; Shun Zheng; Jia Li; Jiang Bian. *NeurIPS 2024 D&B*. arXiv:[2310.07446](https://arxiv.org/abs/2310.07446) · DOI:[10.52202/079017-1523](https://doi.org/10.52202/079017-1523)  
  构建 ProbTS 基准，统一评测点预测与分布预测在不同预测长度下的表现。
- **TAB: Unified Benchmarking of Time Series Anomaly Detection Methods**  
  Xiangfei Qiu; Zhe Li; Wanghui Qiu; Shiyan Hu; Lekui Zhou; Xingjian Wu; Zhengyu Li; Chenjuan Guo; Aoying Zhou; Zhenli Sheng; Jilin Hu; Christian S. Jensen; Bin Yang. *VLDB 2025*. arXiv:[2506.18046](https://arxiv.org/abs/2506.18046) · DOI:[10.14778/3746405.3746407](https://doi.org/10.14778/3746405.3746407)  
  提出统一的时序异常检测基准 TAB，覆盖多类数据集与方法的标准化评测。
- **Are Language Models Actually Useful for Time Series Forecasting?**  
  Mingtian Tan; Mike A. Merrill; Vinayak Gupta; Tim Althoff; Thomas Hartvigsen. *NeurIPS 2024 (Spotlight)*. arXiv:[2406.16964](https://arxiv.org/abs/2406.16964) · DOI:[10.52202/079017-1922](https://doi.org/10.52202/079017-1922)  
  通过消融实验发现移除 LLM 组件或替换为注意力层在多数时序预测基准上不降低性能。
- **An Analysis of Linear Time Series Forecasting Models**  
  William Toner; Luke Nicholas Darlow. *ICML 2024*. arXiv:[2403.14587](https://arxiv.org/abs/2403.14587)  
  证明多种流行线性时序预测模型与标准线性回归在功能上等价且后者往往表现更优。
- **A Careful Examination of Large Language Model Performance on Grade School Arithmetic**  
  Hugh Zhang; Jeff Da; Dean Lee; Vaughn Robinson; Catherine Wu; Will Song; Tiffany Zhao; Pranav Raja; Charlotte Zhuang; Dylan Slack; Qin Lyu; Sean Hendryx; Russell Kaplan; Michele Lunati; Summer Yue. *NeurIPS 2024 D&B (Spotlight)*. arXiv:[2405.00332](https://arxiv.org/abs/2405.00332) · DOI:[10.52202/079017-1485](https://doi.org/10.52202/079017-1485)  
  构建 GSM1k 以测量 GSM8k 上的基准污染，发现部分模型存在系统性过拟合证据。
- **LiveBench: A Challenging, Contamination-Limited LLM Benchmark**  
  Colin White; Samuel Dooley; Manley Roberts; Arka Pal; Benjamin Feuer; Siddhartha Jain; Ravid Shwartz-Ziv; Neel Jain; Khalid Saifullah; Sreemanti Dey; Shubh-Agrawal; Sandeep Singh Sandha; Siddartha Venkat Naidu; Chinmay Hegde; Yann LeCun; Tom Goldstein; Willie Neiswanger; Micah Goldblum. *ICLR 2025 (Spotlight)*. arXiv:[2406.19314](https://arxiv.org/abs/2406.19314)  
  提出基于持续更新问题与客观自动评分的 LLM 基准 LiveBench，以限制测试集污染。
- **This Time is Different: An Observability Perspective on Time Series Foundation Models**  
  Ben Cohen; Emaad Khwaja; Youssef Doubli; Salahidine Lemaachi; Chris Lettieri; Charles Masson; Hugo Miccinilli; Elise Ramé; Qiqi Ren; Afshin Rostamizadeh; Jean Ogier du Terrail; Anna-Monica Toon; Kan Wang; Stephan Xie; Zongzhe Xu; Viktoriya Zhukova; David Asker; Ameet Talwalkar; Othmane Abou-Amal. *NeurIPS 2025*. arXiv:[2505.14766](https://arxiv.org/abs/2505.14766) · DOI:[10.52202/085713-1698](https://doi.org/10.52202/085713-1698)  
  发布 Toto 模型与 BOOM 可观测性指标大规模基准，用于评测时序基础模型。
- **TimeSeriesExamAgent: Creating Time Series Reasoning Benchmarks at Scale**  
  Malgorzata Gwiazda; Yifu Cai; Mononito Goswami; Arjun Choudhry; Artur Dubrawski. *ICLR 2026*. arXiv:[2604.10291](https://arxiv.org/abs/2604.10291)  
  提出可规模化自动生成时序推理评测题目的代理框架。
- **Zero-shot forecasting of chaotic systems**  
  Yuanzhao Zhang; William Gilpin. *ICLR 2025*. arXiv:[2409.15771](https://arxiv.org/abs/2409.15771)  
  在 135 个混沌动力系统上系统评测时序基础模型的零样本预测能力。

## 5. 数据污染/记忆化/成员推断（17 篇）

- **Detecting Pretraining Data from Large Language Models**  
  Weijia Shi; Anirudh Ajith; Mengzhou Xia; Yangsibo Huang; Daogao Liu; Terra Blevins; Danqi Chen; Luke Zettlemoyer. *ICLR 2024*. arXiv:[2310.16789](https://arxiv.org/abs/2310.16789)  
  提出 WikiMIA 基准与 Min-K% Prob 方法，在无需训练数据分布知识下检测文本是否在预训练数据中。
- **Min-K%++: Improved Baseline for Pre-Training Data Detection from Large Language Models**  
  Jingyang Zhang; Jingwei Sun; Eric Yeats; Yang Ouyang; Martin Kuo; Jianyi Zhang; Hao Frank Yang; Hai Li. *ICLR 2025 (Spotlight)*. arXiv:[2404.02936](https://arxiv.org/abs/2404.02936)  
  提出 Min-K%++，基于局部极大值判别改进预训练数据检测基线。
- **Infilling Score: A Pretraining Data Detection Algorithm for Large Language Models**  
  Negin Raoof; Litu Rout; Giannis Daras; Sujay Sanghavi; Constantine Caramanis; Sanjay Shakkottai; Alex Dimakis. *ICLR 2025*. [OpenReview](https://openreview.net/forum?id=9QPH1YQCMn)  
  提出基于非因果填充似然的 Infilling Score 用于预训练数据检测。
- **LLM Dataset Inference: Did you train on my dataset?**  
  Pratyush Maini; Hengrui Jia; Nicolas Papernot; Adam Dziedzic. *NeurIPS 2024*. arXiv:[2406.06443](https://arxiv.org/abs/2406.06443) · DOI:[10.52202/079017-3941](https://doi.org/10.52202/079017-3941)  
  提出数据集级推断方法，聚合多种成员推断特征以统计检验某数据集是否被用于训练。
- **Rethinking LLM Memorization through the Lens of Adversarial Compression**  
  Avi Schwarzschild; Zhili Feng; Pratyush Maini; Zachary C. Lipton; J. Zico Kolter. *NeurIPS 2024*. arXiv:[2404.15146](https://arxiv.org/abs/2404.15146) · DOI:[10.52202/079017-1790](https://doi.org/10.52202/079017-1790)  
  提出对抗压缩比 ACR 作为记忆化度量：能被更短对抗提示诱出的训练串视为被记忆。
- **Task Contamination: Language Models May Not Be Few-Shot Anymore**  
  Changmao Li; Jeffrey Flanigan. *AAAI 2024*. arXiv:[2312.16337](https://arxiv.org/abs/2312.16337) · DOI:[10.1609/aaai.v38i16.29808](https://doi.org/10.1609/aaai.v38i16.29808)  
  系统度量任务污染：LLM 在训练截止日期前发布的数据集上零/少样本表现显著优于其后发布者。
- **Membership Inference Attacks against Fine-tuned Large Language Models via Self-prompt Calibration**  
  Wenjie Fu; Huandong Wang; Chen Gao; Guanghua Liu; Yong Li; Tao Jiang. *NeurIPS 2024*. arXiv:[2311.06062](https://arxiv.org/abs/2311.06062) · DOI:[10.52202/079017-4290](https://doi.org/10.52202/079017-4290)  
  提出 SPV-MIA，用自提示校准的概率波动信号对微调 LLM 进行成员推断。
- **ConStat: Performance-Based Contamination Detection in Large Language Models**  
  Jasper Dekoninck; Mark Niklas Müller; Martin Vechev. *NeurIPS 2024*. arXiv:[2405.16281](https://arxiv.org/abs/2405.16281) · DOI:[10.52202/079017-2935](https://doi.org/10.52202/079017-2935)  
  提出 ConStat，将污染定义为不可泛化的基准性能虚高并给出统计检测方法。
- **Recite, Reconstruct, Recollect: Memorization in LMs as a Multifaceted Phenomenon**  
  USVSN Sai Prashanth; Alvin Deng; Kyle O'Brien; Jyothir S; Mohammad Aflah Khan; Jaydeep Borkar; Christopher A. Choquette-Choo; Jacob Ray Fuehne; Stella Biderman; Tracy Ke; Katherine Lee; Naomi Saphra. *ICLR 2025*. arXiv:[2406.17746](https://arxiv.org/abs/2406.17746)  
  将 LM 记忆化分类为背诵/重构/回忆三类，并分析各因素对记忆化的影响。
- **Proving Test Set Contamination in Black-Box Language Models**  
  Yonatan Oren; Nicole Meister; Niladri Chatterji; Faisal Ladhak; Tatsunori B. Hashimoto. *ICLR 2024 (Oral)*. arXiv:[2310.17623](https://arxiv.org/abs/2310.17623)  
  利用可交换性检验在黑盒访问下对测试集污染给出可证明的统计证据。
- **Rethinking Pretraining Data Detection for LLMs: From Local to Global**  
  Chenye Ke; Yan Zhuang; Zirui Liu; Qi Liu. *ICML 2026*. [OpenReview](https://openreview.net/forum?id=ThlRUXqFZ0)  
  从局部到全局重新思考 LLM 预训练数据检测的建模方式。
- **Fine-tuning can Help Detect Pretraining Data from Large Language Models**  
  Hengxiang Zhang; Songxin Zhang; Bingyi Jing; Hongxin Wei. *ICLR 2025*. arXiv:[2410.10880](https://arxiv.org/abs/2410.10880)  
  发现用少量未见数据微调可放大成员与非成员的困惑度偏差，提出 FSD 检测方法。
- **Unlocking Post-hoc Dataset Inference with Synthetic Data**  
  Bihe Zhao; Pratyush Maini; Franziska Boenisch; Adam Dziedzic. *ICML 2025*. arXiv:[2506.15271](https://arxiv.org/abs/2506.15271)  
  用合成数据构造验证集，解锁无需持出集的事后数据集推断。
- **STAMP Your Content: Proving Dataset Membership via Watermarked Rephrasings**  
  Saksham Rastogi; Pratyush Maini; Danish Pruthi. *ICML 2025*. arXiv:[2504.13416](https://arxiv.org/abs/2504.13416)  
  通过水印改写公开内容以证明数据集成员身份。
- **Ward: Provable RAG Dataset Inference via LLM Watermarks**  
  Nikola Jovanović; Robin Staab; Maximilian Baader; Martin Vechev. *ICLR 2025*. arXiv:[2410.03537](https://arxiv.org/abs/2410.03537)  
  用 LLM 水印实现对 RAG 语料使用的可证明数据集推断。
- **An Investigation of Memorization Risk in Healthcare Foundation Models**  
  Sana Tonekaboni; Lena Stempfle; Adibvafa Fallahpour; Walter Gerych; Marzyeh Ghassemi. *NeurIPS 2025*. arXiv:[2510.12950](https://arxiv.org/abs/2510.12950) · DOI:[10.52202/085713-0353](https://doi.org/10.52202/085713-0353)  
  系统评估医疗基础模型（含时序 EHR）中的训练数据记忆化风险。
- **Membership Inference Attacks Against Fine-tuned Diffusion Language Models**  
  Yuetian Chen; Kaiyuan Zhang; Yuntao Du; Edoardo Stoppa; Charles Fleming; Ashish Kundu; Bruno Ribeiro; Ninghui Li. *ICLR 2026*. arXiv:[2601.20125](https://arxiv.org/abs/2601.20125)  
  针对微调扩散语言模型提出成员推断攻击并评估其隐私风险。

## 6. 共形预测/不确定性量化（16 篇）

- **Adaptive Conformal Inference by Betting**  
  Aleksandr Podkopaev; Darren Xu; Kuang-Chih Lee. *ICML 2024*. arXiv:[2412.19318](https://arxiv.org/abs/2412.19318)  
  将自适应共形推断视为在线博弈/下注问题，提出无需学习率的参数自由方法。
- **Online conformal prediction with decaying step sizes**  
  Anastasios N. Angelopoulos; Rina Foygel Barber; Stephen Bates. *ICML 2024*. arXiv:[2402.01139](https://arxiv.org/abs/2402.01139)  
  提出步长衰减的在线共形预测，可在分布收敛时同时获得覆盖保证与区间收敛。
- **Conformal prediction for multi-dimensional time series by ellipsoidal sets**  
  Chen Xu; Hanyang Jiang; Yao Xie. *ICML 2024 (Spotlight)*. arXiv:[2403.03850](https://arxiv.org/abs/2403.03850)  
  提出 MultiDimSPCI，为多维时序构造椭球形预测集并给出覆盖性分析。
- **Kernel-based Optimally Weighted Conformal Time-Series Prediction**  
  Jonghyeok Lee; Chen Xu; Yao Xie. *ICLR 2025*. [OpenReview](https://openreview.net/forum?id=oP7arLOWix)  
  提出核加权最优的共形时序预测区间方法 KOWCPI。
- **Copula Conformal Prediction for Multi-Step Time Series Forecasting**  
  Sophia Sun; Rose Yu. *ICLR 2024*. arXiv:[2212.03281](https://arxiv.org/abs/2212.03281)  
  提出 CopulaCPTS，用 copula 建模多步预测误差相关性以构造联合有效的预测区域。
- **Conformal Prediction Regions for Time Series using Linear Complementarity Programming**  
  Matthew Cleaveland; Insup Lee; George J. Pappas; Lars Lindemann. *AAAI 2024*. arXiv:[2304.01075](https://arxiv.org/abs/2304.01075) · DOI:[10.1609/aaai.v38i19.30089](https://doi.org/10.1609/aaai.v38i19.30089)  
  用线性互补规划优化多步时序共形预测区域以降低保守性。
- **Error-quantified Conformal Inference for Time Series**  
  Junxi Wu; Dongjian Hu; Yajie Bao; Shu-Tao Xia; Changliang Zou. *ICLR 2025*. arXiv:[2502.00818](https://arxiv.org/abs/2502.00818)  
  提出误差量化的共形推断，用平滑误差反馈改进时序在线覆盖与区间效率。
- **Conformalized Time Series with Semantic Features**  
  Baiting Chen; Zhimei Ren; Lu Cheng. *NeurIPS 2024*. DOI:[10.52202/079017-3859](https://doi.org/10.52202/079017-3859)  
  在潜在语义特征空间中加权非交换共形预测，为时序构造更高效的预测区间。
- **Relational Conformal Prediction for Correlated Time Series**  
  Andrea Cini; Alexander Jenkins; Danilo Mandic; Cesare Alippi; Filippo Maria Bianchi. *ICML 2025*. arXiv:[2502.09443](https://arxiv.org/abs/2502.09443)  
  利用相关时序间的关系结构（图）进行共形预测以改进不确定性量化。
- **Conformal Prediction for Time-series Forecasting with Change Points**  
  Sophia Sun; Rose Yu. *NeurIPS 2025*. arXiv:[2509.02844](https://arxiv.org/abs/2509.02844) · DOI:[10.52202/085713-0410](https://doi.org/10.52202/085713-0410)  
  针对含变点的时序预测设计共形预测方法并保持覆盖保证。
- **Flow-based Conformal Prediction for Multi-dimensional Time Series**  
  Junghwan Lee; Chen Xu; Yao Xie. *ICLR 2026*. arXiv:[2502.05709](https://arxiv.org/abs/2502.05709)  
  用流模型变换构造多维时序的共形预测集。
- **ResCP: Reservoir Conformal Prediction for Time Series Forecasting**  
  Roberto Neglia; Andrea Cini; Michael M. Bronstein; Filippo Maria Bianchi. *ICLR 2026*. arXiv:[2510.05060](https://arxiv.org/abs/2510.05060)  
  提出无需训练的水库计算共形预测 ResCP，用回声状态网络相似度加权非一致性分数。
- **Conditional Quantile Adjusted Conformal Prediction for Time Series**  
  Cheng Yu; Zhoufan Zhu; Ke Zhu. *ICML 2026*. [OpenReview](https://openreview.net/forum?id=I7qW7mXhRF)  
  提出条件分位数调整的时序共形预测方法。
- **Exploring the Noise Robustness of Online Conformal Prediction**  
  Huajun Xi; Kangdao Liu; Hao Zeng; Wenguang Sun; Hongxin Wei. *NeurIPS 2025*. arXiv:[2501.18363](https://arxiv.org/abs/2501.18363) · DOI:[10.52202/085713-1546](https://doi.org/10.52202/085713-1546)  
  研究在线共形预测在标签噪声下的稳健性并提出修正方法。
- **Distribution-informed Online Conformal Prediction**  
  Dongjian Hu; Junxi Wu; Shu-Tao Xia; Changliang Zou. *ICLR 2026*. arXiv:[2512.07770](https://arxiv.org/abs/2512.07770)  
  将分布信息引入在线共形预测以改进覆盖与效率。
- **Online Differentially Private Conformal Prediction for Uncertainty Quantification**  
  Qiangqiang Zhang; Ting Li; Xinwei Feng; Xiaodong Yan; Jinhan Xie. *ICML 2025*. [OpenReview](https://openreview.net/forum?id=dmZQrojdVU)  
  提出在线差分隐私共形预测框架进行不确定性量化。

## 7. 多模态/文本条件化（16 篇）

- **Time-MMD: Multi-Domain Multimodal Dataset for Time Series Analysis**  
  Haoxin Liu; Shangqing Xu; Zhiyuan Zhao; Lingkai Kong; Harshavardhan Kamarthi; Aditya B. Sasanur; Megha Sharma; Jiaming Cui; Qingsong Wen; Chao Zhang; B. Aditya Prakash. *NeurIPS 2024 D&B*. arXiv:[2406.08627](https://arxiv.org/abs/2406.08627) · DOI:[10.52202/079017-2476](https://doi.org/10.52202/079017-2476)  
  发布覆盖 9 个领域的数值-文本配对多模态时序数据集 Time-MMD 及评测库 MM-TSFlib。
- **TimeCAP: Learning to Contextualize, Augment, and Predict Time Series Events with Large Language Model Agents**  
  Geon Lee; Wenchao Yu; Kijung Shin; Wei Cheng; Haifeng Chen. *AAAI 2025*. arXiv:[2502.11418](https://arxiv.org/abs/2502.11418) · DOI:[10.1609/aaai.v39i17.33989](https://doi.org/10.1609/aaai.v39i17.33989)  
  提出 TimeCAP，用 LLM 代理生成上下文摘要并增强时序事件预测。
- **Context is Key: A Benchmark for Forecasting with Essential Textual Information**  
  Andrew Robert Williams; Arjun Ashok; Étienne Marcotte; Valentina Zantedeschi; Jithendaraa Subramanian; Roland Riachi; James Requeima; Alexandre Lacoste; Irina Rish; Nicolas Chapados; Alexandre Drouin. *ICML 2025*. arXiv:[2410.18959](https://arxiv.org/abs/2410.18959)  
  提出 CiK 基准：预测须结合数值历史与关键文本上下文，并给出 LLM 评测与区域缩放 CRPS 指标。
- **From News to Forecast: Integrating Event Analysis in LLM-Based Time Series Forecasting with Reflection**  
  Xinlei Wang; Maike Feng; Jing Qiu; Jinjin Gu; Junhua Zhao. *NeurIPS 2024*. arXiv:[2409.17515](https://arxiv.org/abs/2409.17515) · DOI:[10.52202/079017-1853](https://doi.org/10.52202/079017-1853)  
  用 LLM 代理筛选新闻事件并经反思机制融入时序预测。
- **ChatTime: A Unified Multimodal Time Series Foundation Model Bridging Numerical and Textual Data**  
  Chengsen Wang; Qi Qi; Jingyu Wang; Haifeng Sun; Zirui Zhuang; Jinming Wu; Lei Zhang; Jianxin Liao. *AAAI 2025*. arXiv:[2412.11376](https://arxiv.org/abs/2412.11376) · DOI:[10.1609/aaai.v39i12.33384](https://doi.org/10.1609/aaai.v39i12.33384)  
  将时序建模为外语，构建统一处理数值与文本输入输出的多模态时序基础模型 ChatTime。
- **Time-VLM: Exploring Multimodal Vision-Language Models for Augmented Time Series Forecasting**  
  Siru Zhong; Weilin Ruan; Ming Jin; Huan Li; Qingsong Wen; Yuxuan Liang. *ICML 2025*. arXiv:[2502.04395](https://arxiv.org/abs/2502.04395)  
  提出 Time-VLM，利用视觉-语言模型融合时序图像化与文本上下文以增强预测。
- **ChatTS: Aligning Time Series with LLMs via Synthetic Data for Enhanced Understanding and Reasoning**  
  Zhe Xie; Zeyan Li; Xiao He; Longlong Xu; Xidao Wen; Tieying Zhang; Jianjun Chen; Rui Shi; Dan Pei. *VLDB 2025*. arXiv:[2412.03104](https://arxiv.org/abs/2412.03104) · DOI:[10.14778/3742728.3742735](https://doi.org/10.14778/3742728.3742735)  
  用属性驱动合成数据训练原生支持多元时序输入的对话模型 ChatTS。
- **CALF: Aligning LLMs for Time Series Forecasting via Cross-modal Fine-Tuning**  
  Peiyuan Liu; Hang Guo; Tao Dai; Naiqi Li; Jigang Bao; Xudong Ren; Yong Jiang; Shu-Tao Xia. *AAAI 2025*. arXiv:[2403.07300](https://arxiv.org/abs/2403.07300) · DOI:[10.1609/aaai.v39i18.34082](https://doi.org/10.1609/aaai.v39i18.34082)  
  提出 CALF，通过跨模态匹配与蒸馏对齐文本与时序分支微调 LLM 预测器。
- **Time-LLM: Time Series Forecasting by Reprogramming Large Language Models**  
  Ming Jin; Shiyu Wang; Lintao Ma; Zhixuan Chu; James Y. Zhang; Xiaoming Shi; Pin-Yu Chen; Yuxuan Liang; Yuan-Fang Li; Shirui Pan; Qingsong Wen. *ICLR 2024*. arXiv:[2310.01728](https://arxiv.org/abs/2310.01728)  
  提出 Time-LLM，通过输入重编程与 Prompt-as-Prefix 冻结 LLM 进行时序预测。
- **S2IP-LLM: Semantic Space Informed Prompt Learning with LLM for Time Series Forecasting**  
  Zijie Pan; Yushan Jiang; Sahil Garg; Anderson Schneider; Yuriy Nevmyvaka; Dongjin Song. *ICML 2024*. arXiv:[2403.05798](https://arxiv.org/abs/2403.05798)  
  提出 S2IP-LLM，将时序嵌入与预训练语义空间对齐并用语义锚点提示进行预测。
- **TEMPO: Prompt-based Generative Pre-trained Transformer for Time Series Forecasting**  
  Defu Cao; Furong Jia; Sercan O Arik; Tomas Pfister; Yixiang Zheng; Wen Ye; Yan Liu. *ICLR 2024*. arXiv:[2310.04948](https://arxiv.org/abs/2310.04948)  
  提出 TEMPO，结合趋势-季节-残差分解与提示设计的生成式预训练时序 Transformer。
- **AutoTimes: Autoregressive Time Series Forecasters via Large Language Models**  
  Yong Liu; Guo Qin; Xiangdong Huang; Jianmin Wang; Mingsheng Long. *NeurIPS 2024*. arXiv:[2402.02370](https://arxiv.org/abs/2402.02370) · DOI:[10.52202/079017-3882](https://doi.org/10.52202/079017-3882)  
  将 LLM 重用为自回归时序预测器，用时间戳作为位置文本对齐多模态时间信息。
- **T2S: High-resolution Time Series Generation with Text-to-Series Diffusion Models**  
  Yunfeng Ge; Jiawei Li; Yiji Zhao; Haomin Wen; Zhao Li; Meikang Qiu; Hongyan Li; Ming Jin; Shirui Pan. *IJCAI 2025*. arXiv:[2505.02417](https://arxiv.org/abs/2505.02417)  
  提出 T2S 扩散模型，实现领域无关的文本到时序生成。
- **Time-IMM: A Dataset and Benchmark for Irregular Multimodal Multivariate Time Series**  
  Ching Chang; Jeehyun Hwang; Yidan Shi; Haixin Wang; Wen-Chih Peng; Tien-Fu Chen; Wei Wang. *NeurIPS 2025 D&B*. arXiv:[2506.10412](https://arxiv.org/abs/2506.10412) · DOI:[10.52202/085713-1533](https://doi.org/10.52202/085713-1533)  
  发布不规则多模态多元时序数据集与基准 Time-IMM。
- **MAESTRO: Adaptive Sparse Attention and Robust Learning for Multimodal Dynamic Time Series**  
  Payal Mohapatra; Yueyuan Sui; Akash Pandey; Stephen Xia; Qi Zhu. *NeurIPS 2025 (Spotlight)*. arXiv:[2509.25278](https://arxiv.org/abs/2509.25278) · DOI:[10.52202/085713-2694](https://doi.org/10.52202/085713-2694)  
  提出 MAESTRO，用自适应稀疏注意力与稳健学习建模多模态动态时序。
- **TimeOmni-1: Incentivizing Complex Reasoning with Time Series in Large Language Models**  
  Tong Guan; Zijie Meng; Dianqi Li; Shiyu Wang; Chao-Han Huck Yang; Qingsong Wen; Zuozhu Liu; Sabato Marco Siniscalchi; Ming Jin; Shirui Pan. *ICLR 2026*. arXiv:[2509.24803](https://arxiv.org/abs/2509.24803)  
  提出 TimeOmni-1，激励 LLM 对时序进行复杂推理的训练框架。

## 附录：池外参考条目（不计入统计）

- **GIFT-Eval: A Benchmark for General Time Series Forecasting Model Evaluation** — NeurIPS 2024 TSALM Workshop / arXiv（非主会，池外参考）. arXiv:[2410.10393](https://arxiv.org/abs/2410.10393)。提出覆盖 24 个数据集、多领域多频率的通用时序预测基准 GIFT-Eval。
- **fev-bench: A Realistic Benchmark for Time Series Forecasting** — arXiv 2025（未见顶会收录，池外参考）. arXiv:[2509.26468](https://arxiv.org/abs/2509.26468)。提出含协变量的 100 个任务的预测基准 fev-bench 及配套评测库 fev。
