# zjstatus 插件

> 可配置、可主题化的状态栏插件

**GitHub**: [dj95/zjstatus](https://github.com/dj95/zjstatus)

---

## 简介

`zjstatus` 是一个功能丰富的 Zellij 状态栏插件，提供高度的可配置性和主题支持。它是 Zellij 生态中最流行的状态栏插件之一，被许多用户用于替换默认的状态栏。

---

## 功能特点

### 1. 高度可配置
- 自定义显示内容和格式
- 丰富的配置选项
- 条件显示逻辑

### 2. 主题支持
- 内置多种主题
- 自定义颜色方案
- 动态颜色变化

### 3. 模块系统
- 多种内置模块
- 可启用/禁用模块
- 模块排列顺序

### 4. 图标支持
- 支持 Nerd Fonts 图标
- 美化状态栏显示
- 可自定义图标

### 5. 动态更新
- 实时反映状态变化
- 条件渲染
- 高效刷新

---

## 使用场景

### 场景 1: 自定义状态栏
完全自定义状态栏外观：
```
选择显示的模块
自定义颜色和格式
添加图标
```

### 场景 2: 主题应用
应用美观的主题：
```
选择预设主题
或自定义主题
匹配终端配色
```

### 场景 3: 信息显示
显示丰富的状态信息：
```
系统信息
Git 状态
自定义信息
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/dj95/zjstatus.git
cd zjstatus
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zjstatus.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zjstatus location="file:~/.config/zellij/plugins/zjstatus.wasm"
}

layout {
    default_tab_template {
        pane size=1 borderless=true {
            plugin location="zellij:tab-bar"
        }
        pane
        pane size=1 borderless=true {
            plugin location="zjstatus"
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zjstatus location="file:~/.config/zellij/plugins/zjstatus.wasm" {
        format_left "[{mode}] {tabs}"
        format_center ""
        format_right "{session} {datetime}"
        
        hide_frame_for_single_pane "false"
        
        mode_normal "#[bg=#2e3440,fg=#d8dee9]"
        mode_insert "#[bg=#bf616a,fg=#2e3440]"
        mode_locked "#[bg=#ff5555,fg=#ffffff]"
        
        tab_normal "#[bg=#2e3440,fg=#d8dee9]"
        tab_active "#[bg=#88c0d0,fg=#2e3440]"
    }
}
```

---

## 配置选项

| 选项 | 类型 | 描述 |
|------|------|------|
| `format_left` | string | 左侧格式 |
| `format_center` | string | 中间格式 |
| `format_right` | string | 右侧格式 |
| `mode_*` | string | 各模式颜色 |
| `tab_*` | string | 标签颜色 |

---

## 相关资源
- [官方仓库](https://github.com/dj95/zjstatus)
- [zjstatus-hints](./zjstatus-hints.md) - zjstatus 的提示扩展
- [开发博客](https://blog.nerd.rocks/posts/profiling-zellij-plugins/)

---

*最后更新：2025年2月*
