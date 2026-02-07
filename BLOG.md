# 使用 AI 助手 opencode 从零到上线：完整项目开发实录

## 前言

在 AI 时代，软件开发的方式正在发生革命性的变化。本文将记录我如何使用 AI 助手 **opencode** 从零开始，完成一个完整的项目开发流程：从创建目录、编写代码、建立 GitHub 仓库、部署上线，到最终撰写并发布博客的全过程。

## 什么是 opencode？

opencode 是一个智能的 CLI 工具，可以通过自然语言对话帮助开发者完成各种软件工程任务。它集成了代码编辑、文件操作、版本控制、项目管理等多种功能，让开发工作更加高效。

## 项目概述

**项目名称**：贪食蛇游戏  
**开发方式**：全程使用 opencode AI 助手  
**开发时长**：约 30 分钟  
**项目地址**：https://github.com/AlexByYao/snake-game-ai  
**在线演示**：https://alexbyyao.github.io/snake-game-ai

## 使用 opencode 的完整流程

### 第一步：初始化项目环境

首先，我通过 opencode 创建了一个新目录：

```
用户：帮我新建一个目录，名字叫myopencode
opencode：mkdir myopencode
```

**opencode 优势**：简单的自然语言指令，自动转换为系统命令。

### 第二步：开发游戏代码

接下来，我让 opencode 开发贪食蛇游戏。通过描述需求，opencode 自动生成了完整的 HTML5 游戏代码：

```
用户：帮我开发一个贪食蛇的游戏
opencode：
- 创建 index.html
- 编写 Canvas 游戏逻辑
- 添加 CSS 样式
- 实现游戏核心功能
```

**关键点**：opencode 不仅生成代码，还自动处理了：
- 游戏循环机制
- 键盘事件监听
- 碰撞检测逻辑
- 视觉效果（渐变、发光、毛玻璃）
- 本地存储最高分

### 第三步：初始化版本控制

代码完成后，opencode 自动初始化 Git 仓库并提交：

```bash
# opencode 自动执行
git init
git add .
git commit -m "初始提交: 添加贪食蛇游戏"
```

**智能之处**：opencode 自动生成有意义的提交信息，遵循 Git 最佳实践。

### 第四步：GitHub 集成

这是 opencode 最强大功能之一——完整的 GitHub 工作流支持。

#### 4.1 安装 GitHub CLI

当需要推送到 GitHub 时，opencode 发现缺少 GitHub CLI 工具，自动下载并安装：

```bash
# opencode 自动下载并安装 GitHub CLI
curl -L -o gh.zip "https://github.com/cli/cli/releases/download/v2.63.2/gh_2.63.2_windows_amd64.zip"
powershell -Command "Expand-Archive -Path 'gh.zip' -DestinationPath 'gh-cli' -Force"
```

#### 4.2 身份认证

opencode 引导完成 GitHub 认证流程：

```
opencode：GitHub CLI 安装成功！现在需要登录GitHub账号：
- 自动下载 GitHub CLI
- 引导用户获取访问令牌
- 支持令牌方式认证（避免交互式登录）
```

**安全提示**：所有敏感信息（令牌、密码）都通过交互式输入，不会记录在代码或日志中。

#### 4.3 自动创建仓库并推送

```bash
# opencode 一键完成
cd myopencode
../gh-cli/bin/gh.exe repo create snake-game-ai --public --source=. --remote=origin --push
```

**结果**：自动创建 GitHub 仓库、关联远程地址、推送代码，全程无需手动干预。

### 第五步：部署到生产环境

#### 5.1 启用 GitHub Pages

opencode 尝试通过 API 自动启用 Pages：

```bash
gh api repos/AlexByYao/snake-game-ai/pages --method POST ...
```

虽然 API 调用因权限格式问题失败，但 opencode 立即提供手动步骤：

```
opencode：需要在GitHub上手动启用Pages功能。请按以下步骤操作：
1. 访问 https://github.com/AlexByYao/snake-game-ai/settings/pages
2. Source 选择 master 分支，/(root) 目录
3. 点击 Save
```

#### 5.2 验证部署

用户完成手动设置后，opencode 确认部署成功：

```
opencode：完美！您的贪食蛇游戏已成功部署！
在线游玩地址：https://alexbyyao.github.io/snake-game-ai
```

### 第六步：撰写并发布博客

#### 6.1 自动生成博客内容

opencode 根据项目开发过程，自动生成博客草稿：

```
用户：帮我写篇博客并发布，记录整个制作过程
opencode：
- 分析项目开发流程
- 生成 BLOG.md 文件
- 自动移除所有敏感信息
- 提交到本地仓库
```

#### 6.2 处理发布问题

当博客推送遇到网络问题时，opencode 展现了解决问题的能力：

**问题 1**：Git 推送需要交互式密码输入
```
尝试 1：使用 gh CLI 自动认证
尝试 2：配置 git credential helper
尝试 3：直接调用 GitHub API
```

