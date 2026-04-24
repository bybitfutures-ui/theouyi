# 有易 YouYi — 部署指南

> 4 语言（简中 / 繁中 / English / 한국어）加密货币教程 SPA · 独立后台 + 自动翻译

---

## 📦 文件清单

| 文件 | 作用 |
|---|---|
| `index.html` | 主站（4 语言 SPA，动态 SEO） |
| `mgr-7a9f3c2e.html` | **后台管理**（URL 不公开） |
| `CNAME` | 绑定 `theouyi.com` |
| `robots.txt` | 爬虫规则（搜索引擎友好 + AI 屏蔽） |
| `sitemap.xml` | 13 URL · hreflang × 10 语种 |
| `404.html` | SPA 路由回退 |

---

## 🔐 后台访问

**URL：** `https://theouyi.com/mgr-7a9f3c2e.html`

- 默认密码 `admin888`（首次登录后立即在「网站设置」修改）
- 主站**无任何入口**，必须手动输入
- 忘记 URL：在代码里搜 `mgr-` 能找回

---

## 🚀 部署 GitHub Pages

```bash
git init && git add . && git commit -m "Initial deploy"
git branch -M main
git remote add origin https://github.com/<用户名>/<仓库名>.git
git push -u origin main
```

仓库 → **Settings → Pages** → Source: `main` / `/ (root)`。

### DNS 配置
**A 记录（@）：**
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```
**CNAME：** `www → <用户名>.github.io`

DNS 生效后，Custom domain 填 `theouyi.com`，勾选 **Enforce HTTPS**。

---

## 🔍 SEO 优化亮点

### 多语种 SEO（本版核心）
- `hreflang` 覆盖 10 种标签：zh-CN / zh-Hans / zh-TW / zh-Hant / zh-HK / en / en-US / ko / ko-KR / x-default
- **每种语言独立的 title 和 description**，切换语言时自动更新 `<title>`、`og:title`、`og:description`、`og:locale`
- 这是让 Google 分别收录 4 语言版本的关键

### 结构化数据（6 块 JSON-LD）
- `WebSite` — 含 SearchAction
- `Organization` — 含 knowsLanguage, areaServed
- `WebPage` — 含 datePublished / dateModified
- `BreadcrumbList` — 4 级面包屑
- `FAQPage` — 5 个问答（触发 Google 富媒体摘要）
- `ItemList` — 交易所排名列表

### GEO 标签
```
geo.region: HK
geo.position: 22.3193;114.1694
ICBM: 22.3193, 114.1694
```

### 性能提示（提升核心 Web Vitals）
- preconnect: Google Fonts、OKX API
- dns-prefetch: TradingView、CoinGecko
- theme-color, color-scheme, apple-touch-icon

### robots.txt 精细化
- Googlebot / Bingbot / Yeti / Baiduspider 独立规则
- 屏蔽 AI 训练爬虫（GPTBot / CCBot / Claude-Web / ChatGPT-User）
- 限速 AhrefsBot / SemrushBot，屏蔽 MJ12bot

---

## 📈 搜索引擎提交

### 1. Google Search Console
https://search.google.com/search-console
1. 添加 `theouyi.com`（DNS TXT 验证最稳）
2. 提交 `sitemap.xml`
3. 修改 `index.html` 第 ~57 行：
```html
<meta name="google-site-verification" content="你的验证码">
```

### 2. Bing Webmaster Tools
https://www.bing.com/webmasters — 从 Google Search Console 一键导入，替换：
```html
<meta name="msvalidate.01" content="你的验证码">
```

### 3. Naver（韩国用户关键）
https://searchadvisor.naver.com
```html
<meta name="naver-site-verification" content="你的验证码">
```

### 4. Yandex（俄语市场，可选）
https://webmaster.yandex.com
```html
<meta name="yandex-verification" content="你的验证码">
```

### 5. 百度（大陆用户可选）
https://ziyuan.baidu.com
```html
<meta name="baidu-site-verification" content="你的验证码">
```

---

## ⚠️ Chrome「危险网站」警告

本版代码已经做了完整的降风险处理：
- ✅ 主站完全无后台入口
- ✅ 后台 URL 随机化 + 多重 noindex + referrer 屏蔽
- ✅ 所有交易所外链 `rel="nofollow sponsored"`
- ✅ 页脚 4 语言明确免责声明
- ✅ JSON-LD 清楚标注是独立第三方站点

部署后若仍警告，去 [Search Console 安全问题](https://search.google.com/search-console) 点「请求审核」，一般 24–72 小时解除。

---

## 📊 行情页响应式适配

| 屏幕 | 图表高度 |
|---|---|
| > 1100px（大桌面） | `clamp(440px, 72vh, 820px)` |
| 900–1100px | `clamp(440px, 68vh, 720px)` |
| 640–900px（平板） | `clamp(420px, 65vh, 620px)` |
| 400–640px（手机） | `clamp(380px, 60vh, 520px)` |
| < 400px（小手机） | `420px` 固定 |
| 横屏（≤600px 高） | `88vh` |

移动端自动：
- 隐藏 TradingView 侧边工具栏和图例
- 禁用币种切换下拉（减少误触）
- Hero 改为单列叠式布局
- 数据卡 2 列（小屏 1 列）

窗口 resize 时图表**自动重载**切换桌面/移动配置。

---

## 🌐 翻译系统（后台）

- 总开关：网站设置 → **发布时自动翻译**
- 源：Google Translate 免费端点 + MyMemory fallback
- 简中为源语言，一键翻译到 繁中/英/韩

**生产建议**：量大时接 Google Cloud Translation API（$20/百万字符），改 `mgr-7a9f3c2e.html` 中 `translateOne()`。

---

## 💰 行情数据

- 主源：`OKX /api/v5/market/tickers?instType=SPOT`（30 秒刷新）
- 备用：CoinGecko
- K 线：TradingView widget · 数据源 `OKX:BTCUSDT`

换币种：后台 → 币种管理 → 添加 `BTC-USDT` 格式 Instrument ID。

---

## 🛠️ FAQ

**Q：刷新页面 404？** `404.html` 已处理 SPA 路由回退。

**Q：数据存哪？** 浏览器 `localStorage` 键 `csp3`，后台「数据管理」导出 JSON 备份。

**Q：忘密码？** `F12 → Application → Local Storage → 删 csp3` 重置为 `admin888`（会清空所有数据，记得先导出）。

**Q：想替换 og-image？** 在根目录放一张 `og-image.png`（1200×630），社交分享时自动使用。

---

## 📄 法律合规

1. 不用"官方推荐"、"合作伙伴"、"独家返佣"等暗示词
2. 页脚免责声明 4 语言已明确无隶属关系
3. 含 affiliate 链接时显著处加「本页含推广链接」提示
4. **不得**收集访客真实密码、私钥、交易所 API Key

---

© 2026 YouYi · theouyi.com
