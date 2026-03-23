# 内容分类体系

## 分类总览（用于Excel Sheet划分）

| Sheet名称 | 分类标准 | 识别关键词/特征 |
|-----------|----------|-----------------|
| **用户实测案例** | 用户亲自使用产品完成具体任务的真实案例（Good Cases），必须是实际操作而非转发或评论 | I tried, I used, I built, I made, I asked it to, here's what happened, it helped me, worked for me, my workflow, my project, 实测, 试用 |
| **技术讨论** | 讨论架构、实现原理、API、性能、对比分析 | architecture, API, performance, benchmark, under the hood, how it works, technical, implementation, vs, comparison, latency, tokens |
| **功能好评** | 对产品功能的正面评价（非实测案例的一般性称赞） | amazing, incredible, impressive, game-changer, love, best, finally, exactly what I needed, mind-blowing |
| **功能差评与问题** | 对产品功能的负面评价、bug报告 | disappointed, broken, issue, bug, problem, worse, failed, crashed, deleted, lost data, frustrating, unusable, expensive, overpriced |
| **KOL观点** | 来自 references/kol-list.md 中的账号 | 按账号名匹配 |

## 用户实测案例的判断标准

这是最重要的分类，也是最容易判断失误的。核心原则：必须看到"某人用这个产品做了某件具体的事"。

### 必须满足的条件
1. 展示了使用产品完成具体任务的案例（用户实测或官方演示均可）
2. 描述了具体的使用场景和任务
3. 展示了实际的输入/输出或结果
4. 可以是成功案例也可以是失败案例

### 属于用户实测案例
- 用户亲自使用产品的案例
- 官方发布的产品使用演示视频/截图
- 官方博客中展示的具体用例
- KOL/开发者分享的实际使用过程

### 不属于用户实测案例
- 下载安装指南（没有展示具体用例）
- 纯粹的功能评论（没有实际使用展示）
- 转发他人案例但没有补充自己使用经验
- 新闻报道中的功能介绍（非案例展示）

## 分类冲突处理

一条内容可能同时符合多个分类。处理规则：
1. KOL观点优先级最高：如果来自KOL列表中的账号，归入KOL观点（即使内容也包含实测案例）
2. 用户实测案例优先级第二：如果包含实际使用过程，优先归入实测案例
3. 技术讨论 vs 好评/差评：如果内容以技术分析为主（含数据、对比），归入技术讨论
