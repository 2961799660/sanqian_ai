# 三千AI智能体 · 版本更新日志

所有版本的重要变更都会记录在此。版本命名规则:`vYY.M.X` · 年.月.迭代号。

---

## [v1.0.7] - 2026-05-17

### 新增
- **🎵 AI 音乐工坊**(智能体广场):接入 Suno V5/V4.5+ 全套能力,一个卡片 5 个 Tab —— 🎵 音乐生成(一次返 2 首 + 封面)、✍️ 歌词创作(带分段标记)、✂️ 续写翻唱(支持上传 mp3)、🔊 音轨处理(人声分离/加人声/加伴奏/转 WAV)、🎬 MV 生成。每首歌自动镜像到自家 OSS 永久化。
- **📊 AI PPT 生成**(智能体广场):接入 AIPPT.cn 全流程,主题→大纲(SSE)→内容(SSE)→pptx 文件,1-3 分钟出片。模板自动加载 12 张缩略图可视化选择,生成时自动套用。
- **🎬 数字人 4-Tab 全模式**:原"提交音频 URL"单一模式扩展为 4 个 Tab —— 🎤 文字→视频(选预设音色)、👤 自传形象+文字(上传人像)、🎙️ 声音克隆+文字(火山 SeedICL 2.0 真克隆)、⚙️ 高级(直接粘 audioUrl)。
- **📁 文件上传按钮**:数字人 3 个文件字段(形象图/参考音频/高级音频)+ Suno 续写翻唱、音轨处理 2 个上传槽位,都加「📁 上传」按钮(走 `/api/upload/file`),自动写入 OSS URL。仍可手动粘 URL。
- **富落地页 Hero**:数字人 / Suno / AIPPT 三张卡片新增大 Hero(eyebrow + 大字标题 + CTA)+ 「能做什么」用例网格(6 个示例点一下自动填表单)+ 声音/风格选择器 + 真实素材 demo 轮播(数字人 5 段真用户成片 / Suno 2 首带封面 / AIPPT 4 张模板示意)。
- **后台菜单分类 6 大区**:重构系统管理菜单为 📊 数据看板 / 🤖 AI 中枢 / 📝 内容运营 / 👥 用户与会员 / 💰 财务管理 / ⚙️ 系统,合并原"订单管理 + 财务管理"两个父级,渠道管理从顶级移到系统设置下,公告管理移到内容运营下。
- **AIPPT 后台配置页**(超管):系统设置 → AIPPT 配置,可视化录入 AccessKey / Secret / Channel / UID,带"测试鉴权"按钮,token 自动按 30 天缓存。
- **DIY 中心 + 站点信息合并**:站点信息从"系统设置"移到 DIY 中心下,跟微页面 / 前台导航并列,统一后台装修入口。

### 优化
- **后台菜单全中文化**:90+ `console-menu.xxx.title` i18n key 由于命名空间 `console-menu`(含连字符)与 vue-i18n dot-path 冲突,直接在 menus 表 name 字段写入中文,前端 `menu-helper.ts` 的 `t(menu.name) || menu.name` 兜底兼容,所有菜单立刻显示中文。
- **数字人合成卡片**:移除"🚧 服务搭建中: 117.50 GPU 服务器安装 MuseTalk"占位 UI,接通已就绪的火山 chimera 后端(`POST /api/auto-tools/digital-human` 等)。
- **清理成本/来源文案**:数字人 / Suno / AIPPT 三张卡片 Hero / hint / agents.description 中去除 "¥0.5/页上游"、"算力扣 50/次"、"Suno V5"、"火山奇美拉"、"Pixelle 内部"、"按秒扣 (10 算力/秒 · 会员 4)" 等成本/来源细节。
- **后台支付选项分开**:`pay_type=1` 和 `pay_type=3` 原本在 payconfig 表里都叫"微信支付"导致前端展示重名,`pay_type=3` 改名"易支付" + 加独立 logo,sort 重排。
- **作品广场内容补全**:14 件 `pixelle/video` 空壳作品的 `cover_url` 从 `sourceData.videos[0]` 反推填上;删 9 件 admin 重复"数字人口播" / 测试用户作品 / 乱码标题;新增 6 件电商带货模板示例(美妆/服饰/家居/食品/3C/个护)。
- **Suno 续写/音轨/MV tab 改下拉选历史**:不再让用户手动粘 taskId,自动列出已成功的 generate 任务做下拉,无历史时给「先去音乐生成」一键跳转链接。
- **AIPPT templates 未配置时静默失败**:后端在 AIPPT key 未录入时返回空数组而非 500,前端模板区显示橙色提示而非 JS crash。

