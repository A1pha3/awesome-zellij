# zellij-sessionizer 插件

> 基于文件夹名称创建会话

**GitHub**: [laperlej/zellij-sessionizer](https://github.com/laperlej/zellij-sessionizer)

---

## 简介

`zellij-sessionizer` 是一个会话创建插件，它通过选择文件夹来创建新的 Zellij 会话。与 [zellij-choose-tree](./zellij-choose-tree.md) 不同，sessionizer 专注于基于文件夹创建新会话，非常适合项目管理。

---

## 功能特点

### 1. 文件夹选择
- 浏览文件系统选择文件夹
- 支持模糊搜索文件夹
- 记住最近使用的文件夹

### 2. 自动会话创建
- 基于文件夹名自动命名会话
- 自动导航到该文件夹
- 自动创建初始标签和面板

### 3. 布局支持
- 可为不同项目配置不同布局
- 自动应用项目特定的布局
- 支持默认布局

### 4. 会话管理
- 切换到现有会话
- 创建新会话
- 删除不需要的会话

### 5. 快速访问
- 快捷键快速启动
- 浮动窗口不干扰工作流
- 一键完成创建

---

## 使用场景

### 场景 1: 项目启动
快速为项目创建新会话：
```
1. 按快捷键打开 sessionizer
2. 导航到项目文件夹或输入项目名
3. 按 Enter 创建会话
4. 自动创建以项目名命名的会话
```

### 场景 2: 多项目切换
在不同项目间切换：
```
项目 A: ~/work/project-a
项目 B: ~/work/project-b
个人项目: ~/personal/blog

使用 sessionizer 快速在它们之间切换
```

### 场景 3: 临时会话
为临时任务创建会话：
```
需要查看 /var/log 的日志
使用 sessionizer 创建 "logs" 会话
完成后删除
```

### 场景 4: 标准化工作流
为新项目使用标准布局：
```
所有 Rust 项目使用开发布局
所有 Node 项目使用前端布局
sessionizer 自动应用正确布局
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/laperlej/zellij-sessionizer.git
cd zellij-sessionizer
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-sessionizer.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-sessionizer location="file:~/.config/zellij/plugins/zellij-sessionizer.wasm"
}

keybinds {
    normal {
        bind "Ctrl s" {
            LaunchOrFocusPlugin "zellij-sessionizer" {
                floating true
                height "60%"
                width "70%"
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-sessionizer location="file:~/.config/zellij/plugins/zellij-sessionizer.wasm" {
        // 默认搜索路径
        search_paths ["~", "~/projects", "~/work"]
        
        // 排除路径
        exclude_paths ["node_modules", ".git", "target"]
        
        // 默认布局
        default_layout "default"
        
        // 布局映射
        layout_mappings {
            "rust" "~/layouts/rust-dev.kdl"
            "node" "~/layouts/node-dev.kdl"
            "python" "~/layouts/python-dev.kdl"
        }
        
        // 检测项目类型的文件
        project_markers {
            "Cargo.toml" "rust"
            "package.json" "node"
            "requirements.txt" "python"
            "pyproject.toml" "python"
        }
        
        // 显示选项
        show_recent 10
        show_preview true
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+s`（或你的配置键）打开 sessionizer 面板。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zellij-sessionizer                                   │
├─────────────────────────────────────────────────────┤
│ > project                                            │
│                                                      │
│ Recent Folders:                                      │
│   1. ~/work/project-a     [2 days ago]              │
│   2. ~/projects/web-app   [1 week ago]              │
│   3. ~/personal/blog      [2 weeks ago]             │
│                                                      │
│ Matching Folders:                                    │
│   ~/work/project-alpha                              │
│   ~/projects/web-frontend                           │
│   ~/archive/old-project                             │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Enter] Create/Join  [Ctrl+n] New  [Esc] Quit       │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+s` | 打开 sessionizer |
| `↑/↓` | 导航文件夹列表 |
| `Enter` | 创建或加入会话 |
| `Ctrl+n` | 强制创建新会话 |
| `Tab` | 切换视图 |
| `/` | 搜索文件夹 |
| `Esc` | 关闭面板 |

### 工作流程

**创建新会话：**
```
1. 按 Ctrl+s 打开 sessionizer
2. 使用 ↑/↓ 导航或使用 / 搜索
3. 选择文件夹
4. 按 Enter
5. 如果会话不存在，自动创建
6. 如果会话存在，自动切换
```

**使用布局：**
```
选择包含 Cargo.toml 的文件夹
插件检测到 Rust 项目
自动应用 rust-dev 布局
创建编辑器面板 + 终端面板 + 测试面板
```

---

## 布局配置

### 创建项目特定布局

创建 `~/.config/zellij/layouts/rust-dev.kdl`：

```kdl
layout {
    pane split_direction="horizontal" {
        pane
        pane size="30%" split_direction="horizontal" {
            pane {
                command "cargo"
                args "watch" "-x" "check"
            }
            pane {
                command "cargo"
                args "test"
            }
        }
    }
}
```

### 配置映射

```kdl
plugins {
    zellij-sessionizer location="file:~/.config/zellij/plugins/zellij-sessionizer.wasm" {
        layout_mappings {
            "rust" "~/.config/zellij/layouts/rust-dev.kdl"
            "node" "~/.config/zellij/layouts/node-dev.kdl"
        }
        
        project_markers {
            "Cargo.toml" "rust"
            "package.json" "node"
        }
    }
}
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `search_paths` | list | ["~"] | 搜索文件夹的路径 |
| `exclude_paths` | list | [".git"] | 排除的路径模式 |
| `default_layout` | string | "default" | 默认布局 |
| `layout_mappings` | map | {} | 布局映射 |
| `project_markers` | map | {} | 项目类型标记文件 |
| `show_recent` | integer | 10 | 显示最近文件夹数 |
| `show_preview` | boolean | true | 显示文件夹预览 |

---

## 与其他工具对比

| 工具 | 功能 | 特点 |
|------|------|------|
| **zellij-sessionizer** | 创建+切换 | 基于文件夹 |
| zellij-choose-tree | 管理现有 | 树形视图 |
| zsm | 切换 | zoxide 集成 |
| zellij-favs | 收藏 | 管理收藏 |

---

## 故障排除

### 文件夹不显示
- 检查 `search_paths` 配置
- 确认路径存在且有权限
- 检查 `exclude_paths` 没有误排除

### 布局不应用
- 检查布局文件路径
- 验证布局文件语法
- 检查 `project_markers` 配置

### 会话创建失败
- 确认文件夹存在
- 检查 Zellij 版本
- 查看日志

---

## 相关资源
- [官方仓库](https://github.com/laperlej/zellij-sessionizer)
- [zellij-choose-tree](./zellij-choose-tree.md) - 会话树形浏览
- [zsm](./zsm.md) - zoxide 集成会话切换
- [ThePrimeagen/tmux-sessionizer](https://github.com/ThePrimeagen/tmux-sessionizer) - Tmux 版本

---

*最后更新：2025年2月*
