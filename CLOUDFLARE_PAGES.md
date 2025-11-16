# DNSViz - Cloudflare Pages 部署指南

本文档说明如何将 DNSViz 项目部署到 Cloudflare Pages。

## 关于此部署

这是一个静态网站部署，提供：
- DNSViz 项目信息和文档
- 安装和使用说明
- 链接到官方在线工具和 GitHub 仓库

**注意**：DNSViz 本身是一个命令行工具，此部署仅提供信息展示页面，不包含实际的 DNS 分析功能。

## 部署到 Cloudflare Pages

### 方法 1: 通过 Cloudflare Pages 控制面板

1. 登录到 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 进入 **Pages** 部分
3. 点击 **创建项目**
4. 选择 **连接到 Git**
5. 授权并选择此 GitHub 仓库
6. 配置构建设置：
   - **项目名称**: `dnsviz` (或您选择的名称)
   - **生产分支**: `main` 或 `master`
   - **构建命令**: (留空)
   - **构建输出目录**: `public`
7. 点击 **保存并部署**

### 方法 2: 使用 Wrangler CLI

如果您已安装 Wrangler CLI：

```bash
# 安装 Wrangler (如果尚未安装)
npm install -g wrangler

# 登录 Cloudflare
wrangler login

# 部署到 Cloudflare Pages
wrangler pages deploy public --project-name=dnsviz
```

## 文件结构

```
public/
├── index.html          # 主页面
├── dnsviz-graph.html   # DNSViz 图形文档
├── images/             # 文档图片
├── _headers            # Cloudflare Pages 头部配置
└── _redirects          # URL 重定向规则
```

## 自定义

### 修改页面内容

编辑 `public/index.html` 文件以自定义主页内容。

### 添加自定义域名

1. 在 Cloudflare Pages 项目设置中
2. 进入 **自定义域** 部分
3. 添加您的域名

### 更新安全头部

编辑 `public/_headers` 文件以调整安全策略。

## 本地测试

您可以使用任何静态文件服务器在本地测试：

```bash
# 使用 Python
cd public
python -m http.server 8000

# 使用 Node.js http-server
npm install -g http-server
cd public
http-server

# 使用 PHP
cd public
php -S localhost:8000
```

然后访问 `http://localhost:8000`

## 更新部署

每次推送到配置的分支时，Cloudflare Pages 会自动重新部署。

## 故障排除

### 页面显示 404
- 确认构建输出目录设置为 `public`
- 检查 `public/index.html` 文件是否存在

### 资源加载失败
- 检查 `_headers` 文件中的 CORS 设置
- 确认所有资源路径使用相对路径

### 部署失败
- 检查 Cloudflare Pages 构建日志
- 确认所有必需文件都已提交到 Git

## 许可证

DNSViz 是自由软件，遵循 GNU 通用公共许可证 v2+ 发布。

## 链接

- [DNSViz GitHub](https://github.com/dnsviz/dnsviz)
- [DNSViz 官方网站](https://dnsviz.net/)
- [Cloudflare Pages 文档](https://developers.cloudflare.com/pages/)
