---
name: ai-sentiment-tracker
description: "AI产品舆情抓取与分析工具，从X(Twitter)、Reddit等平台收集AI产品的用户反馈、使用案例和专家评价，生成可视化HTML报告和Excel数据表。确保在以下场景触发此skill：(1) 用户想了解某个AI产品/模型的社交媒体反应或口碑 (2) 抓取用户good cases、demo展示、使用案例 (3) 收集AI领域KOL和学者的评价 (4) 分析新AI产品发布后的舆情 (5) 用户说'看看大家怎么说'、'社交媒体上的反应'、'what people are saying about' (6) 任何涉及AI产品sentiment analysis、用户反馈收集、社交媒体监测的请求，即使用户没有明确说'舆情'。中英文请求均应触发。"
---

# AI舆情抓取与分析 Skill

## 工作流程

### Step 1: 确认抓取参数
从用户获取以下信息：
- **目标产品/模型名称**：如 Claude Cowork, Gemini 3, GPT-5 等
- **时间范围**：默认最近7天，可自定义
- **重点关注的KOL**：默认包含 references/kol-list.md 中的AI学者

### Step 2: 执行抓取

#### 数据源优先级

**首选：Apify Actors**（如果环境中有 Apify 工具可用）
Apify 能直接抓取社交平台，数据质量远高于 web search 拼 `site:` 参数。优先使用以下 Actor：
- X/Twitter 抓取：搜索 Apify Store 中的 Twitter/X scraper，用产品名作为搜索关键词
- Reddit 抓取：搜索 Apify Store 中的 Reddit scraper，指定目标 subreddit

**备选：Web Search**（Apify 不可用时使用）
使用 web_search 工具，按以下策略搜索。注意：`site:x.com` 的效果不稳定，搜索结果可能有限，这是正常的。

#### X (Twitter) 搜索策略
```
搜索query模板（根据实际效果选择性使用，不必全部执行）：
- "{产品名} demo OR video site:x.com"
- "{产品名} built OR made site:x.com"
- "{产品名} amazing OR impressive OR incredible site:x.com"
- "{产品名} disappointed OR broken OR bug site:x.com"
- "{产品名} vs OR comparison site:x.com"
- "{产品名} review OR impressions site:x.com"
```

#### KOL专项搜索
```
针对 references/kol-list.md 中的重要KOL搜索：
- "karpathy {产品名}"
- "emollick {产品名}"
- "simonw {产品名}"
- "ylecun {产品名}"
- "{产品名} site:simonwillison.net OR site:oneusefulthing.org"
```

#### Reddit 搜索策略
```
- "{产品名} site:reddit.com/r/ClaudeAI"
- "{产品名} site:reddit.com/r/artificial"
- "{产品名} site:reddit.com/r/MachineLearning"
- "{产品名} site:reddit.com/r/LocalLLaMA"
```

#### 新闻与博客搜索
注意：使用当前年份（通过系统日期获取），不要硬编码年份。
```
- "{产品名} review {当前年份}"
- "{产品名} launch announcement"
- "{产品名} hands-on first look"
```

### Step 3: 内容筛选与分类

分类体系详见 `references/classification.md`。核心分为5类：用户实测案例、技术讨论、功能好评、功能差评与问题、KOL观点。

其中"用户实测案例"的判断最关键——必须是用户亲自使用产品完成具体任务的真实记录，而非转发、纯评论或功能介绍。详细判断标准见 `references/classification.md`。

### Step 4: 生成输出

#### 4.1 Excel数据表
Excel 表的详细结构定义见 `references/excel-schema.md`（共6个Sheet）。

生成Excel时，如果环境中有 xlsx skill 可用，参考该 skill 获取最佳实践，确保格式专业。

#### 4.2 HTML分析报告
使用 `references/report-template.md` 中的结构作为内容大纲，但最终输出为一个**自包含的 HTML 页面**（不是 .md 文件）。

HTML报告要求：
- 单文件、自包含，内联所有CSS和JS
- 现代化设计，使用渐变色卡片、阴影、圆角等视觉元素
- 包含数据可视化：情感分布饼图（用 Chart.js CDN 或纯 CSS/SVG）、平台分布柱状图
- 表格使用斑马纹样式，链接可点击
- 响应式布局，手机端也能正常阅读
- 顶部放概览摘要卡片（总数据量、正负面比例、KOL数量）
- 保存到用户的输出目录，文件名格式：`{产品名}_舆情报告_{日期}.html`

## 数据量要求

数据量目标应根据产品热度动态调整，而非一刀切：
- **热门产品**（如 GPT、Claude、Gemini 等主流模型）：目标 50-100 条
- **中等热度产品**（如新发布的中型公司AI工具）：目标 30-50 条
- **小众/新产品**：20-30 条也是合格的，不要为了凑数而引入低质量内容

通用规则：
- **去重处理**: 相同URL只保留一条
- **用户实测案例**: 重点抓取，热门产品至少10条，小众产品至少5条

### 当数据不足时的降级策略

如果搜索结果明显偏少（少于预期的50%），按以下顺序尝试：
1. **扩大时间范围**：从7天扩展到14天或30天
2. **放宽关键词**：使用更通用的产品名或去掉限定词
3. **增加平台覆盖**：加入 YouTube、Hacker News 等补充搜索
4. **如实报告**：在报告中说明数据量有限的原因和已采取的补救措施，不要编造数据

## 注意事项

1. **充分搜索**：执行足够多次搜索以覆盖不同角度，但也要根据返回结果的质量灵活调整，不必机械执行固定次数
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
3. 检查是否有 Apify 工具可用 → 有则优先用 Apify 抓取 X 和 Reddit
4. 补充 web_search 搜索 KOL观点、新闻博客等 Apify 不覆盖的来源
5. 按分类体系整理数据（详见 references/classification.md）
6. 生成 Excel 数据表（结构详见 references/excel-schema.md）
7. 生成 HTML 可视化报告
