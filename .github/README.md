# GitHub 工作流说明

本目录包含此仓库的 GitHub Actions 工作流配置。

## 工作流列表

### 1. Sync upstream (main.yml)

**目的**: 自动同步上游仓库 [dnsviz/dnsviz](https://github.com/dnsviz/dnsviz) 的更新。

**触发条件**:
- **定时执行**: 每天 UTC 时间 03:00（北京时间 11:00）自动运行
- **手动触发**: 可在 GitHub Actions 页面手动触发

**功能说明**:
1. 检出本地仓库
2. 添加上游仓库为远程源
3. 检查上游是否有新的提交
4. 如果有更新，自动合并到本地 master 分支
5. 推送更新到本仓库
6. 如果合并时发生冲突，自动创建 Issue 通知维护者

**权限要求**:
- `contents: write` - 用于推送更新到仓库

**注意事项**:
- 如果发生合并冲突，工作流会自动创建一个带有 `upstream-sync-conflict` 标签的 Issue
- 需要手动解决冲突后，Issue 应被关闭
- 同一时间只会创建一个冲突 Issue，避免重复通知

### 2. Build and Test DNSViz (python-package.yml)

**目的**: 在多个 Python 版本和操作系统上构建和测试 DNSViz。

**触发条件**:
- 推送到 master 分支
- 针对 master 分支的 Pull Request

**功能说明**:
- 在 Ubuntu 和 macOS 上测试
- 支持 Python 3.9、3.10、3.11、3.12
- 运行单元测试和功能测试

## 手动触发工作流

要手动触发 upstream sync 工作流：

1. 访问仓库的 Actions 页面
2. 选择 "Sync upstream" 工作流
3. 点击 "Run workflow" 按钮
4. 选择分支（通常是 master）
5. 点击 "Run workflow" 确认

## 问题排查

如果上游同步失败：

1. 检查 Actions 日志了解失败原因
2. 查看是否有自动创建的 Issue
3. 手动同步：
   ```bash
   git remote add upstream https://github.com/dnsviz/dnsviz.git
   git fetch upstream
   git merge upstream/master
   # 解决任何冲突
   git push origin master
   ```

## 配置说明

上游同步工作流的关键配置：

- **上游仓库**: `https://github.com/dnsviz/dnsviz.git`
- **同步分支**: `master`
- **执行时间**: 每天 UTC 03:00（可在 main.yml 中修改 cron 表达式）
