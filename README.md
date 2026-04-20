# 有易 YouYi - 部署指南

> 4 语言（简中 / 繁中 / English / 한국어）加密货币教程 SPA · 独立后台 + 自动翻译

---

## 📦 文件清单

| 文件 | 作用 |
|---|---|
| `index.html` | 主站（单页应用，4 语言） |
| `mgr-7a9f3c2e.html` | **后台管理**（URL 不公开，不在主站显示任何入口） |
| `CNAME` | 绑定域名 `theouyi.com` |
| `robots.txt` | 禁止爬虫索引所有后台路径 |
| `sitemap.xml` | 4 语言 hreflang 站点地图 |
| `404.html` | SPA 路由回退 |

---

## 🔐 后台访问方式（非常重要，请记牢）

后台 URL：**`https://theouyi.com/mgr-7a9f3c2e.html`**

- 主站**没有任何按钮、彩蛋、快捷键**可以进入后台
- 不在 sitemap.xml 中，`robots.txt` 全面禁爬
- 元标签 `noindex, nofollow, noarchive, nosnippet` + `referrer: no-referrer` 防索引泄漏
- 默认密码：`admin888`（**首次登录后立即在「网站设置」→「修改密码」中改掉**）

如需进一步加强：把 `mgr-7a9f3c2e.html` 再改成更长随机串（如 `mgr-a7f3c8e2d1b9.html`）。

---

## 🚀 部署到 GitHub Pages

### 1. 上传代码
```bash
git init
git add .
git commit -m "Initial deploy"
git branch -M main
git remote add origin https://github.com/<用户名>/<仓库名>.git
git push -u origin main
```

### 2. 启用 Pages
仓库 → **Settings → Pages**：Source 选 `main` 分支 `/ (root)`。

### 3. DNS 配置

**A 记录（@）：**
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**CNAME 记录：** `www → <用户名>.github.io`

DNS 生效后，Settings → Pages → Custom domain 填 `theouyi.com`，勾选 **Enforce HTTPS**。

---

## ⚠️ 解决 Chrome「危险网站」警告

### 1. 本版本已完成的修复
- ✅ 主站完全移除后台入口（无彩蛋、无快捷键、无 hash 跳转）
- ✅ 后台 URL 随机化 + 多重 noindex + referrer 屏蔽
- ✅ 所有交易所外链 `rel="nofollow sponsored"`
- ✅ 页脚 4 语言明确免责声明
- ✅ 后台页标题改英文，去品牌关键词

### 2. 部署后去 Google 申诉

[**Google Search Console**](https://search.google.com/search-console) →
1. 添加并验证 `theouyi.com`（推荐 DNS TXT 验证）
2. 左侧菜单 → **安全问题** (Security Issues)
3. 点击 **请求审核** (Request Review)
4. 申诉文字模板：
```
This site is an independent third-party tutorial website for
cryptocurrency exchanges. We have removed all potential triggers:
- No password input visible on public pages
- All external affiliate links marked rel="nofollow sponsored"
- Clear disclaimer that we are not affiliated with any exchange
- Admin page uses obscure URL with full noindex directives
Please re-review. Thank you.
```
5. 一般 **24–72 小时**解除

### 3. 检查域名历史
- [VirusTotal](https://www.virustotal.com) 查信誉
- [Safe Browsing 透明度报告](https://transparencyreport.google.com/safe-browsing/search)
- [Wayback Machine](https://web.archive.org) 查历史页面

如果 `theouyi.com` 以前被做过钓鱼站，申诉时附购买凭证说明"新所有者"。

---

## 🔍 搜索引擎提交

### Google Search Console
1. https://search.google.com/search-console 添加 `theouyi.com`
2. 站点地图 → 添加 `sitemap.xml`
3. 修改 `index.html` 第 11 行左右：
```html
<meta name="google-site-verification" content="你的验证码">
```

### Bing Webmaster Tools
1. https://www.bing.com/webmasters
2. 从 Google Search Console 一键导入
3. 修改 `index.html`：
```html
<meta name="msvalidate.01" content="你的验证码">
```

### Naver（韩国用户必做）
https://searchadvisor.naver.com — 对韩语版收录有帮助

---

## 🌐 翻译系统

- 后台「网站设置」→ 总开关 **发布时自动翻译**
- 翻译源：Google Translate 免费端点 + MyMemory fallback
- 简中为源语言，可翻到 繁中/英/韩
- 编辑器每语言字段旁独立 🌐 按钮

**生产环境建议**接 Google Cloud Translation API 正版（$20/百万字符），修改 `mgr-7a9f3c2e.html` 的 `translateOne()` 函数即可。

---

## 💰 行情数据

- 主源：OKX `/api/v5/market/tickers?instType=SPOT`，备用 CoinGecko
- 30 秒刷新
- K 线：TradingView，数据源 `OKX:BTCUSDT`

换币种：后台 → 币种管理 → 添加 `BTC-USDT` 格式 Instrument ID。

---

## 🛠️ 常见问题

**Q：刷新页面 404？** `404.html` 已处理 SPA 路由。

**Q：数据存哪？** 浏览器 `localStorage`（键 `csp3`）。定期在后台「数据管理」导出 JSON 备份。

**Q：忘记密码？** 清 localStorage 的 `csp3` 键恢复默认 `admin888`，**注意会清空所有文章和设置**。

**Q：想换后台 URL？** 直接重命名 `mgr-7a9f3c2e.html`，同步更新书签。

---

## 📄 合规建议

1. 不要用"官方推荐"、"合作伙伴"、"独家返佣"等暗示词
2. 免责声明 4 语言已写明无隶属关系
3. 含 affiliate 链接的话，显著处加"本页含推广链接"提示
4. 本站**不得**收集访客真实密码、私钥、交易所 API Key

---

© 2026 有易 YouYi · theouyi.com
