# Awesome [Zellij](https://github.com/zellij-org/zellij) 中文文档

> Zellij 是一个面向开发者的现代终端复用器，提供强大的工作区管理和插件系统。

这是一个汇集 Zellij 资源的精选列表，包含插件、教程和配置设置。

所有列出的资源都是社区驱动的：我们无法提供官方支持，但非常欢迎建议和评论。

---

## 目录

- [插件](#插件) - 增强 Zellij 功能的强大工具
- [集成工具](#集成工具) - 与 Zellij 配合使用的辅助工具
- [教程](#教程) - 学习和开发资源

---

## 插件

插件是 Zellij 的核心功能之一，通过 WebAssembly 技术实现，可以为终端工作流添加各种强大功能。

### 导航与切换类插件

| 插件 | 描述 |
|------|------|
| [ghost](./plugins/ghost.md) | 生成浮动命令终端面板（交互式 zrf） |
| [harpoon](./plugins/harpoon.md) | 快速导航面板（Neovim Harpoon 的克隆） |
| [room](./plugins/room.md) | 快速搜索和切换标签页 |
| [zbuffers](./plugins/zbuffers.md) | 受 Emacs vertico-buffers 启发的标签切换工具 |
| [zellij-jump-list](./plugins/zellij-jump-list.md) | 在面板间导航跳转（类似 Vim 跳转列表） |
| [zjpane](./plugins/zjpane.md) | 轻松在面板间导航 |
| [zellij-pane-picker](./plugins/zellij-pane-picker.md) | 快速切换、收藏和跳转到面板 |

### 编辑器集成类插件

| 插件 | 描述 |
|------|------|
| [vim-zellij-navigator](./plugins/vim-zellij-navigator.md) | 在 Zellij 中与 Vim 无缝导航 |
| [zellij-nvim-nav-plugin](./plugins/zellij-nvim-nav-plugin.md) | 另一个用于与 Neovim/Vim 窗口无缝导航的插件 |
| [zellij-autolock](./plugins/zellij-autolock.md) | 根据焦点面板中的命令自动锁定 Zellij |
| [zjswitcher](./plugins/zjswitcher.md) | 在普通模式和锁定模式间自动切换 |

### 会话管理类插件

| 插件 | 描述 |
|------|------|
| [zellij-choose-tree](./plugins/zellij-choose-tree.md) | 受 tmux choose-tree 启发的快速会话切换 |
| [zellij-favs](./plugins/zellij-favs.md) | 保存收藏会话并清除其他会话 |
| [zellij-sessionizer](./plugins/zellij-sessionizer.md) | 基于文件夹名称创建会话 |
| [zellij-switch](./plugins/zellij-switch.md) | 使用 `zellij pipe` 在 CLI 中切换会话 |
| [zellij-workspace](./plugins/zellij-workspace.md) | 将布局应用到当前会话 |
| [zsm](./plugins/zsm.md) | 支持默认布局的 zoxide 集成会话切换器 |

### 搜索与查找类插件

| 插件 | 描述 |
|------|------|
| [grab](./plugins/grab.md) | 面向 Rust 开发者的模糊查找器 |
| [monocole](./plugins/monocole.md) | 文件名和内容的模糊查找 |

### 开发与构建类插件

| 插件 | 描述 |
|------|------|
| [gitpod.zellij](./plugins/gitpod.zellij.md) | Gitpod 插件，集成 .gitpod.yml 任务 |
| [jbz (Just Bacon Zellij)](./plugins/jbz.md) | 在 bacon 中包装显示 just 命令 |
| [multitask](./plugins/multitask.md) | 作为 Zellij 插件的迷你 CI |
| [zellij-playbooks](./plugins/zellij-playbooks.md) | 浏览、选择和执行剧本文件中的命令 |

### 系统信息类插件

| 插件 | 描述 |
|------|------|
| [zellij-datetime](./plugins/zellij-datetime.md) | 在 Zellij 中添加日期和时间面板 |
| [zellij-what-time](./plugins/zellij-what-time.md) | 在状态栏显示主机系统日期/时间 |
| [zellij-load](./plugins/zellij-load.md) | 显示系统资源（CPU、内存、GPU 使用率） |
| [zj-docker](./plugins/zj-docker.md) | 显示 Docker 容器并执行基本操作 |

### Git 集成类插件

| 插件 | 描述 |
|------|------|
| [zj-git-branch](./plugins/zj-git-branch.md) | 管理 Git 分支 |

### 实用工具类插件

| 插件 | 描述 |
|------|------|
| [zellij-bookmarks](./plugins/zellij-bookmarks.md) | 管理命令书签并快速插入终端 |
| [zellij-forgot](./plugins/zellij-forgot.md) | 快速展示和访问你的键绑定 |
| [zellij-getmode](./plugins/zellij-getmode.md) | 获取 Zellij 当前输入模式的简单工具 |
| [zellij-notepad](./plugins/zellij-notepad.md) | 带可配置编辑器、位置和时间戳的浮动记事本 |
| [zellij-qr-share](./plugins/zellij-qr-share.md) | 在终端中将 Web 令牌显示为 QR 码 |
| [zellij-newtab-plus](./plugins/zellij-newtab-plus.md) | 创建命名标签页并使用 zoxide 导航 |

### 状态栏与界面类插件

| 插件 | 描述 |
|------|------|
| [zellij-cb](./plugins/zellij-cb.md) | 可定制的紧凑型状态栏 |
| [zellij-tab-bar-indexed](./plugins/zellij-tab-bar-indexed.md) | 为标签添加数字索引以便快速导航 |
| [zj-status-bar](./plugins/zj-status-bar.md) | compact-bar 插件的意见分支 |
| [zj-quit](./plugins/zj-quit.md) | Zellij 的友好退出插件 |
| [zjstatus](./plugins/zjstatus.md) | 可配置、可主题化的状态栏插件 |
| [zjstatus-hints](./plugins/zjstatus-hints.md) | 为 zjstatus 添加模式感知的键绑定提示 |

---

## 集成工具

这些工具与 Zellij 配合使用，提供更强大的工作流。

### Shell 集成

| 工具 | 描述 |
|------|------|
| [fzf-zellij](./integrations.md#fzf-zellij) | 在 Zellij 浮动面板中启动 fzf 的 shell 脚本 |
| [zellij-sessionizer](./integrations.md#zellij-sessionizer) | 受 tmux-sessionizer 启发的模糊查找项目切换器 |

### IDE 工作流

| 工具 | 描述 |
|------|------|
| [theylix](./integrations.md#theylix) | Zellij、Helix 和各种 CLI 工具构成的禅模式 IDE |
| [yazelix](./integrations.md#yazelix) | 为 Helix 添加文件树和 Zellij 友好键绑定 |
| [zide](./integrations.md#zide) | 创建类 IDE 文件选择器和编辑器工作流 |

### AI 与自动化

| 工具 | 描述 |
|------|------|
| [opencode-zellij-namer](./integrations.md#opencode-zellij-namer) | 基于项目上下文的 AI 动态会话命名 |

### 远程协作

| 工具 | 描述 |
|------|------|
| [zeco](./integrations.md#zeco) | 在互联网上轻松安全地共享 Zellij 会话 |

### 编辑器包装器

| 工具 | 描述 |
|------|------|
| [zellix](./integrations.md#zellix) | 利用 Zellij 将 Helix 变成插件系统的 Nushell 包装器 |

---

## 教程

### 基础教程

* [Zellij 基础功能](https://zellij.dev/tutorials/basic-functionality/) - 官方基础教程
* [使用和创建布局](https://zellij.dev/tutorials/layouts/) - 掌握 Zellij 布局系统

### 插件开发

* [Zellij 插件开发全面指南](https://github.com/Kangaxx-0/first-zellij-plugin) - 从零开始开发插件
* [开发 Zellij 插件的经验教训](https://blog.nerd.rocks/posts/profiling-zellij-plugins/) - zjstatus 创建者的博客文章
* [为 Zellij 开发 WebAssembly Rust 插件](https://www.youtube.com/watch?v=pgNIcQ8rTXk) - Aram Drevekenin 在 RustLab 2023 的演讲
* [如何使用 Zellij Switch 插件](https://mostafaqanbaryan.com/zellij-attach-plugin/) - 关于使用 fzf 和 zoxide 的教程

### 文档与支持

* [Zellij 官方文档](https://zellij.dev/documentation/introduction) - 完整的参考文档
* [Discord 社区](https://discord.com/invite/CrUAFH3) - 实时讨论和问答
* [Matrix 聊天室](https://matrix.to/#/#zellij_general:matrix.org) - 开源即时通讯平台

---

## 贡献指南

我们欢迎社区贡献！如果你有优秀的 Zellij 插件、工具或教程想要分享，请提交 PR。

---

## 许可证

本 awesome 列表采用 [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/) 许可证。

---

*最后更新：2025年2月*