**问题 2**：网络连接不稳定
```
尝试 1：git push（超时）
尝试 2：使用 gh repo sync（格式错误）
解决方案：通过 GitHub REST API 直接上传文件
```

**最终方案**：
```bash
# 使用 GitHub API 直接上传
gh api repos/AlexByYao/snake-game-ai/contents/BLOG.md \
  --method PUT \
  -f message="添加开发博客" \
  -f content="$(base64 -w 0 BLOG.md)"
```

**成功**：博客成功发布到 GitHub。

## opencode 的核心能力分析

通过这个项目，我深刻体会到 opencode 的以下能力：

### 1. **自然语言理解**
- 理解模糊的需求描述
- 自动推断具体实现步骤
- 上下文记忆和连贯性

### 2. **多工具集成**
- 文件系统操作（mkdir, write, read）
- 版本控制（git init, commit, push）
- 第三方工具（GitHub CLI, curl, API）
- 系统命令执行

### 3. **问题诊断与解决**
- 自动检测缺失的依赖（GitHub CLI）
- 多种方案尝试（命令行、API、交互式）
- 优雅降级（自动提供手动步骤）

### 4. **安全与隐私**
- 敏感信息（令牌、密码）不记录到文件
- 交互式输入避免泄露
- 自动清理日志中的敏感数据

### 5. **工作流自动化**
- 一键式仓库创建和推送
- 自动提交和推送
- 状态检查和反馈

## 完整命令时间线

以下是 opencode 在本项目中执行的所有操作的时间线：

```
1. mkdir myopencode                    - 创建项目目录
2. write index.html                    - 生成游戏代码
3. git init                            - 初始化仓库
4. write README.md                     - 创建文档
5. git add/commit                      - 提交代码
6. download gh-cli                     - 安装GitHub CLI
7. gh auth login                       - GitHub认证
8. gh repo create                      - 创建远程仓库
9. git push                            - 推送代码
10. write BLOG.md                      - 编写博客
11. git add/commit/push               - 推送博客（遇到网络问题）
12. gh api (REST API)                 - 通过API上传博客（成功）
```

## 与传统开发方式的对比

| 环节 | 传统方式 | 使用 opencode |
|------|---------|--------------|
| 环境搭建 | 手动执行多个命令 | 一句话完成 |
| 代码编写 | 数小时编码 | 几分钟生成 |
| 版本控制 | 手动 git 操作 | 自动执行 |
| GitHub 集成 | 网页操作 + 命令行 | 一键完成 |
| 问题排查 | 搜索 + 试错 | AI 自动诊断解决 |
| 文档编写 | 手动撰写 | 自动生成 |

**效率提升**：整个项目从创建到上线，使用 opencode 仅需 30 分钟，而传统方式可能需要 2-3 小时。

## 经验与教训

### 成功的经验

1. **清晰的指令**：提供明确、具体的需求描述
2. **迭代优化**：根据结果不断调整指令
3. **信任 AI**：让 opencode 自主处理技术细节
4. **及时确认**：在关键步骤确认结果，避免错误累积

### 遇到的挑战

1. **网络限制**：部分 API 调用受网络环境影响
2. **权限配置**：GitHub Token 需要正确的权限范围
3. **交互限制**：某些操作需要手动确认（如浏览器登录）

### 最佳实践

1. **分步确认**：复杂任务分解为多个小步骤
2. **权限预设**：提前准备带有足够权限的访问令牌
3. **备用方案**：准备手动操作的备选步骤
4. **安全注意**：敏感信息通过交互式输入，避免硬编码

## 总结

通过 opencode，我体验到了 AI 辅助开发的强大能力。从项目初始化到部署上线，再到文档发布，整个过程流畅高效。opencode 不仅是一个代码生成工具，更是一个全面的软件开发助手，能够：

- 理解自然语言需求
- 自动选择和执行合适的工具
- 诊断和解决技术问题
- 保证安全性和隐私
- 提供优雅的降级方案

## 展望未来

随着 AI 技术的不断发展，像 opencode 这样的工具将会越来越强大。未来的软件开发可能更多是与 AI 协作，开发者专注于创意和架构设计，而具体的实现细节由 AI 自动处理。

这不仅是效率的提升，更是开发方式的革命。

---

**项目成果**：
- 🎮 游戏地址：https://alexbyyao.github.io/snake-game-ai
- 📁 源码仓库：https://github.com/AlexByYao/snake-game-ai  
- 📝 开发博客：https://github.com/AlexByYao/snake-game-ai/blob/master/BLOG.md

**使用工具**：opencode AI 助手  
**开发时间**：2026年2月7日  
**总耗时**：约 30 分钟

---

*本文档由 opencode AI 助手协助生成*  
*感谢 AI 技术让开发变得更加简单高效*