### 修复
- **微信支付 5/9 以来 0 笔成功** 根因:`packages/api/src/modules/pay/dto/prepay.dto.ts` 的 `channel?: string` 字段缺 `@IsOptional()`,全局 ValidationPipe `forbidNonWhitelisted: true` 直接 400 拦截。修复:加上 `@IsOptional()`。
- **数字人 Tab 4 完全无法用** 根因:容器内 `@ffprobe-installer/linux-x64/ffprobe` 二进制缺执行权限(`EACCES`),无法探测音频时长。修复:`chmod +x` 给 ffprobe / ffmpeg。
- **数字人试听 voice 400** 根因:DH 前端 voice key 用了 `lively_girl / serious_brother / warm_man`,但后端 `NlsVoiceSkin` 枚举只接受 `playful_buddy / serious_teacher / kind_grandpa`。修复:DH 改用合法 key,试听调 `/api/story-oneshot/voice-preview/:skin` 端点(故事工坊已有的)。
- **AIPPT 卡片 TypeError "Cannot read properties of undefined (reading 'length')"**:`aipptTemplates / aipptTemplatesLoading / aipptSelectedTemplateId / loadAipptTemplates` 4 个 ref 声明遗漏,模板区 `v-if="aipptTemplates.length"` 读 undefined 直接 crash。修复:补上 ref 声明 + load 函数 + slug watcher。
- **AIPPT demo 4 张占位图 404**:`url: '.../ai-generated/.../*.jpg'` 而 OSS 实际后缀是 `.jpeg`。修复:.jpg → .jpeg。
- **logo 在前端显示空白** 根因:`config.logo_url / webinfo.logo` 字段值是 PostgreSQL `text[]` 字面量 `{"https://..."}`(某次上传时数组被当字符串存入),前端 `<img src="{...}">` 无法解析。修复:DB UPDATE 用 regex 剥包装 + `site-identity.service.ts` 加 `unwrapPgArrayText()` 防御性兜底(读取 + 写入双端),避免遗留代码再次污染。
- **失败也扣算力**:数字人 3 个 endpoint(`/digital-human` `/text-to-digital-human` `/pixelle/digital-human-customize`)外层加 try/catch,生成失败自动调 `powerActions.refund(userId, actionKey, amount, reason)` 退还算力,账本日志写明退款原因。
- **火山 chimera 单账号并发限制导致 Tab 3 失败**:`DigitalHumanService.runWithMutex` 串行化整个 `submit + poll` 阶段(static `mutexQueue` 跨实例共享),并发请求自动排队等前一笔完成,不再被火山拒"当前并发已满"。

### 已知
- **`canvas/coze-bridge/sso 500`**:旧 bug 仍未根除(`Cannot set headers after they are sent to the client`),已记入 backlog 待修。
- **`auth/wechat-qrcode 500`**:扫码登录间歇 500,已记入 backlog 待修。
- **AIPPT 模板列表为空**:需录入 AIPPT 4 项凭据(AccessKey / Secret / Channel / UID)。后台 → 系统 → 系统设置 → AIPPT 配置 录入后,模板网格立刻加载 12 张真实模板缩略图。
- **Suno 上游积分耗尽**:用户余额 < 10 时无法生成新歌。需要去 sunoapi.org 充值。

---

## [v25.3.7] - 2026-04-30

### 优化
- **方案 E 浅色主题全站化**:聊天首页 / chat layout 默认 light(米白 #fafaf8 + 高对比黑字 + 暖琥珀点缀),用户可在 sidebar 切回 dark 并永久记住偏好
- **sidebar 字色 light 适配**:14 处 `rgba(255,255,255,X)` 硬编码白字在 light 模式下迁移到全局 CSS cascade,sb-section-header / sb-square-entry / sidebar-notice-compact / cv-* / new-chat-btn 都可见
- **`.hp-main` 主区背景**:从 #f0f4f8 冷蓝白 → #fafaf8 暖米白
- **`--accent-warm`** 新 design token #d4a574 暖琥珀

### 修复
- 修复 `body` 背景 `rgb(13,14,16)` 黑色硬编码 dark 残留,加 `html.light body { background: #fafaf8 !important }` 全局兜底
- 修复 `.ia-card` 边框 `rgba(255,255,255,0.08)` 在 light 下不可见,改 `html.light` cascade 用黑色边
- 修复 Vue scoped CSS 中 `:global(html.light) .sb-X` 编译丢失 `[data-v-*]` attr 的 cascade 不生效问题(把 light overrides 移到全局 glass-theme.css)

### 已知
- BuildingAI 部分组件(头像/某些 toast)仍依赖 nuxt-color-mode 默认 token,light 下颜色有偏差,后续逐步替换为 design token

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
