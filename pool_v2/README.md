# pool_v2：2025–2026 时间序列研究扩展池

在原有 `papers.csv` 对比池（约 101 篇）基础上新增 **544 篇** 2025–2026 最新时间序列研究文献。原有条目未做任何改动，本目录为纯增量。

## 文件

- `papers_v2.csv` / `papers_v2.jsonl`：新增条目（两种格式内容一致）
- `STATS.md`：子领域 / venue 档次 / 年份分布统计
- `QUALITY_AUDIT.md`：抓取来源、去重、人工审查与已知局限说明

## 字段

| 字段 | 说明 |
|---|---|
| id | V2-001 起的顺序编号 |
| subfield | 子领域编号 1–8（见下） |
| subfield_name | 子领域中文名 |
| title | 论文标题 |
| first_author | 第一作者 |
| venue | 发表 venue（预印本为 arXiv） |
| year | 年份 |
| tier | CCF-A / CCF-B / CCF-C / Journal(TMLR) / preprint |
| arxiv_id / doi | 标识符（至少其一非空） |
| url | 论文链接 |
| one_line_summary | 基于摘要撰写的一句话总结（解决什么问题 + 方法） |

## 子领域编号

1. 非平稳/分布漂移/归一化/test-time adaptation 时序预测
2. 在线学习/延迟反馈/在线共形预测/时序不确定性量化
3. 不规则采样时序（IMTS）/医疗时序架构
4. 时序基础模型（TSFM）：预训练、零样本、评测
5. 数据污染/记忆化/成员推断/基准泄漏
6. 多模态（文本+时序）预测
7. 时序基准与评测方法学
8. 通用时序架构创新（Transformer/线性/SSM/Mamba）
