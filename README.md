# 律师官网静态镜像（主域名 qisuzhuang.top）

本目录是沈奕呈律师官网的静态内容镜像。构建后需执行
`python _mirror/switch_domain.py https://qisuzhuang.top`，把全站域名统一为主域名
（切换后 `CNAME` 也会自动写入 `qisuzhuang.top`）。

包含首页、关于我、业务领域、法律视角（含 3 篇文章全文）、经典案例（含 3 篇案例全文）、
在线咨询、AI 诉状助手，以及 CNAME、llms.txt、robots.txt、sitemap.xml。

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

## 内容更新流程

1. WordPress 后台（`ououifd.page.gd/wp-admin`）写文章、改页面、配置 AI 接口密钥。
2. `python _mirror\build.py` 重建镜像，再 `python _mirror\switch_domain.py https://qisuzhuang.top`。
3. 变化的文件重新上传到 GitHub 仓库覆盖提交。
4. 需要单文件版时再执行 `python _mirror\build_single.py`。

## 目录说明

- `index.html` 首页；`about.html` 关于我；`practice-areas.html` 业务领域；`insights.html` 法律视角
- `articles/*.html` 三篇文章全文；`cases/*.html` 三篇案例全文；`contact.html` 在线咨询；`ai-pleading.html` AI 诉状助手
- `assets/main.css` 与原站同一套样式；`assets/img/` 两张图片（已压缩）
- `llms.txt` 给大模型的站点说明；`robots.txt` 对 AI 爬虫全放行；`sitemap.xml` 网站地图
- `all-in-one.html` 单文件全站版，图片已内嵌，可本地直接打开
- `CNAME` GitHub Pages 自定义域名标记文件

## 注意

- 页面 `<link rel="canonical">` 指向 qisuzhuang.top 的对应页面。
- 留言表单在镜像里是静态说明，表单链接指向 WordPress 站点（静态站无表单后端）。
- AI 诉状助手在镜像里只展示界面与「待配置」状态，实际模型调用由 WordPress 后台配置控制。
