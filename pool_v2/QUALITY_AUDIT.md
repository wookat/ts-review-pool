# pool_v2 质量审查说明

## 抓取来源

- **arXiv export API**：2025-01 至今（2026-08），按 8 个子领域的多组布尔关键词检索，约 2,900 条去重记录。preprint 条目 tier 标注为 `preprint`；已被会议/期刊接收的（arXiv comment / journal_ref / Semantic Scholar venue 字段可证）按接收 venue 标注 CCF 档次。
- **Semantic Scholar Graph API**：补充 NeurIPS/ICML/ICLR/KDD/AAAI/IJCAI/WWW/CIKM 2025–2026 及 TPAMI/TKDE/JMLR/TMLR 等 venue 信息与 DOI，约 800 条。抓取中遇 429 限速，改用缩小 venue 列表 + 重试的备用脚本完成。
- **OpenReview API**：v1/v2 接口均返回 403，未能直接抓取；ICLR/NeurIPS 接收信息经 arXiv comment 与 Semantic Scholar venue 字段间接获得。
- **DBLP**：TLS 握手失败，未能使用。

## 处理流程

1. 标题归一化（去大小写与标点）合并多来源记录，去除 arXiv 多版本重复。
2. 与现有 `papers.csv`（103 行数据；README 描述约 101 篇）按归一化标题与 arXiv ID 双重去重，pool_v2 与旧池零重叠。
3. 摘要长度 < 200 字符或无法确认为时序相关的记录直接剔除。
4. 关键词规则 + 类目信息进行 8 子领域初分类，人工逐条复核时可调整。

## 人工审查

- **全量摘要复核**：全部候选（约 770 条）均由人工阅读标题与摘要后决定收录与否并撰写 one_line_summary；每条总结基于摘要事实撰写（解决什么问题 + 方法），未收录摘要中不存在的信息。
- **剔除约 230 条**弱相关候选，典型剔除原因：纯领域应用（天文、遥感、电网、金融交易系统等）而无时序方法学贡献；研讨会/教程/研究计划书/学位论文摘要；与子领域主题不符（如 sf5 中与数据污染无关的通用 LLM 工作）。
- 子领域 5（数据污染/记忆化/成员推断）中部分条目为通用 LLM/VLM 记忆化与成员推断研究而非时序专属，符合该子领域主题定位，予以保留。

## 已知局限

- 2026 年会议接收信息以 arXiv comment 与 Semantic Scholar 元数据为准，个别 venue 标注可能滞后于最终接收状态。
- OpenReview 与 DBLP 不可用导致部分仅在 OpenReview 上的接收论文可能未覆盖。
- preprint 占比约 56%，符合"覆盖 2025–2026 最新研究"的目标，但其中部分论文后续可能被会议接收。
