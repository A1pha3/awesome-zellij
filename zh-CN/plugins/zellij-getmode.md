# zellij-getmode 插件

> 获取 Zellij 当前输入模式的简单工具插件

**GitHub**: [chardskarth/zellij-getmode](https://github.com/chardskarth/zellij-getmode)

---

## 简介

`zellij-getmode` 是一个简单但实用的 Zellij 插件，它提供了一种获取当前 Zellij 输入模式的方法。这对于编写脚本或开发其他需要知道 Zellij 当前状态的插件非常有用。

---

## 功能特点

### 1. 模式查询
- 获取当前 Zellij 输入模式
- 返回标准模式名称
- 支持所有 Zellij 模式

### 2. 脚本集成
- 可以从外部脚本调用
- 适合自动化工作流
- 配合 `zellij pipe` 使用

### 3. 开发工具
- 其他插件可以依赖此插件
- 提供统一的模式查询接口
- 简化插件开发

### 4. 轻量级
- 极小资源占用
- 快速响应
- 无界面组件

---

## 使用场景

### 场景 1: 外部脚本自动化
在脚本中检查 Zellij 当前模式：
```bash
#!/bin/bash
mode=$(zellij pipe --plugin zellij-getmode)
if [ "$mode" = "locked" ]; then
    echo "Zellij is locked"
else
    echo "Current mode: $mode"
fi
```

### 场景 2: 状态栏增强
在自定义状态栏中显示当前模式：
```bash
# 在状态栏脚本中
mode=$(zellij pipe --plugin zellij-getmode)
echo "[$mode]"
```

### 场景 3: 插件开发
在其他插件中使用此插件查询模式：
```rust
// 在其他插件中调用
let mode = pipe_message_to_plugin("zellij-getmode", "");
match mode.as_str() {
    "normal" => { /* 处理普通模式 */ },
    "locked" => { /* 处理锁定模式 */ },
    _ => { /* 其他模式 */ }
}
```

### 场景 4: 模式切换验证
验证模式切换是否成功：
```bash
zellij action switch-to-mode locked
sleep 0.1
current=$(zellij pipe --plugin zellij-getmode)
if [ "$current" = "locked" ]; then
    echo "Successfully switched to locked mode"
fi
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/chardskarth/zellij-getmode.git
cd zellij-getmode
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-getmode.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-getmode location="file:~/.config/zellij/plugins/zellij-getmode.wasm"
}
```

此插件通常不需要额外的键绑定配置，因为它主要通过 `zellij pipe` 调用。

---

## 使用方法

### 命令行调用

使用 `zellij pipe` 命令调用插件：

```bash
# 获取当前模式
zellij pipe --plugin zellij-getmode

# 输出示例
# normal
# locked
# pane
# tab
# resize
```

### 在脚本中使用

**Bash 脚本示例：**
```bash
#!/bin/bash

# 获取当前模式
CURRENT_MODE=$(zellij pipe --plugin zellij-getmode 2>/dev/null)

case "$CURRENT_MODE" in
    "normal")
        echo "In normal mode - can use all keybindings"
        ;;
    "locked")
        echo "In locked mode - keys pass to application"
        ;;
    "pane")
        echo "In pane mode - pane management shortcuts active"
        ;;
    "tab")
        echo "In tab mode - tab management shortcuts active"
        ;;
    *)
        echo "Unknown mode: $CURRENT_MODE"
        ;;
esac
```

**Fish shell 示例：**
```fish
set mode (zellij pipe --plugin zellij-getmode 2>/dev/null)

switch $mode
    case "locked"
        echo "Zellij is locked"
    case "normal"
        echo "Zellij is in normal mode"
    case "*"
        echo "Current mode: $mode"
end
```

### 在其他插件中使用

```rust
// 在你的 Zellij 插件中
use zellij_tile::prelude::*;

fn get_current_mode() -> String {
    pipe_message_to_plugin("zellij-getmode", "")
}

fn handle_event(&mut self, event: Event) {
    let mode = get_current_mode();
    match mode.as_str() {
        "normal" => self.handle_normal_mode(),
        "locked" => self.handle_locked_mode(),
        _ => self.handle_other_modes(),
    }
}
```

---

## 返回的模式值

| 返回值 | 说明 |
|--------|------|
| `normal` | 普通模式 - 默认模式 |
| `locked` | 锁定模式 - 键传递给应用程序 |
| `pane` | 面板模式 - 面板管理 |
| `tab` | 标签模式 - 标签管理 |
| `resize` | 调整大小模式 |
| `move` | 移动模式 |
| `scroll` | 滚动模式 |
| `session` | 会话模式 |
| `tmux` | Tmux 模式 |
| `search` | 搜索模式 |

---

## 实用示例

### 智能提示脚本

创建 `~/.local/bin/zellij-prompt`：

```bash
#!/bin/bash
# 根据 Zellij 模式显示不同的提示符

MODE=$(zellij pipe --plugin zellij-getmode 2>/dev/null)

if [ -n "$MODE" ]; then
    case "$MODE" in
        "locked")
            echo -n "🔒 "
            ;;
        "normal")
            echo -n "⌘ "
            ;;
        "pane"|"tab"|"resize")
            echo -n "🔧 "
            ;;
        *)
            echo -n "[$MODE] "
            ;;
    esac
fi
```

添加到 shell 配置：
```bash
# ~/.bashrc 或 ~/.zshrc
export PS1='$(zellij-prompt)'$PS1
```

### 模式变化通知

```bash
#!/bin/bash
# 监控模式变化

last_mode=""
while true; do
    current=$(zellij pipe --plugin zellij-getmode 2>/dev/null)
    if [ "$current" != "$last_mode" ]; then
        echo "Mode changed: $last_mode -> $current"
        last_mode="$current"
    fi
    sleep 1
done
```

---

## 故障排除

### 插件无响应
- 确认插件已正确加载
- 检查 Zellij 版本兼容性
- 尝试重新加载 Zellij

### 返回空值
- 确认在 Zellij 会话中运行
- 检查是否有权限问题
- 查看 Zellij 日志

### 模式名称不正确
- 检查 Zellij 版本
- 确认插件版本兼容性
- 查看文档更新

---

## 相关资源
- [官方仓库](https://github.com/chardskarth/zellij-getmode)
- [Zellij 模式文档](https://zellij.dev/documentation/modes.html)
- [zellij-switch](./zellij-switch.md) - 使用 pipe 的会话切换插件

---

*最后更新：2025年2月*
