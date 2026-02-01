# Zellij 集成工具

本文档介绍与 Zellij 配合使用的各种集成工具，它们可以扩展 Zellij 的功能或提供特定场景下的解决方案。

---

## 目录

- [Shell 集成](#shell-集成)
- [IDE 工作流](#ide-工作流)
- [AI 与自动化](#ai-与自动化)
- [远程协作](#远程协作)
- [编辑器包装器](#编辑器包装器)

---

## Shell 集成

### fzf-zellij

**GitHub**: [k-kuroguro/fzf-zellij](https://github.com/k-kuroguro/fzf-zellij)

**描述**：在 Zellij 浮动面板中启动 fzf 的 Shell 脚本

**功能特点**：
- 将 fzf 集成到 Zellij 浮动面板
- 支持文件查找、历史搜索等
- 与 Zellij 无缝集成

**使用场景**：
- 文件模糊查找
- 命令历史搜索
- Git 分支选择

**安装**：
```bash
git clone https://github.com/k-kuroguro/fzf-zellij.git
# 按照仓库说明安装
```

**使用**：
```bash
# 在 Zellij 中使用 fzf
fzf-zellij
```

### zellij-sessionizer (victor-falcon)

**GitHub**: [victor-falcon/zellij-sessionizer](https://github.com/victor-falcon/zellij-sessionizer)

**描述**：受 ThePrimeagen/tmux-sessionizer 启发的模糊查找项目切换器

**功能特点**：
- 使用 fzf 选择项目
- 自动创建/切换会话
- 基于文件夹的项目管理

**使用场景**：
- 多项目快速切换
- 项目管理
- 工作流标准化

**安装**：
```bash
git clone https://github.com/victor-falcon/zellij-sessionizer.git
# 安装到 PATH
```

---

## IDE 工作流

### theylix

**Codeberg**: [hobgoblina/theylix](https://codeberg.org/hobgoblina/theylix)

**描述**：Zellij、Helix 和各种 CLI 工具构成的禅模式 IDE

**包含工具**：
- Yazi（文件管理器）
- Lazygit（Git UI）
- LazySQL（SQL 客户端）
- Slumber（API 客户端）
- Serpl（搜索替换）
- Tig（Git  blame）

**功能特点**：
- 一体化开发环境
- 禅模式专注体验
- 键盘驱动操作

**适用人群**：
- Helix 编辑器用户
- 终端 IDE 爱好者
- 追求简洁的开发者

### yazelix

**GitHub**: [luccahuguet/yazelix](https://github.com/luccahuguet/yazelix)

**描述**：为 Helix 添加文件树和 Zellij 友好键绑定的组合

**功能特点**：
- 集成 Yazi 文件管理器
- Nushell 支持
- 文件树侧边栏
- Helix 友好的 Zellij 键绑定

**组件**：
- Zellij（终端复用）
- Yazi（文件管理）
- Helix（编辑器）
- Nushell（Shell）

**安装**：
```bash
git clone https://github.com/luccahuguet/yazelix.git
# 按照仓库说明配置
```

### zide

**GitHub**: [josephschmitt/zide](https://github.com/josephschmitt/zide)

**描述**：Zellij 布局 + bash 脚本创建类 IDE 文件选择器和编辑器工作流

**功能特点**：
- 类 IDE 文件选择器
- 支持多种文件选择器（fzf、fd 等）
- 与大多数 Shell 兼容
- 与大多数编辑器兼容

**使用场景**：
- IDE 式文件浏览
- 快速文件切换
- 项目管理

**安装**：
```bash
git clone https://github.com/josephschmitt/zide.git
# 按照仓库 README 配置
```

---

## AI 与自动化

### opencode-zellij-namer

**GitHub**: [24601/opencode-zellij-namer](https://github.com/24601/opencode-zellij-namer)

**描述**：基于项目上下文的 AI 动态会话命名

**功能特点**：
- AI 驱动的会话命名
- 自动根据项目上下文重命名
- 与 OpenCode 集成

**使用场景**：
- 自动会话管理
- 智能命名建议
- 大型项目组织

**安装**：
```bash
git clone https://github.com/24601/opencode-zellij-namer.git
# 配置 OpenCode API
```

---

## 远程协作

### zeco

**GitHub**: [julianbuettner/zeco](https://github.com/julianbuettner/zeco)

**描述**：在互联网上轻松安全地共享 Zellij 会话

**功能特点**：
- 共享 Zellij 会话
- 安全连接
- 简单易用

**使用场景**：
- 远程协作
- 技术支持
- 实时共享终端

**安装**：
```bash
cargo install zeco
# 或
pip install zeco
```

**使用**：
```bash
# 共享会话
zeco share

# 加入会话
zeco join <token>
```

---

## 编辑器包装器

### zellix

**GitHub**: [TheEmeraldBee/zellix](https://github.com/TheEmeraldBee/zellix)

**描述**：利用 Zellij 将 Helix 变成插件系统的 Nushell 包装器

**功能特点**：
- Nushell 集成
- 将 Helix 作为插件系统
- Zellij 功能扩展

**使用场景**：
- Helix + Zellij 用户
- Nushell 用户
- 插件化编辑体验

**安装**：
```bash
git clone https://github.com/TheEmeraldBee/zellix.git
# 需要 Nushell 和 Helix
```

---

## 对比总结

| 工具类型 | 工具 | 特点 |
|---------|------|------|
| Shell 集成 | fzf-zellij | fzf 集成 |
| Shell 集成 | zellij-sessionizer | 项目切换 |
| IDE | theylix | 禅模式 IDE |
| IDE | yazelix | Helix 集成 |
| IDE | zide | 通用 IDE |
| AI | opencode-zellij-namer | 智能命名 |
| 协作 | zeco | 会话共享 |
| 编辑器 | zellix | Helix 包装器 |

---

## 选择合适的工具

### 如果你是...

**Vim/Neovim 用户**：
- 使用 zide 创建 IDE 工作流
- 考虑 fzf-zellij 增强文件查找

**Helix 用户**：
- theylix 或 yazelix 提供完整体验
- zellix 提供插件化功能

**多项目开发者**：
- zellij-sessionizer 管理项目
- zsm 使用 zoxide 快速切换

**远程协作者**：
- zeco 共享会话
- 结合其他协作工具

---

## 相关资源

- [Zellij 插件](../plugins/) - 扩展 Zellij 功能的插件
- [Zellij 教程](./tutorials.md) - 学习资源

---

*最后更新：2025年2月*
