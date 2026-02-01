# zj-git-branch 插件

> 管理 Git 分支

**GitHub**: [dam4rus/zj-git-branch](https://github.com/dam4rus/zj-git-branch)

---

## 简介

`zj-git-branch` 是一个 Git 分支管理插件，让你在 Zellij 中直接查看和管理 Git 分支。它提供了一个交互式界面来列出、切换、创建和删除分支。

---

## 功能特点

### 1. 分支列表
- 显示所有本地分支
- 显示当前分支
- 显示远程分支

### 2. 分支操作
- 快速切换分支
- 创建新分支
- 删除分支

### 3. 分支信息
- 显示分支提交信息
- 显示与主分支的差异
- 显示最近提交

### 4. 模糊搜索
- 搜索分支名
- 快速定位分支
- 过滤显示

### 5. 可视化
- 分支关系图
- 提交历史
- 合并状态

---

## 使用场景

### 场景 1: 分支切换
快速在不同功能分支间切换：
```
打开 zj-git-branch
选择 feature-x 分支
按 Enter 切换
```

### 场景 2: 分支管理
清理已合并的分支：
```
查看所有分支
删除已合并的 feature 分支
保持仓库整洁
```

### 场景 3: 分支创建
为新功能创建分支：
```
基于 main 创建新分支
自动命名
快速开始开发
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Git 已安装
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/dam4rus/zj-git-branch.git
cd zj-git-branch
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zj-git-branch.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zj-git-branch location="file:~/.config/zellij/plugins/zj-git-branch.wasm"
}

keybinds {
    normal {
        bind "Ctrl b" {
            LaunchOrFocusPlugin "zj-git-branch" {
                floating true
                height "60%"
                width "70%"
            }
        }
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+b`（或你的配置键）打开 zj-git-branch。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zj-git-branch                                        │
├─────────────────────────────────────────────────────┤
│ > feat                                               │
│                                                      │
│ Branches:                                            │
│ * main              latest commit: abc1234          │
│   feature-x         ahead 5, behind 0               │
│   feature-y         ahead 3, behind 2               │
│   hotfix-123        merged                          │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Enter] Switch  [n] New  [d] Delete  [Esc] Quit     │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+b` | 打开 zj-git-branch |
| `↑/↓` | 导航分支 |
| `Enter` | 切换分支 |
| `n` | 创建新分支 |
| `d` | 删除分支 |
| `r` | 刷新 |
| `Esc` | 关闭 |

---

## 与 Lazygit 对比

| 工具 | 功能 | 特点 |
|------|------|------|
| **zj-git-branch** | 分支管理 | 集成在 Zellij 中 |
| Lazygit | 完整 Git UI | 独立 TUI |

**建议**：
- 快速分支操作 → zj-git-branch
- 复杂 Git 操作 → Lazygit

---

## 相关资源
- [官方仓库](https://github.com/dam4rus/zj-git-branch)
- [Lazygit](https://github.com/jesseduffield/lazygit)

---

*最后更新：2025年2月*
