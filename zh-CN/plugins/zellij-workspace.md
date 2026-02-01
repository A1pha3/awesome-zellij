# zellij-workspace 插件

> 将布局应用到当前会话

**GitHub**: [vdbulcke/zellij-workspace](https://github.com/vdbulcke/zellij-workspace)

---

## 简介

`zellij-workspace` 是一个实用的 Zellij 插件，允许你在当前会话中动态应用预定义的布局。这让你可以在不重启会话的情况下，重新组织你的工作区布局。

---

## 功能特点

### 1. 布局应用
- 在当前会话中应用布局
- 动态重新组织面板
- 无需重启 Zellij

### 2. 布局管理
- 浏览可用布局
- 快速切换布局
- 保存当前布局

### 3. 工作流切换
- 为不同任务切换不同布局
- 从开发布局切换到调试布局
- 灵活适应工作流变化

### 4. 预设布局
- 内置常用布局
- 支持自定义布局
- 布局模板库

### 5. 无损切换
- 尽量保留现有面板
- 智能合并布局
- 最小化干扰

---

## 使用场景

### 场景 1: 开发模式切换
从编码布局切换到调试布局：
```
编码布局: [编辑器] [终端]
调试布局: [编辑器] [调试器] [日志] [控制台]
```

### 场景 2: 代码审查
切换到审查布局：
```
审查布局: [旧代码] [新代码] [终端] [注释]
```

### 场景 3: 演示准备
快速切换到演示布局：
```
演示布局: [代码] [运行结果] [说明]
```

### 场景 4: 多显示器适配
根据显示器切换布局：
```
单显示器: 紧凑布局
双显示器: 扩展布局
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/vdbulcke/zellij-workspace.git
cd zellij-workspace
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-workspace.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-workspace location="file:~/.config/zellij/plugins/zellij-workspace.wasm"
}

keybinds {
    normal {
        bind "Ctrl w" {
            LaunchOrFocusPlugin "zellij-workspace" {
                floating true
                height "50%"
                width "60%"
            }
        }
    }
}
```

### 创建布局文件

创建 `~/.config/zellij/layouts/` 目录并添加布局文件：

```kdl
// ~/.config/zellij/layouts/debug.kdl
layout {
    pane split_direction="horizontal" {
        pane size="60%"
        pane size="40%" split_direction="horizontal" {
            pane {
                command "gdb"
            }
            pane split_direction="horizontal" {
                pane {
                    command "tail"
                    args "-f" "logs/app.log"
                }
                pane
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-workspace location="file:~/.config/zellij/plugins/zellij-workspace.wasm" {
        // 布局目录
        layouts_dir "~/.config/zellij/layouts"
        
        // 默认布局
        default_layout "default"
        
        // 显示选项
        show_preview true
        show_description true
        
        // 应用行为
        preserve_panes true
        confirm_apply true
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+w`（或你的配置键）打开 workspace 面板。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zellij-workspace                                     │
├─────────────────────────────────────────────────────┤
│ > dev                                                │
│                                                      │
│ Available Layouts:                                   │
│   1. default          Standard development layout   │
│   2. debug            Debug session with gdb        │
│   3. review           Code review side-by-side      │
│   4. presentation     Demo layout with preview      │
│   5. monitor          System monitoring layout      │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Enter] Apply  [p] Preview  [s] Save Current       │
│ [Esc] Quit                                           │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+w` | 打开 workspace |
| `↑/↓` | 导航布局 |
| `Enter` | 应用选中布局 |
| `p` | 预览布局 |
| `s` | 保存当前布局 |
| `Esc` | 关闭面板 |

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `layouts_dir` | string | "~/.config/zellij/layouts" | 布局文件目录 |
| `default_layout` | string | "default" | 默认布局 |
| `show_preview` | boolean | true | 显示布局预览 |
| `show_description` | boolean | true | 显示布局描述 |
| `preserve_panes` | boolean | true | 尽量保留现有面板 |
| `confirm_apply` | boolean | true | 应用前确认 |

---

## 故障排除

### 布局不显示
- 检查布局文件路径
- 验证布局文件语法
- 确认文件扩展名为 .kdl

### 布局应用失败
- 检查布局文件有效性
- 确认 `preserve_panes` 设置
- 查看 Zellij 日志

---

## 相关资源
- [官方仓库](https://github.com/vdbulcke/zellij-workspace)
- [Zellij 布局文档](https://zellij.dev/documentation/layouts.html)

---

*最后更新：2025年2月*
