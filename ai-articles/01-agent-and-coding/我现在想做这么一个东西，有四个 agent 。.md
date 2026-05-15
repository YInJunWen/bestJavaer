# 内容检索分发平台架构设计

> 日期：2026-03-28

我现在想做这么一个东西，有五个 agent 。



第一个 agent 负责网上搜集信息源，我可以指定哪种类型的信息源，这个 agent 可以去做收集



第二个 agent 负责把收集到的内容经过一层渲染，渲染到一个 AI 编辑器中



第三个 agent 负责针对不同的市场喜好，把文章渲染成为不同的格式，比如掘金 知乎 头条 CSDN X 小红书 等



第四个 agent 负责把这个编辑器的内容进行分发。



第五个 agent 负责统计各平台发布效果（阅读量、互动），反向优化 Agent 3 的适配策略







可行性分析与架构方案设计

  需求概述

  构建一个五智能体协同的内容生产与分发系统：

  1. 信息收集Agent - 根据指定类型从网络收集信息源
  2. 内容渲染Agent - 将收集的内容渲染到AI编辑器中
  3. 平台适配Agent - 根据不同平台(掘金/知乎/头条/CSDN/X/小红书)的喜好适配文章格式
  4. 内容分发Agent - 将编辑器内容发布到各个平台
  5. 数据分析Agent - 统计各平台发布效果，反向优化平台适配策略

---
  可行性分析

  技术可行性：✅ 完全可行

  ![image-20260328135502512](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260328135502512.png)

  挑战与限制：

  1. 反爬限制 - 部分平台对爬取和自动化发布有反爬机制
  2. API依赖 - 需要申请各个平台的开发者API密钥
  3. 格式变化 - 各平台可能会更新编辑器格式，需要维护
  4. 成本 - LLM调用+搜索引擎API会产生一定成本

---
  架构设计

  整体架构图

  ┌─────────────────────────────────────────────────────────────────────────┐
  │                        用户输入 (指定主题/信息源类型)                        │
  └────────────────────────────────────┬────────────────────────────────────┘
                                   ┌──▼──┐
                                   │ Agent 1: 信息收集 │
                                   └──┬──┘
                                       │ 收集到的原始信息 (URLs + 摘要)
                                       ▼
  ┌─────────────────────────────────────────────────────────────────────────┐
  │                        信息存储 (Vector DB / 文件系统)                       │
  └────────────────────────────────────┬────────────────────────────────────┘
                                   ┌──▼──┐
                                   │ Agent 2: 内容渲染 │
                                   └──┬──┘
                                       │ 整理后的完整文章 (Markdown)
                                       ▼
  ┌─────────────────────────────────────────────────────────────────────────┐
  │                            AI 编辑器 (支持人工编辑)                          │
  └────────────────────────────────────┬────────────────────────────────────┘
                                   ┌──▼──┐
                                   │ Agent 3: 平台适配 │
                                   └──┬──┘
        ┌───────────────────┬─────────┼─────────┬───────────┬──────────┐
        ▼                   ▼         ▼         ▼           ▼          ▼
    掘金适配            知乎适配    头条适配    CSDN适配    X适配    小红书适配
        └───────────────────┴─────────┴─────────┴───────────┴──────────┘
                                       │ 各平台格式的终稿
                                       ▼
                                   ┌──▼──┐
                                   │ Agent 4: 内容分发 │
                                   └──┬──┘
                                       │ 发布记录 (URLs, 发布时间)
                                       ▼
  ┌─────────────────────────────────────────────────────────────────────────┐
  │                         发布结果存储 (SQLite / PostgreSQL)                  │
  └────────────────────────────────────┬────────────────────────────────────┘
                                   ┌──▼──┐
                                   │ Agent 5: 效果分析 │
                                   └──┬──┘
                                       │ 统计数据 + 优化建议
                                       ▼
  ┌─────────────────────────────────────────────────────────────────────────┐
  │                   反向优化 → 更新 Agent 3 平台适配策略                        │
  └─────────────────────────────────────────────────────────────────────────┘

  技术栈选型建议

  语言与框架

  - Python + LangChain / Autogen - 成熟的多Agent框架，生态丰富
  - 或者 TypeScript + LangChain.js - 适合前端集成编辑器

  依赖服务

 ![image-20260328135445236](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260328135445236.png)

---
  各Agent详细设计

  1. 信息收集Agent (Information Collection Agent)

  职责：
  - 接收用户指定的信息类型/主题
  - 自动搜索相关网页、文章、新闻
  - 提取核心内容和来源链接
  - 去重并按相关性排序

  输入：
  {
    "topic": "AI Agent 最新进展",
    "source_type": "blog,paper,news",
    "max_sources": 10
  }

  输出：
  {
    "topic": "AI Agent 最新进展",
    "sources": [
      {
        "url": "https://...",
        "title": "...",
        "summary": "...",
        "content": "...",
        "published_date": "...",
        "author": "...",
        "score": 0.95
      }
    ],
    "collected_at": "2026-..."
  }

  关键技术：
  - 基于LLM的搜索query生成
  - 相关性排序算法
  - 内容去重（embedding相似度）

---
  2. 内容渲染Agent (Content Rendering Agent)

  职责：
  - 将多个来源的信息整合为一篇完整文章
  - 按照逻辑结构排版
  - 生成引言、正文、结论
  - 输出到AI编辑器供人工编辑

  工作流：
  1. 阅读所有收集到的信息源
  2. 提取核心论点和关键数据
  3. 组织文章大纲
  4. 撰写完整内容
  5. 格式化为Markdown

  输入： Agent 1 的输出
  输出： 结构化Markdown文章

