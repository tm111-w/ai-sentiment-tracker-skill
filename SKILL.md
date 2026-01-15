---
name: ai-sentiment-tracker
description: "AI产品舆情抓取与分析工具。用于从X(Twitter)和Reddit抓取特定AI产品/模型的用户反馈、使用案例和专家评价。使用场景：(1) 抓取特定AI产品的用户good cases和demo展示 (2) 收集AI领域KOL和学者(如Andrej Karpathy)的评价 (3) 分析新AI产品发布后的舆情反应 (4) 生成舆情分析报告和Excel数据表。触发词：舆情分析、sentiment analysis、用户反馈、good case、AI产品评价、社交媒体抓取"
---

# AI舆情抓取与分析 Skill

## 工作流程

### Step 1: 确认抓取参数
从用户获取以下信息：
- **目标产品/模型名称**：如 Claude Cowork, Gemini 3, GPT-5 等
- **时间范围**：默认最近7天，可自定义
- **重点关注的KOL**：默认包含 references/kol-list.md 中的AI学者

### Step 2: 执行抓取（目标50-100条数据）

**重要：需执行15-20次搜索以确保数据量充足**

使用 web_search 工具按以下策略搜索：

#### X (Twitter) 搜索策略（至少8次搜索）
```
搜索query模板：
- "{产品名} demo video site:x.com"
- "{产品名} screenshot site:x.com"
- "{产品名} built made site:x.com"
- "{产品名} workflow automation site:x.com"
- "{产品名} amazing incredible impressive site:x.com"
- "{产品名} disappointed broken issue bug site:x.com"
- "{产品名} vs comparison site:x.com"
- "{产品名} review first impressions site:x.com"
- "{产品名} tutorial how to site:x.com"
- "{产品名} thread site:x.com"
```

#### KOL专项搜索（至少5次搜索）
```
针对 references/kol-list.md 中的KOL逐一搜索：
- "karpathy {产品名}"
- "emollick {产品名}"
- "simonw {产品名}"
- "ylecun {产品名}"
- "{产品名} site:simonwillison.net OR site:oneusefulthing.org"
```

#### Reddit 搜索策略（至少4次搜索）
```
- "{产品名} site:reddit.com/r/ClaudeAI"
- "{产品名} site:reddit.com/r/artificial"
- "{产品名} site:reddit.com/r/MachineLearning"
- "{产品名} site:reddit.com/r/LocalLLaMA"
```

#### 新闻与博客搜索（至少3次搜索）
```
- "{产品名} review 2024 OR 2025 OR 2026"
- "{产品名} launch announcement"
- "{产品名} hands-on first look"
```

### Step 3: 内容筛选与分类

#### 分类体系（用于Excel Sheet划分）

| Sheet名称 | 分类标准 | 识别关键词/特征 |
|-----------|----------|-----------------|
| **视频图片展示** | 包含视频demo、截图、GIF、实际操作录屏 | video, demo, screenshot, gif, screen recording, watch, 🎬, 📹, look at this, check this out, here's how |
| **技术讨论** | 讨论架构、实现原理、API、性能、对比分析 | architecture, API, performance, benchmark, under the hood, how it works, technical, implementation, vs, comparison, latency, tokens |
| **功能好评** | 对产品功能的正面评价 | amazing, incredible, impressive, game-changer, love, best, finally, exactly what I needed, 🔥, mind-blowing |
| **功能差评与问题** | 对产品功能的负面评价、bug报告 | disappointed, broken, issue, bug, problem, worse, failed, crashed, deleted, lost data, frustrating, unusable, expensive, overpriced |
| **KOL观点** | 来自 references/kol-list.md 中的账号 | 按账号名匹配 |

### Step 4: 生成输出

#### 4.1 Excel数据表结构

**Sheet 1: 视频图片展示**
| 列名 | 说明 |
|------|------|
| 序号 | 自增ID |
| 平台 | X / Reddit / YouTube / Blog |
| 发布日期 | YYYY-MM-DD |
| 作者 | 用户名 |
| 展示类型 | 视频 / 截图 / GIF / 录屏 |
| 展示场景 | 具体用例描述 |
| 内容摘要 | 100字以内 |
| 原文链接 | URL |
| 互动数据 | likes/retweets/views（如有）|

**Sheet 2: 技术讨论**
| 列名 | 说明 |
|------|------|
| 序号 | 自增ID |
| 平台 | X / Reddit / Blog |
| 发布日期 | YYYY-MM-DD |
| 作者 | 用户名 |
| 讨论主题 | 架构/性能/API/对比/实现原理 |
| 核心观点 | 150字以内 |
| 原文链接 | URL |

**Sheet 3: 功能好评**
| 列名 | 说明 |
|------|------|
| 序号 | 自增ID |
| 平台 | X / Reddit |
| 发布日期 | YYYY-MM-DD |
| 作者 | 用户名 |
| 称赞的功能点 | 具体功能 |
| 用户评价原文 | 关键评价内容 |
| 原文链接 | URL |

**Sheet 4: 功能差评与问题**
| 列名 | 说明 |
|------|------|
| 序号 | 自增ID |
| 平台 | X / Reddit |
| 发布日期 | YYYY-MM-DD |
| 作者 | 用户名 |
| 问题类型 | Bug/性能/定价/功能缺失/安全 |
| 问题描述 | 具体问题 |
| 原文链接 | URL |

**Sheet 5: KOL观点**
| 列名 | 说明 |
|------|------|
| 序号 | 自增ID |
| KOL姓名 | 真实姓名 |
| 账号 | @username |
| 身份背景 | 职位/公司 |
| 核心观点 | 200字以内 |
| 情感倾向 | 正面/负面/中性 |
| 原文链接 | URL |

**Sheet 6: 统计摘要**
- 数据抓取时间
- 各分类数量统计
- 情感分布饼图数据
- 平台分布
- 热门话题词云数据

#### 4.2 分析报告 (Markdown)
使用 references/report-template.md 模板生成报告

## 数据量要求

- **最低目标**: 50条有效数据
- **理想目标**: 80-100条有效数据
- **搜索次数**: 15-20次web_search调用
- **去重处理**: 相同URL只保留一条

## 注意事项

1. **充分搜索**：必须执行足够多次搜索以覆盖不同角度，不要过早停止
2. **多样化query**：变换关键词组合，避免重复相似搜索
3. **时效性**：注明数据抓取时间，社交媒体内容变化快
4. **链接验证**：确保所有链接可访问
5. **隐私保护**：不收集或分析普通用户的个人信息
6. **版权合规**：仅摘要引用，不全文复制

## 示例用法

用户输入：
> "帮我抓取一下Claude Cowork最近一周在X和Reddit上的舆情，重点关注用户展示的good case"

执行流程：
1. 确认产品名：Claude Cowork
2. 确认时间：最近一周
3. 执行15-20轮web_search（覆盖视频demo、技术讨论、好评、差评、KOL等维度）
4. 按新分类体系整理数据到Excel（5个内容Sheet + 1个统计Sheet）
5. 生成分析报告
