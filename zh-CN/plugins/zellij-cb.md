# zellij-cb 插件

> Zellij 的可定制紧凑型状态栏

**GitHub**: [ndavd/zellij-cb](https://github.com/ndavd/zellij-cb)

---

## 简介

`zellij-cb` (compact-bar) 是一个高度可定制的紧凑型状态栏插件，用于替换或补充 Zellij 的默认状态栏。它提供更简洁的界面和更多的自定义选项，适合喜欢极简风格的用户。

---

## 功能特点

### 1. 紧凑设计
- 更小的屏幕占用
- 简洁的信息展示
- 适合小屏幕或注重空间的用户

### 2. 高度可定制
- 自定义显示哪些信息
- 自定义颜色和样式
- 自定义布局和格式

### 3. 信息显示
- 当前模式
- 活动标签页
- 系统信息（可选）
- 自定义文本

### 4. 主题支持
- 支持 Zellij 主题
- 自定义颜色方案
- 动态颜色变化

### 5. 轻量级
- 低资源占用
- 快速渲染
- 不影响性能

---

## 使用场景

### 场景 1: 小屏幕笔记本
在屏幕空间有限的情况下最大化编辑区域：
```
默认状态栏: 占用 2-3 行
zellij-cb: 仅占用 1 行，信息更紧凑
```

### 场景 2: 极简主义用户
喜欢干净的界面，只显示必要信息：
- 隐藏不常用的信息
- 只显示模式和标签
- 自定义颜色匹配终端主题

### 场景 3: 嵌入式开发
在资源受限的环境中：
- 低 CPU 和内存占用
- 简化界面
- 专注核心功能

### 场景 4: 自定义品牌
为团队或项目定制状态栏：
- 显示项目标识
- 自定义颜色和 Logo
- 显示环境信息

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/ndavd/zellij-cb.git
cd zellij-cb
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-cb.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中替换默认状态栏：

```kdl
// 定义紧凑栏插件
plugins {
    zellij-cb location="file:~/.config/zellij/plugins/zellij-cb.wasm"
}

// 在布局中使用
layout {
    default_tab_template {
        pane size=1 borderless=true {
            plugin location="zellij:tab-bar"
        }
        pane
        pane size=1 borderless=true {
            plugin location="zellij-cb"
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-cb location="file:~/.config/zellij/plugins/zellij-cb.wasm" {
        // 显示选项
        show_mode true
        show_tabs true
        show_session_name true
        show_clock true
        
        // 格式选项
        mode_format "[{mode}]"
        tab_format "{index}:{name}"
        separator " | "
        
        // 颜色配置
        mode_normal_color "#00ff00"
        mode_insert_color "#ffff00"
        mode_locked_color "#ff0000"
        tab_active_color "#ffffff"
        tab_inactive_color "#666666"
        
        // 对齐方式
        alignment "left"  // left, right, center
    }
}
```

---

## 使用方法

### 界面说明

**默认状态栏：**
```
┌─────────────────────────────────────────────────────────────────┐
│ 1: project │ 2: docs* │ 3: config                            │
│ [NORMAL] | Session: dev | user@hostname | 2025-01-30 14:30     │
└─────────────────────────────────────────────────────────────────┘
```

**zellij-cb 紧凑型：**
```
┌─────────────────────────────────────────────────────────────────┐
│ [N] 1:project | 2:docs* | 3:config | dev | 14:30                │
└─────────────────────────────────────────────────────────────────┘
```

### 配置示例

**极简配置：**
```kdl
plugins {
    zellij-cb location="file:~/.config/zellij/plugins/zellij-cb.wasm" {
        show_mode true
        show_tabs true
        show_session_name false
        show_clock false
        mode_format "{mode}"
        separator " "
    }
}
```

结果显示：
```
│ NORMAL 1:project 2:docs* 3:config                                  │
```

**信息丰富配置：**
```kdl
plugins {
    zellij-cb location="file:~/.config/zellij/plugins/zellij-cb.wasm" {
        show_mode true
        show_tabs true
        show_session_name true
        show_clock true
        show_hostname true
        show_user true
        mode_format "[{mode}]"
        separator " │ "
        alignment "center"
    }
}
```

结果显示：
```
│          [NORMAL] │ 1:project │ 2:docs* │ dev │ user@host │ 14:30          │
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `show_mode` | boolean | true | 显示当前模式 |
| `show_tabs` | boolean | true | 显示标签列表 |
| `show_session_name` | boolean | false | 显示会话名称 |
| `show_clock` | boolean | false | 显示时间 |
| `show_hostname` | boolean | false | 显示主机名 |
| `show_user` | boolean | false | 显示用户名 |
| `mode_format` | string | "[{mode}]" | 模式显示格式 |
| `tab_format` | string | "{index}:{name}" | 标签显示格式 |
| `separator` | string | " | " | 项目分隔符 |
| `alignment` | string | "left" | 对齐方式 |

### 颜色选项

| 选项 | 描述 |
|------|------|
| `mode_normal_color` | 普通模式颜色 |
| `mode_insert_color` | 插入模式颜色 |
| `mode_locked_color` | 锁定模式颜色 |
| `mode_tmux_color` | Tmux 模式颜色 |
| `mode_resize_color` | 调整大小模式颜色 |
| `mode_pane_color` | 面板模式颜色 |
| `mode_tab_color` | 标签模式颜色 |
| `mode_scroll_color` | 滚动模式颜色 |
| `mode_search_color` | 搜索模式颜色 |
| `mode_session_color` | 会话模式颜色 |
| `mode_move_color` | 移动模式颜色 |
| `tab_active_color` | 活动标签颜色 |
| `tab_inactive_color` | 非活动标签颜色 |
| `background_color` | 背景颜色 |
| `foreground_color` | 前景颜色 |

---

## 主题配置

### Dracula 主题示例

```kdl
plugins {
    zellij-cb location="file:~/.config/zellij/plugins/zellij-cb.wasm" {
        mode_normal_color "#50fa7b"
        mode_insert_color "#f1fa8c"
        mode_locked_color "#ff5555"
        tab_active_color "#bd93f9"
        tab_inactive_color "#6272a4"
        background_color "#282a36"
        foreground_color "#f8f8f2"
    }
}
```

### Nord 主题示例

```kdl
plugins {
    zellij-cb location="file:~/.config/zellij/plugins/zellij-cb.wasm" {
        mode_normal_color "#a3be8c"
        mode_insert_color "#ebcb8b"
        mode_locked_color "#bf616a"
        tab_active_color "#88c0d0"
        tab_inactive_color "#4c566a"
        background_color "#2e3440"
        foreground_color "#d8dee9"
    }
}
```

---

## 与默认状态栏对比

| 特性 | zellij-cb | 默认 compact-bar | 默认 tab-bar |
|------|-----------|------------------|--------------|
| 可定制性 | ⭐⭐⭐ | ⭐⭐ | ⭐ |
| 资源占用 | 低 | 中 | 低 |
| 信息显示 | 可配置 | 固定 | 较少 |
| 主题支持 | ✅ | ⚠️ | ⚠️ |
| 第三方插件 | ✅ | ❌ | ❌ |

---

## 故障排除

### 状态栏不显示
- 确认插件路径正确
- 检查布局配置
- 确认插件已编译

### 颜色不生效
- 检查颜色格式（使用 #RRGGBB）
- 确认终端支持真彩色
- 检查 Zellij 主题配置

### 信息显示不正确
- 检查配置选项拼写
- 确认布尔值格式正确
- 重启 Zellij

---

## 相关资源
- [官方仓库](https://github.com/ndavd/zellij-cb)
- [zj-status-bar](./zj-status-bar.md) - 另一个状态栏分支
- [zjstatus](./zjstatus.md) - 高级状态栏插件
- [Zellij 主题文档](https://zellij.dev/documentation/themes.html)

---

*最后更新：2025年2月*
