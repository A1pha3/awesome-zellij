# zjstatus-hints 插件

> 为 zjstatus 添加模式感知的键绑定提示

**GitHub**: [b0o/zjstatus-hints](https://github.com/b0o/zjstatus-hints)

---

## 简介

`zjstatus-hints` 是 [zjstatus](./zjstatus.md) 的扩展插件，为状态栏添加模式感知的键绑定提示。它会在状态栏中显示当前模式下可用的快捷键，帮助用户学习和记忆 Zellij 的快捷键。

---

## 功能特点

### 1. 模式感知提示
- 根据当前模式显示提示
- 显示该模式下的可用快捷键
- 实时更新

### 2. 与 zjstatus 集成
- 无缝集成 zjstatus
- 使用 zjstatus 的格式系统
- 统一的视觉风格

### 3. 可定制提示
- 选择显示哪些提示
- 自定义提示格式
- 快捷键分组

### 4. 学习辅助
- 帮助记忆快捷键
- 新用户友好
- 渐进式学习

### 5. 节省空间
- 紧凑的提示显示
- 不占用过多状态栏空间
- 智能布局

---

## 使用场景

### 场景 1: 学习快捷键
刚接触 Zellij 时学习快捷键：
```
查看当前模式的可用快捷键
边用边学
逐渐记住常用快捷键
```

### 场景 2: 不常用模式
使用不熟悉的模式时：
```
进入 resize 模式
状态栏显示 resize 的快捷键
完成操作后自动切换提示
```

### 场景 3: 快速参考
忘记特定快捷键时：
```
看一眼状态栏
找到需要的快捷键
无需查阅文档
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- [zjstatus](./zjstatus.md) 已安装
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/b0o/zjstatus-hints.git
cd zjstatus-hints
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zjstatus-hints.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 与 zjstatus 一起配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zjstatus location="file:~/.config/zellij/plugins/zjstatus.wasm"
    zjstatus-hints location="file:~/.config/zellij/plugins/zjstatus-hints.wasm"
}

layout {
    default_tab_template {
        pane size=1 borderless=true {
            plugin location="zellij:tab-bar"
        }
        pane
        pane size=2 borderless=true {
            plugin location="zjstatus-hints"  // 或集成到 zjstatus
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zjstatus-hints location="file:~/.config/zellij/plugins/zjstatus-hints.wasm" {
        // 显示选项
        show_in_normal true
        show_in_locked false
        max_hints 5
        
        // 提示格式
        hint_format "{key}: {action}"
        separator " | "
        
        // 自定义提示
        custom_hints {
            normal {
                "Ctrl+p" "Plugins"
                "Ctrl+t" "Tabs"
            }
            pane {
                "←→↑↓" "Navigate"
                "x" "Close"
            }
        }
    }
}
```

---

## 界面显示

```
┌─────────────────────────────────────────────────────────────────┐
│ [NORMAL] 1:main 2:docs* 3:test                                   │
│ Ctrl+p:Pane | Ctrl+t:Tab | Ctrl+s:Session | Ctrl+n:New          │
└─────────────────────────────────────────────────────────────────┘
```

---

## 与 zellij-forgot 对比

| 特性 | zjstatus-hints | zellij-forgot |
|------|---------------|---------------|
| 位置 | 状态栏 | 浮动面板 |
| 实时性 | ✅ 始终显示 | ❌ 需要打开 |
| 详细程度 | 简洁 | 详细 |
| 模式感知 | ✅ | ✅ |

**建议组合使用**：
- zjstatus-hints：日常快速参考
- zellij-forgot：详细查询

---

## 相关资源
- [官方仓库](https://github.com/b0o/zjstatus-hints)
- [zjstatus](./zjstatus.md) - 基础状态栏插件
- [zellij-forgot](./zellij-forgot.md) - 快捷键查看插件

---

*最后更新：2025年2月*
