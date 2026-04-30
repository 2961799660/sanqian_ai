# 三千AI智能体 · 版本更新日志

所有版本的重要变更都会记录在此。版本命名规则:`vYY.M.X` · 年.月.迭代号。

---

## [v25.3.6] - 2026-04-30

### 新增
- **社媒数据**:新增「现在全网在聊什么」实时热点模块,接入抖音/微博/B站/快手/小红书/视频号 6 平台,真实数据每整点小时自动刷新
- **工作台引流**:口播IP工作台顶部「现在大家在聊」teaser banner,3 条最新热搜一行展示
- **一键复制**:每条热搜支持一键复制标题到剪贴板
- **作品广场**:新增 `/square` 页面,展示用户对话/画布生成并发布的图片/视频作品(支持点赞、浏览计数)
- **TikHub 后台密钥管理**:TikHub API Key 可在「后台 - 密钥配置」中可视化配置(无需改 .env 文件,5 分钟内生效)
- **AuthGuard 双通道鉴权**:后端同时支持 Bearer Header + cookie,解决前端时序问题导致的间歇性 401

### 优化
- **品牌字样统一**:全站 BuildingAI / Building 替换为「三千AI」(app.config / 5 个 vue / 4 份 i18n / logo-full.svg 重做)
- **Canvas 工作台 logo**:左上角原「画」青蓝渐变 → 紫色品牌方块「三千」
- **Workflow 工作流子应用**:残留扣子 / Coze 字符全清(JS bundle 19+ 处文本替换)
- **作品广场视觉**:浅色反差报刊风(`#fafaf8` 米白底 + `#1a1a1a` 高对比黑 + `#d4a574` 暖琥珀点缀),反 AI 味
- **agreement 协议页**:中文 title + 强制 light 主题,修复 dark/light race 导致的低对比度阅读问题
- **chat 余额不足**:从 silent redirect 改为 toast 提示,保留对话上下文不丢
- **notifications 公告**:DB 改写公告内容,清掉「千帆」「蒋点AI」「4 月 5 日 22:00」等过期文案
- **Canvas 子应用 useCanvasApi**:onRequest 加 localStorage fallback + 401 诊断日志

### 修复
- 修复 SSO 桥重复 login 导致 `user.session_key` 被覆盖,Workflow 子应用多 tab 切换时间歇性 "session not exist"
- 修复 IP 卡片不可访问性:加 `role="button"` + `tabindex="0"` + `keydown` enter/space 支持键盘操作
- 修复 BuildingAI 部署目录缺失 `/square` 子页面导致 404
- 修复 BuildingAI 数据库 entities 列表未注册 `square_works` / `social_trending_snapshot` 导致 TypeORM `metadata not found`
- 修复 Coze 模板表「扣子官方」作者字段(`template.meta_info.user_info.name`)残留

### 已知问题
- TikHub 视频号 (`fetch_hot_words`) 端点要求复杂 `last_buffer` 参数,文档与实际不符,等 TikHub 上游修复
- TikHub 部分时段对快手 `fetch_kuaishou_hot_list_v1` 返回 `detail: null`,Cron 自动重试

---

## [v25.3.5] - 2026-04-23

### 新增
- **AI 画布**:新增「Copilot 面板」,选中节点可让 AI 优化提示词并推荐下一步节点
- **画布节点**:新增「去水印」「首尾帧」「融合」3 个节点,总数扩到 20
- **文生视频**:接入豆包 Seedance 2.0 Pro
- **文生图**:接入豆包 Seedream 5.0
- **多模型 LLM**:新增 xAI Grok、文心一言入口
- **MCP 工具**:新增 skillmaster-mcp(视频文案提取)、tikhub-douyin(抖音数据)
- **口播IP工作台**:新增「知识图谱」模块
- **后台**:DIY 装修中心新增主题色选项

### 优化
- 文案工坊风格标签从 8 种扩展到 12 种
- 画布批量执行优化:CROSS 模式支持最多 3 维参数交叉
- PostgreSQL 升级到 17.6(pgvector + zhparser)
- Redis 升级到 8.2.2
- 前端构建产物体积减小 18%

### 修复
- 修复大批量 ZIP 模式下任务队列堵塞问题
- 修复部分浏览器下 SSE 流式响应断连问题
- 修复知识库中文分词边界问题

---

## [v25.2] - 2026-02

### 新增
- 口播IP工作台「定位引擎」模块上线,引入「立权威·建信任·做转化」三轮配比可视化
- 接入 Kling V3、Kling 动作控制模型
- 后台新增「推广返佣」模块

### 优化
- Nuxt 升级到 4.x(SSR 首屏速度提升约 30%)
- 数字人 + 口型同步节点合并为一体化流程

---

## [v25.1] - 2026-01

### 新增
- 口播IP工作台首版发布:灵感爆发 / 文案工坊 / 数据回流 三大核心模块
- 接入深度求索、智谱 GLM
- MCP 协议支持

---

## [v24.12] - 2025-12

### 新增
- AI 画布首版发布:15 节点,ZIP 批量模式
- 支持 GPT / Claude / Gemini / Kimi 多模型聚合
- Docker Compose 一键部署方案

---

## 版本发布节奏

- **月度小版本**:功能增量、性能优化、问题修复
- **季度大版本**:重大模块更新、架构升级
- 所有买断用户自动获得终身免费更新

## 订阅更新通知

加商务微信 `q2026560558`,每次大版本发布会同步通知。

---

© 2026 小妍妍网络科技
