# zsm 插件

> 支持默认布局的 zoxide 集成会话切换器

**GitHub**: [liam-mackie/zsm](https://github.com/liam-mackie/zsm)

---

## 简介

`zsm` (Zellij Session Manager) 是一个 zoxide 集成的会话切换器，类似于 [zellij-sessionizer](./zellij-sessionizer.md) 和 [zellij-switch](./zellij-switch.md)。它通过 zoxide 的智能目录跳转功能，让你快速创建和切换 Zellij 会话。

---

## 功能特点

### 1. zoxide 集成
- 使用 zoxide 数据库
- 智能目录匹配
- 快速跳转到常用目录

### 2. 会话切换
- 快速切换现有会话
- 创建新会话
- 删除会话

### 3. 布局支持
- 支持默认布局
- 目录特定布局
- 自动应用布局

### 4. 模糊搜索
- 模糊匹配目录
- 按 frecency 排序
- 快速选择

### 5. CLI 友好
- 命令行调用
- 脚本集成
- 自动化支持

---

## 使用场景

### 场景 1: 快速项目切换
使用 zoxide 快速跳转到项目：
```bash
zsm pro
# 匹配到 ~/projects/my-project
# 切换到 my-project 会话
```

### 场景 2: 新会话创建
为新项目创建会话：
```bash
zsm new-project
# 如果会话不存在，自动创建
# 应用默认布局
```

### 场景 3: 脚本自动化
在脚本中使用：
```bash
#!/bin/bash
# 切换到项目并运行测试
zsm project-a
cargo test
```

### 场景 4: fzf 集成
与 fzf 结合：
```bash
zsm --list | fzf | xargs zsm
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- [zoxide](https://github.com/ajeetdsouza/zoxide) 已安装
- Rust 工具链

### 安装 zoxide

```bash
# macOS
brew install zoxide

# Ubuntu/Debian
apt-get install zoxide

# 添加到 shell 配置
echo 'eval "$(zoxide init bash)"' >> ~/.bashrc
```

### 从源码安装 zsm

1. 克隆仓库：
```bash
git clone https://github.com/liam-mackie/zsm.git
cd zsm
```

2. 编译：
```bash
cargo build --release
```

3. 安装：
```bash
cp target/release/zsm ~/.local/bin/
# 或
sudo cp target/release/zsm /usr/local/bin/
```

---

## 配置方法

### 基本配置

创建 `~/.config/zsm/config.toml`：

```toml
# zsm 配置

# 默认布局
default_layout = "default"

# 布局映射
[layout_mappings]
"rust" = "rust-dev"
"node" = "node-dev"

# 项目类型检测
[project_types]
"Cargo.toml" = "rust"
"package.json" = "node"
```

---

## 使用方法

### 基本命令

```bash
# 列出所有会话
zsm --list

# 切换到会话（模糊匹配）
zsm project

# 创建或切换到目录对应的会话
zsm ~/projects/web-app

# 删除会话
zsm --delete old-project
```

### 工作流程

**快速开始工作：**
```bash
# 访问目录（zoxide 记录）
cd ~/projects/my-awesome-project

# 之后快速切换
zsm awe
# zoxide 匹配到 my-awesome-project
# zsm 切换到该会话
```

**结合 Zellij：**
```bash
# 在 zellij 中使用
zellij pipe --plugin zellij-getmode  # 获取当前模式
zsm project-a                        # 切换到项目 A
```

---

## 与 zellij-sessionizer 对比

| 工具 | 使用方式 | 特点 |
|------|---------|------|
| **zsm** | CLI | zoxide 集成 |
| zellij-sessionizer | 插件 | 可视化界面 |
| zellij-switch | CLI/pipe | 直接切换 |

**建议**：
- 习惯命令行 → zsm
- 喜欢可视化 → zellij-sessionizer

---

## 相关资源
- [官方仓库](https://github.com/liam-mackie/zsm)
- [zoxide](https://github.com/ajeetdsouza/zoxide)
- [zellij-sessionizer](./zellij-sessionizer.md)
- [zellij-switch](./zellij-switch.md)

---

*最后更新：2025年2月*
