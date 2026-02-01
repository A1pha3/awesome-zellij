# zjpane 插件

> 轻松在面板间导航

**GitHub**: [FuriouZz/zjpane](https://github.com/FuriouZz/zjpane)

---

## 简介

`zjpane` 是一个简洁的面板导航插件，提供了一个简单直观的方式来查看和切换 Zellij 面板。它是 [zellij-pane-picker](./zellij-pane-picker.md) 的轻量级替代方案，专注于核心功能。

---

## 功能特点

### 1. 面板列表
- 显示所有打开的面板
- 简洁的列表视图
- 快速浏览

### 2. 快速切换
- 一键切换到任意面板
- 支持模糊搜索
- 低延迟响应

### 3. 键盘导航
- 完全键盘操作
- 简洁的快捷键
- 无干扰体验

### 4. 轻量级
- 最小资源占用
- 快速启动
- 高效运行

---

## 使用场景

### 场景 1: 快速面板切换
在多个面板间快速切换：
```
打开 zjpane
选择目标面板
按 Enter 切换
```

### 场景 2: 面板概览
查看当前所有面板：
```
了解打开了哪些面板
查看面板标题
快速定位
```

### 场景 3: 键盘工作流
不使用鼠标管理面板：
```
纯键盘操作
快速导航
提高效率
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/FuriouZz/zjpane.git
cd zjpane
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zjpane.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zjpane location="file:~/.config/zellij/plugins/zjpane.wasm"
}

keybinds {
    normal {
        bind "Ctrl p" {
            LaunchOrFocusPlugin "zjpane" {
                floating true
                height "50%"
                width "60%"
            }
        }
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+p`（或你的配置键）打开 zjpane。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zjpane                                               │
├─────────────────────────────────────────────────────┤
│ > bash                                               │
│                                                      │
│ 1. bash                                              │
│ 2. vim - main.rs                                     │
│ 3. cargo test                                        │
│ 4. tail -f log.txt                                   │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Enter] Switch  [Esc] Quit                          │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+p` | 打开 zjpane |
| `↑/↓` | 导航 |
| `Enter` | 切换 |
| `Esc` | 关闭 |

---

## 与 zellij-pane-picker 对比

| 特性 | zjpane | zellij-pane-picker |
|------|--------|-------------------|
| 复杂度 | 简单 | 丰富 |
| 收藏功能 | ❌ | ✅ |
| 自定义 | 基础 | 丰富 |
| 资源占用 | 低 | 中 |

**选择建议**：
- 追求简洁 → zjpane
- 需要收藏 → zellij-pane-picker

---

## 相关资源
- [官方仓库](https://github.com/FuriouZz/zjpane)
- [zellij-pane-picker](./zellij-pane-picker.md)
- [harpoon](./harpoon.md)

---

*最后更新：2025年2月*
