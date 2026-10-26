# Repository Guidelines

## 项目结构与贡献入口

本仓库维护 Shadowrocket 分流与广告过滤规则。当前 `release` 分支主要存放生成产物：根目录 `sr_*.conf` 为各类规则，`lazy.conf`、`lazy_group.conf` 为懒人配置；`figure/` 保存二维码和指南图片；`readme.md` 说明规则选择与导入方式；`.github/workflows/` 管理发布与 Issue 清理。

生成源码位于 `build` 分支的 `factory/`，常规规则贡献应修改其中三个 `manual_*.txt` 文件。优先修正生成输入，避免修改会被下次发布覆盖的产物。

## 构建与开发命令

在干净工作区中获取 `build`，创建贡献分支：

```bash
git fetch origin build
git switch -c fix/rules FETCH_HEAD
```

切换后，在 Linux 或 WSL 中安装 Python 依赖并运行与发布工作流相同的构建入口：

```bash
python -m pip install -r requirements.txt
bash factory/auto_build.sh
```

提交前运行 `git diff --check` 检查空白错误，运行 `git diff --stat` 核对修改范围。当前 `release` 不含构建源码或依赖清单，不能直接执行上述构建命令。

## 格式与命名约定

所有新增或修改文件使用 UTF-8 无 BOM；说明和注释使用中文，保留命令与规则关键字的原文。`.conf` 规则顶格书写，每行一条，沿用 `DOMAIN-SUFFIX,example.com,REJECT` 等逗号分隔格式及 `key = value` 配置风格。保留分节、规则顺序和兜底策略，避免全文件排序或无关格式调整。

沿用 `sr_*.conf`、`lazy*.conf` 命名，配套二维码沿用配置文件名。YAML 缩进与邻近内容一致，通常每级两空格。当前分支未提供格式化或 lint 工具配置。

## 验证要求

当前发布快照未提供测试目录、自动测试框架或覆盖率门槛。生成后检查受影响配置的差异，确认目标域名、策略和规则顺序，并检查相关含广告过滤与不含广告过滤的版本。

在 Shadowrocket 导入受影响配置，重新连接并通过日志核对直连、代理和广告过滤行为。PR 中记录配置名、复现步骤及验证结果；若无法进行设备验证，应明确说明。

## 提交与 Pull Request

当前发布历史仅见自动提交 `Nightly build`，无法据此确定人工提交规范。人工提交使用简短、具体的祈使句，例如 `Fix routing for example.com`，每次聚焦一个问题。

PR 目标为 `build` 分支。描述应包含受影响域名或应用、原有行为、预期策略、修改原因、验证结果及相关 Issue。修改说明或二维码时同步更新对应文档或图片；仅在有助于复现时附截图。
