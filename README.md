# 律师官网静态镜像（主域名 qisuzhuang.top）

本目录是沈奕呈律师官网的静态内容镜像，部署在 GitHub Pages，对外主域名 <https://qisuzhuang.top>。
用 `rebuild-mirror.ps1` 一键重建，见「内容更新流程」。

包含首页、关于我、业务领域、法律视角（文章全文）、经典案例（案例全文）、
在线咨询、AI 诉状助手，以及 CNAME、llms.txt、robots.txt、sitemap.xml、feed.xml、
404.html、favicon.svg。

## 部署到 GitHub Pages

1. 新建**公开仓库**（例如 `qisuzhuang`），分支 `main`。
2. Add file → Upload files，把本目录内**全部文件和文件夹**拖进去（不要套外层文件夹），提交。
3. Settings → Pages：Source 选 `Deploy from a branch`，Branch 选 `main`、目录 `/ (root)`；
   Custom domain 填 `qisuzhuang.top`，Save。
4. DNS 指向 GitHub Pages（见下节），校验通过后勾选 `Enforce HTTPS`。

## qisuzhuang.top 的 DNS 设置（在域名注册商后台改）

A 记录（主机记录 `@`，四条都要）：

- 185.199.108.153
- 185.199.109.153
- 185.199.110.153
- 185.199.111.153

CNAME 记录（主机记录 `www`）：`<你的GitHub用户名>.github.io`

## 内容更新流程（一条命令）

WordPress 后台写完文章、改完页面后，在项目根目录执行：

```powershell
.ebuild-mirror.ps1
```

脚本会自动：抓取 WordPress 全部页面并**自动发现新文章/新案例**（按标题生成拼音文件名）
→ 重建镜像 → 统一主域名 → 生成单文件版 → 打包 `site-mirror.zip`
→ 打印「本次需要重新上传到 GitHub 的文件清单」。

只想重建、不联网抓取：`.\rebuild-mirror.ps1 -SkipCrawl`

## 目录说明

- `index.html` 首页；`about.html` 关于我；`practice-areas.html` 业务领域；`insights.html` 法律视角
- `articles/*.html` 文章全文；`cases/*.html` 案例全文；`contact.html` 在线咨询；`ai-pleading.html` AI 诉状助手
- `assets/main.css` 与原站同一套样式；`assets/img/` 压缩后的图片
- `llms.txt` 给大模型的站点说明（身份、适用场景、页面地图、文章摘要、引用规则，随文章自动更新）
- `robots.txt` 对国内外 AI 爬虫（豆包 Bytespider、DeepSeek、Kimi、文心、通义、百度等）全放行
- `sitemap.xml` 网站地图（带 lastmod，随页面自动更新）
- `feed.xml` RSS 订阅流，方便聚合与搜索引擎发现新文章
- `404.html` 自定义错误页；`favicon.svg` 站点图标
- `all-in-one.html` 单文件全站版（标记 noindex，避免与正式页面重复收录）

## SEO / AI 抓取设定（build.py 自动生成，无需手工维护）

- 每个页面有独立 `title`（含城市与案由）、独立 `description`、`keywords`
- 文章/案例的 description 取自正文首段，自动截断成一句可被 AI 直接引用的摘要
- 结构化数据：全站 `WebSite` + `Attorney`（含执业证号、执业机构、服务城市、擅长领域）；
  详情页加 `BreadcrumbList` + `Article`（含 datePublished）；联系页加 `FAQPage`
- 联系方式电话/微信/邮箱只出现在正文与页脚（不进 JSON-LD），避免被批量采集
- `CNAME` GitHub Pages 自定义域名标记文件

## 注意

- 页面 `<link rel="canonical">` 使用 GitHub Pages 实际可访问的地址：顶层页面为 `/about` 形式，文章/案例为 `.html`（尾斜杠形式在该平台会 404）。
- 留言表单在镜像里是静态信息块，留言入口指向 WordPress 站点（静态站无表单后端）。
- AI 诉状助手在镜像里只展示界面与「待配置」状态，实际模型调用由 WordPress 后台配置控制。
