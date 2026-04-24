# OuYi Hub — 部署指南

> 4 语言加密交易所导航站 · 动态网络时间 · 完整 SEO · 独立后台

---

## 📦 文件清单

| 文件 | 作用 |
|---|---|
| `index.html` | 主站（SPA，4 语言，动态年份） |
| `en.html` | 英文独立首页（Google 直接抓取） |
| `mgr-7a9f3c2e.html` | 后台管理（URL 保密） |
| `og-image.svg` | 社交分享图（1200×630，源文件） |
| `generate-og-image.html` | SVG→PNG 本地生成工具 |
| `CNAME` | 绑定 `theouyi.com` |
| `robots.txt` | 爬虫规则 |
| `sitemap.xml` | 站点地图（含 image sitemap） |
| `404.html` | SPA 路由回退 |
| `.github/workflows/update-sitemap.yml` | **每日自动更新 sitemap lastmod** |

---

## ⏰ 动态网络时间系统（本版核心）

- 网站年份**不再硬编码**，用 `{YEAR}` 占位符 + JS 实时注入
- 三源容错同步：**OKX API → WorldTimeAPI → timeapi.io**
- 每 30 分钟自动刷新
- 自动更新：`<title>` / `meta description` / `og:title` / JSON-LD `dateModified` / 所有 `data-year` 元素
- 后台有实时时钟条，显示同步源和同步时长
- 离线时回退到本地时间，不会崩

---

## 🚀 部署 GitHub Pages

### 1. 推送代码
```bash
git init && git add . && git commit -m "Deploy OuYi Hub"
git branch -M main
git remote add origin https://github.com/<用户名>/<仓库名>.git
git push -u origin main
```

### 2. 开启 GitHub Actions
第一次 push 后，GitHub 会自动检测 `.github/workflows/update-sitemap.yml`。
- 仓库 Settings → Actions → General → 勾选 **Allow all actions**
- 勾选 **Read and write permissions**（让 Action 有权提交 sitemap 更新）

Action 每天 UTC 03:00（HK 11:00）自动运行，把 sitemap 的 `lastmod` 改成当日。也可手动触发（Actions 标签 → Update Sitemap → Run workflow）。

### 3. 启用 Pages
Settings → Pages → Source: `main` / `/ (root)` → Custom domain `theouyi.com` → Enforce HTTPS

### 4. DNS 配置
```
A @      185.199.108.153
A @      185.199.109.153
A @      185.199.110.153
A @      185.199.111.153
CNAME www <用户名>.github.io
```

---

## 🔐 后台访问

**URL：** `https://theouyi.com/mgr-7a9f3c2e.html`
**默认密码：** `admin888`（首次登录立即修改）

### 后台快捷键
| 快捷键 | 功能 |
|---|---|
| `N` | 新建文章 |
| `/` | 聚焦搜索框 |
| `Ctrl/⌘+S` | 保存当前文章 |
| `Ctrl/⌘+B` | 导出 JSON 备份 |
| `Esc` | 返回文章列表 |

点击右上角 `⌨` 图标查看完整帮助。

### 后台新功能
- **实时网络时钟** — 显示 UTC 时间 + 同步源
- **统计仪表板** — 文章数、发布状态、启用币种一目了然
- **快速发布切换** — 列表里一键转发布/草稿
- **一键复制文章** — 自动加 "[副本]" 前缀
- **语言翻译状态** — 每篇文章 4 种语言指示（绿色=已翻译）
- **实时搜索** — 标题/内容/标签过滤
- **自动翻译** — 中文为源，一键翻译到繁/英/韩

---

## 🖼️ 生成 og-image.png

分享图 SVG 已做好，浏览器需要 PNG 版：

1. 解压后**双击** `generate-og-image.html`
2. 浏览器显示预览
3. 点击 **Download PNG** 保存到项目根目录
4. Push 到 GitHub

**也可用在线工具：** https://cloudconvert.com/svg-to-png

---

## 📄 页面路由

| URL | 内容 |
|---|---|
| `/` | 首页（4 语言 SPA） |
| `/#/about` | 关于我们（AboutPage schema） |
| `/#/articles` | 文章列表 |
| `/#/article/<id>` | 文章详情 |
| `/#/coin/BTC-USDT` | BTC K 线 |
| `/en.html` | 英文独立首页 |
| `/mgr-7a9f3c2e.html` | 后台 |

---

## 🔍 SEO 全景

### 结构化数据（7 块 JSON-LD）
1. WebSite + SearchAction
2. Organization（明确非交易所声明）
3. WebPage（dateModified 自动更新）
4. BreadcrumbList（5 级）
5. FAQPage（6 问答）
6. AboutPage
7. ItemList（交易所排名）

### hreflang 覆盖
10 条：zh-CN / zh-Hans / zh-TW / zh-Hant / zh-HK / **en → en.html** / en-US → en.html / ko / ko-KR / x-default

### GEO 标签
- `geo.region: HK` + `geo.position: 22.3193;114.1694`
- Dublin Core：`DC.language` / `DC.publisher` / `DC.rights` / `DC.coverage: Worldwide`
- `target: all` / `audience: all`

### 其他
- Image sitemap（`<image:image>` 包 og-image.png）
- preconnect / dns-prefetch 到关键域
- `Disallow: /mgr-` + 屏蔽 GPTBot/CCBot 等 AI 爬虫

---

## 📈 搜索引擎提交

### Google Search Console
1. https://search.google.com/search-console 添加 `theouyi.com`
2. 提交 `sitemap.xml`
3. 修改 `index.html` 和 `en.html` 的验证码占位符

### Bing / Naver / Yandex / 百度
README 底部有各平台链接 + 验证 meta 位置。

---

## 🌐 翻译系统

- **源**：Google Translate 免费端点 + MyMemory fallback
- **流程**：简中为源 → 翻译到 繁中/英/韩
- **总开关**：后台「网站设置」→ 发布时自动翻译
- **生产建议**：量大时接 Google Cloud Translation API（$20/百万字符）

---

## 💰 行情数据

- **主源**：OKX `/api/v5/market/tickers?instType=SPOT`（30 秒刷新）
- **备用**：CoinGecko
- **K 线**：TradingView · 数据源 `OKX:BTCUSDT`
- **响应式**：图表高度 `clamp(440px, 72vh, 820px)`，移动端自动隐藏侧边工具栏

---

## 🛠️ FAQ

**Q：时间显示不对？** 后台右上角有 🔄 立即同步。检查网络是否能访问 okx.com（如果被墙会 fallback 到其他时间源）。

**Q：数据存哪？** localStorage 键 `csp3`。后台「数据管理」→ 导出 JSON 备份。

**Q：忘密码？** F12 → Application → Local Storage → 删 `csp3` → 重置为 `admin888`（清空所有数据）。

**Q：刷新 404？** `404.html` 已处理 SPA 路由。

**Q：GitHub Action 没运行？** Actions 需要仓库至少有一次手动 push 后才开始计划。Settings → Actions → 确保已启用并给 write 权限。

---

## 📄 法律合规

1. 页脚 4 语言声明**无隶属关系**
2. About 页明确**非交易所/非经纪商/不托管资金**
3. 外链 `rel="nofollow sponsored"`
4. **不得**收集访客密码/私钥/API Key

---

© OuYi Hub · theouyi.com
