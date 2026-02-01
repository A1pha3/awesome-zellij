# zellij-tab-bar-indexed 插件

> 为标签栏添加数字索引以便快速导航

**GitHub**: [ivoronin/zellij-tab-bar-indexed](https://github.com/ivoronin/zellij-tab-bar-indexed)

---

## 简介

`zellij-tab-bar-indexed` 是一个增强版的标签栏插件，它在每个标签前添加数字索引（1、2、3...），让你可以使用数字键快速跳转到特定标签。这对于频繁切换标签的用户非常有用。

---

## 功能特点

### 1. 数字索引
- 为每个标签添加数字前缀
- 清晰显示标签编号
- 支持 1-9 的标签

### 2. 快速导航
- 使用 Alt+1~9 快速跳转到标签
- 无需查看标签栏即可切换
- 肌肉记忆导航

### 3. 视觉增强
- 高亮当前标签
- 清晰的数字标识
- 可定制颜色

### 4. 与原标签栏兼容
- 基于官方 tab-bar 插件
- 保留所有原有功能
- 无缝替换

### 5. 轻量级
- 最小性能开销
- 快速渲染
- 稳定可靠

---

## 使用场景

### 场景 1: 快速标签切换
使用数字键快速跳转到任意标签：
```
Alt+1 → 跳转到标签 1
Alt+2 → 跳转到标签 2
...
Alt+9 → 跳转到标签 9
```

### 场景 2: 固定工作流
为常用功能分配固定编号：
```
标签 1: 代码编辑
标签 2: 终端
标签 3: 日志
标签 4: 文档
```

### 场景 3: 多项目切换
为不同项目使用不同标签：
```
1: 项目 A
2: 项目 B
3: 项目 C
```

### 场景 4: 键盘驱动工作流
不使用鼠标，完全键盘操作：
- 创建标签
- 切换标签
- 关闭标签

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/ivoronin/zellij-tab-bar-indexed.git
cd zellij-tab-bar-indexed
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-tab-bar-indexed.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中替换默认标签栏：

```kdl
plugins {
    zellij-tab-bar-indexed location="file:~/.config/zellij/plugins/zellij-tab-bar-indexed.wasm"
}

layout {
    default_tab_template {
        pane size=1 borderless=true {
            plugin location="zellij-tab-bar-indexed"
        }
        pane
        pane size=1 borderless=true {
            plugin location="zellij:compact-bar"
        }
    }
}

keybinds {
    normal {
        // 标签导航
        bind "Alt 1" { GoToTab 1; }
        bind "Alt 2" { GoToTab 2; }
        bind "Alt 3" { GoToTab 3; }
        bind "Alt 4" { GoToTab 4; }
        bind "Alt 5" { GoToTab 5; }
        bind "Alt 6" { GoToTab 6; }
        bind "Alt 7" { GoToTab 7; }
        bind "Alt 8" { GoToTab 8; }
        bind "Alt 9" { GoToTab 9; }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-tab-bar-indexed location="file:~/.config/zellij/plugins/zellij-tab-bar-indexed.wasm" {
        // 显示选项
        show_index true
        index_separator ":"
        
        // 颜色
        active_color "#00ff00"
        inactive_color "#666666"
        index_color "#ffff00"
        
        // 格式
        tab_format "{index}{separator}{name}"
    }
}
```

---

## 使用方法

### 界面显示

**默认标签栏：**
```
┌─────────────────────────────────────────────────────┐
│ myproject │ docs* │ config                            │
└─────────────────────────────────────────────────────┘
```

**zellij-tab-bar-indexed：**
```
┌─────────────────────────────────────────────────────┐
│ 1:myproject │ 2:docs* │ 3:config                      │
└─────────────────────────────────────────────────────┘
```

### 快速导航

| 快捷键 | 功能 |
|--------|------|
| `Alt+1` | 跳转到标签 1 |
| `Alt+2` | 跳转到标签 2 |
| `Alt+3` | 跳转到标签 3 |
| ... | ... |
| `Alt+9` | 跳转到标签 9 |

### 工作流程

**创建固定工作流：**
```
1. 创建标签并分配功能：
   标签 1: 主代码编辑
   标签 2: 测试和构建
   标签 3: 文档和笔记

2. 使用 Alt+1~3 在这些标签间快速切换
3. 无需查看标签栏，纯肌肉记忆操作
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `show_index` | boolean | true | 显示数字索引 |
| `index_separator` | string | ":" | 索引和名称分隔符 |
| `active_color` | string | 自动 | 活动标签颜色 |
| `inactive_color` | string | 自动 | 非活动标签颜色 |
| `index_color` | string | 自动 | 索引数字颜色 |
| `tab_format` | string | "{index}{separator}{name}" | 标签格式 |

---

## 与 room/zbuffers 对比

| 工具 | 导航方式 | 适用场景 |
|------|---------|---------|
| **zellij-tab-bar-indexed** | Alt+数字 | 固定布局 |
| room | 搜索+选择 | 动态标签 |
| zbuffers | 列表选择 | 浏览模式 |

**建议组合使用**：
- zellij-tab-bar-indexed：快速跳转到常用标签
- room：查找不常用的标签

---

## 故障排除

### 索引不显示
- 确认插件正确加载
- 检查 `show_index` 设置
- 重启 Zellij

### 导航不工作
- 确认键绑定配置正确
- 检查是否有快捷键冲突
- 确认 Alt 键未被终端拦截

### 颜色不生效
- 检查颜色格式（#RRGGBB）
- 确认终端支持真彩色
- 检查 Zellij 主题配置

---

## 相关资源
- [官方仓库](https://github.com/ivoronin/zellij-tab-bar-indexed)
- [room](./room.md) - 标签搜索切换
- [zbuffers](./zbuffers.md) - 缓冲区式标签切换

---

*最后更新：2025年2月*
