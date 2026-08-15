# 生信助力科技有限公司网站

## 项目结构

### 布局文件 (layouts/)
```
layouts/
├── index.html                    # 主页面布局（仅组装各区块 partial）
├── _default/
│   ├── baseof.html              # 页面骨架（head/meta/SEO/结构化数据）
│   └── taxonomy.html            # 分类/标签列表页模板
├── partials/
│   ├── header.html              # 导航栏组件
│   ├── footer.html              # 页脚组件
│   ├── sections/                # 页面区块组件
│   │   ├── hero.html           # 英雄区域
│   │   ├── about.html          # 关于我们
│   │   ├── services.html       # 核心服务（数据驱动）
│   │   └── contact.html        # 联系我们（数据驱动）
│   └── components/              # 可复用组件
│       ├── service-card.html    # 服务卡片组件
│       └── contact-item.html    # 联系信息组件
```

### 数据文件 (data/)
```
data/
├── services.yaml                # 服务信息数据（页面服务区块唯一内容来源）
└── contact.yaml                 # 联系信息数据（页面联系区块唯一内容来源）
```

### 前端资源 (src/)
```
src/
├── main.js                      # 入口：Bootstrap JS、平滑滚动等页面交互
└── styles.scss                  # 样式（Bootstrap SCSS + Font Awesome + 自定义主题变量）
```

### 脚本 (scripts/)
```
scripts/
└── update.sh                    # 服务器部署更新脚本（拉取更新 + 安装依赖 + 构建）
```

### 静态资源 (static/)
```
static/
├── favicon.ico                  # 站点图标（.ico 格式）
├── favicon.svg                  # 站点图标（SVG 格式）
└── images/                      # 图片资源（logo 多格式、微信二维码）
    ├── BioinfoBoost-logo.svg
    ├── BioinfoBoost-logo.png
    ├── BioinfoBoost-logo.white.png
    ├── BioinfoBoost-logo-solid.png
    ├── BioinfoBoost-logo-solid.white.png
    └── wechat-qrcode.png
```

### 根目录配置文件
```
hugo.toml         # 站点配置（baseURL/title/description/email/address/keywords）
vite.config.mjs   # 前端构建配置（产物输出到 static/assets/）
package.json      # npm 依赖与脚本
CNAME             # 自定义域名（bioinfo.app）
```

## 设计说明

### 1. 模块化设计
- 页面内容按逻辑关系拆分为 partial 文件，主布局只负责组装
- 服务、联系信息由 YAML 数据文件驱动，页面模板只负责渲染

### 2. 单一内容来源
- 服务卡片内容：只改 `data/services.yaml`
- 联系信息：只改 `data/contact.yaml`
- 站点级信息（title/description/email/address/keywords）：只改 `hugo.toml`
- 页面区块的静态文案：改对应的 `layouts/partials/sections/*.html`

### 3. 前端构建
- 前端资源由 Vite 构建到 `static/assets/`（已 gitignore，勿直接编辑）
- 依赖本地化：Bootstrap、Font Awesome 均从 npm 打包，无外部 CDN 依赖
- 中文字体优先使用 Noto Sans SC，回退到系统字体栈（PingFang SC / Microsoft YaHei 等），不加载外部字体

## 使用说明

### 本地开发
```bash
npm install
npm run dev        # 构建资源并启动 Hugo 开发服务器
```

### 生产构建
```bash
npm run build      # Vite 构建资源 + Hugo 构建站点到 public/
```

### npm 脚本一览
- `npm run build` — Vite 构建资源 + Hugo 构建站点（压缩）到 `public/`
- `npm run dev` — 构建资源并启动 Hugo 开发服务器
- `npm run build:assets` — 仅构建前端资源
- `npm run dev:assets` — 监听模式构建前端资源（配合 `hugo server` 使用）
- `npm run preview` — 本地预览 Vite 构建产物
- `npm run upgrade` — 升级 npm 依赖（npm-check-updates）

### 服务器部署更新
在部署服务器上运行 `scripts/update.sh`：
- 拉取远端最新代码（工作区存在未提交修改时中止）
- 安装依赖（`npm ci`）并执行构建（`npm run build`），产物输出到 `public/`
- `-q, --quiet`：无更新需要时不输出
- `-f, --force`：即使代码已是最新也强制安装与构建

### 添加新服务
1. 在 `data/services.yaml` 中添加服务信息
2. 服务会自动显示在页面上

### 修改联系信息
1. 在 `data/contact.yaml` 中修改联系信息
2. 联系信息会自动更新

### 添加新页面区块
1. 在 `layouts/partials/sections/` 中创建新的 section 文件
2. 在主布局文件中引用新的 section

## 样式规范

### 颜色变量
- 颜色主题通过 CSS 变量定义（`src/styles.scss` 的 `:root`，如 `--primary-color`、`--dark-bg`）
- 深色主题：背景 `#0f1419`，主色 `#00d4aa`，强调色 `#6c5ce7`

### 响应式设计
- 使用 Bootstrap 的栅格系统
- 遵循移动优先的设计原则
- 使用 Bootstrap 的断点进行适配

### 组件样式
- 使用语义化的类名
- 保持样式的一致性
- 优先使用 Bootstrap 类名，必要时才自定义