---
  3. 平台适配Agent (Platform Adaptation Agent)

  职责：
  - 根据目标平台的特性，将通用文章转换为该平台偏好的格式
  - 调整标题、开头、标签、配图建议等

  各平台适配策略表：

  ![image-20260328135426804](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260328135426804.png)

  输入： 编辑好的通用文章 + 目标平台列表
  输出： {platform: 适配后的文章内容}

  可优化点 - Agent 5 根据数据反向调整这里的策略

---
  4. 内容分发Agent (Content Distribution Agent)

  职责：
  - 读取适配后的内容
  - 通过各平台API或自动化方式发布
  - 记录发布结果和URL

  实现方式：
  - 有API的平台：使用官方API发布
  - 无公开API的平台：使用Playwright自动化登录发布
  - 需要维护各平台的认证信息（cookie/token）

  输入： {platform: content} 映射
  输出： [{platform, url, status, published_at}]

---
  5. 数据分析Agent (Analytics & Optimization Agent)

  职责：
  - 定时拉取各平台发布数据（阅读量、点赞、评论、收藏）
  - 分析不同平台的内容特征对效果的影响
  - 生成优化建议，更新Agent 3的适配策略

  采集指标：
  - 阅读量/展示量
  - 点赞/喜欢
  - 评论数
  - 收藏/转发
  - 互动率 = (点赞+评论+收藏) / 阅读量

  优化逻辑：
  统计 → 对比不同适配方式的效果 → LLM分析规律 → 更新平台提示词 → 下次效果更好

  输入： 一段时间的发布记录
  输出：
  - 效果统计报告
  - 适配策略更新建议

---
  数据模型设计

  topics
  ├── id
  ├── title (主题)
  ├── created_at
  └── status

  sources
  ├── id
  ├── topic_id
  ├── url
  ├── title
  ├── summary
  ├── content
  ├── published_date
  ├── author
  ├── relevance_score
  └── collected_at

  articles
  ├── id
  ├── topic_id
  ├── original_content (Agent 2输出)
  ├── created_at
  └── edited_at

  platform_articles
  ├── id
  ├── article_id
  ├── platform
  ├── adapted_content
  ├── published_url
  ├── published_at
  └── status

  analytics
  ├── id
  ├── platform_article_id
  ├── views
  ├── likes
  ├── comments
  ├── bookmarks
  ├── collected_at
  └── updated_at

  adaptation_strategies
  ├── id
  ├── platform
  ├── prompt_template
  ├── performance_score
  ├── created_at
  └── updated_at

---
  目录结构设计

  content-distribution-system/
  ├── README.md
  ├── requirements.txt
  ├── .env.example
  ├── main.py                    # 入口文件
  ├── agents/
  │   ├── __init__.py
  │   ├── base_agent.py          # Agent基类
  │   ├── collector_agent.py     # Agent 1: 信息收集
  │   ├── renderer_agent.py      # Agent 2: 内容渲染
  │   ├── adapter_agent.py       # Agent 3: 平台适配
  │   ├── publisher_agent.py     # Agent 4: 内容分发
  │   └── analytics_agent.py     # Agent 5: 数据分析优化
  ├── core/
  │   ├── __init__.py
  │   ├── config.py              # 配置管理
  │   ├── database.py            # 数据库连接
  │   └── llm_client.py          # LLM客户端封装
  ├── platforms/
  │   ├── __init__.py
  │   ├── base_platform.py
  │   ├── juejin.py
  │   ├── zhihu.py
  │   ├── toutiao.py
  │   ├── csdn.py
  │   ├── twitter.py
  │   └── xiaohongshu.py
  ├── tools/
  │   ├── __init__.py
  │   ├── search.py              # 搜索引擎工具
  │   ├── scraper.py             # 网页抓取工具
  │   └── browser.py             # 自动化浏览器
  ├── models/
  │   ├── __init__.py
  │   ├── topic.py
  │   ├── source.py
  │   ├── article.py
  │   └── analytics.py
  ├── utils/
  │   ├── __init__.py
  │   ├── logger.py
  │   └── helpers.py
  └── data/                      # 数据存储 (gitignore)

---
  部署方式

  1. 本地开发：直接Python运行，SQLite存储
  2. Docker部署：打包为Docker Compose，包含PostgreSQL+向量库
  3. 定时任务：使用APScheduler或Cron定时运行Agent 5拉取数据

---
![image-20260328135402071](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260328135402071.png)

---
  总结

  这个五Agent内容生产分发系统技术上完全可行，架构清晰，各个Agent职责分离，易于开发和维护。核心优势：

  1. ✅ 模块化设计 - 每个Agent职责单一，可独立开发测试
  2. ✅ 可扩展 - 新增平台只需新增平台适配器，不影响整体架构
  3. ✅ 闭环优化 - Agent 5的数据反馈让系统越用越好
  4. ✅ 渐进式开发 - 可以先做核心功能，逐步完善各平台支持

  建议先实现核心流程（Agent1→Agent2→Agent3→Agent4），再添加Agent 5的闭环优化功能。

⏺ Exited plan mode

⏺ 我已经完成了这个五智能体内容生产分发系统的可行性分析和架构设计。

  结论

  这个项目技术上完全可行，架构清晰，模块化设计易于开发维护。

  核心架构总结

  

![image-20260328135345195](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260328135345195.png)



  架构亮点

  - 职责分离：每个Agent职责单一，便于独立开发测试
  - 闭环优化：从发布效果反向迭代优化平台适配策略，系统会越用越好
  - 易于扩展：新增平台只需要添加新的平台适配器
  - 渐进开发：可以先做核心流程，再逐步完善功能
