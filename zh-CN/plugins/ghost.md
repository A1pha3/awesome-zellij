# ghost 插件

> 生成浮动命令终端面板（交互式 zrf）

**GitHub**: [vdbulcke/ghost](https://github.com/vdbulcke/ghost)

---

## 简介

`ghost` 是一个 Zellij 插件，允许你在当前工作区中快速生成浮动的命令终端面板。它是交互式 zrf（Zellij Run File）的实现，让你能够在不离开当前上下文的情况下运行命令。

---

## 功能特点

### 1. 浮动终端面板
- 在当前工作区中生成浮动命令终端面板
- 不干扰当前的工作布局
- 完成后可以自动关闭或保留

### 2. 交互式命令执行
- 支持交互式命令输入
- 可以运行需要用户输入的复杂命令
- 保留命令历史记录

### 3. 快速访问
- 通过快捷键快速调用
- 无需离开当前面板即可执行命令
- 支持自定义命令模板

### 4. 上下文感知
- 自动继承当前目录
- 可以使用当前环境变量
- 保持工作流的连续性

---

## 使用场景

### 场景 1: 快速运行辅助命令
当你在编辑代码时，需要快速运行一个 git 命令或测试命令，而不想切换面板：
```
快捷键 -> 输入命令 -> 查看结果 -> 关闭浮动面板
```

### 场景 2: 运行交互式工具
需要运行交互式 CLI 工具（如 npm init、数据库客户端等）：
- 数据库查询
- 交互式配置
- 命令行向导

### 场景 3: 临时任务执行
需要临时执行一些命令，但不想破坏当前的工作区布局：
- 查看日志
- 运行一次性的脚本
- 临时计算或转换

### 场景 4: 多任务并行
在主面板继续工作的同时，在浮动面板中运行长时间任务：
- 编译代码
- 运行测试套件
- 下载大文件

---

## 安装方法

### 前提条件
- 已安装 Zellij（版本 >= 0.38.0）
- 已安装 Rust 工具链（用于编译）

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/vdbulcke/ghost.git
cd ghost
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 将编译好的插件复制到 Zellij 插件目录：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/ghost.wasm ~/.config/zellij/plugins/
```

### 通过 Zellij 插件管理器安装

如果你的 Zellij 版本支持插件管理器：
```bash
zellij plugin add ghost
```

---

## 配置方法

### KDL 配置

在 `~/.config/zellij/config.kdl` 中添加插件配置：

```kdl
plugins {
    ghost location="file:~/.config/zellij/plugins/ghost.wasm" {
        // 插件特定配置
        floating true
        height "30%"
        width "50%"
    }
}
```

### 键绑定配置

添加键绑定以快速调用 ghost：

```kdl
keybinds {
    normal {
        bind "Ctrl g" {
            LaunchOrFocusPlugin "ghost" {
                floating true
                height "30%"
                width "50%"
            }
        }
    }
}
```

---

## 使用方法

### 基本使用

1. 按下配置的快捷键（如 `Ctrl+g`）
2. 浮动命令终端面板出现
3. 输入你要执行的命令
4. 查看命令输出
5. 按 `Esc` 或配置关闭键关闭面板

### 高级用法

#### 使用命令模板
你可以在配置中预设常用的命令模板：

```kdl
plugins {
    ghost location="file:~/.config/zellij/plugins/ghost.wasm" {
        templates {
            git_status "git status"
            npm_test "npm test"
            cargo_build "cargo build --release"
        }
    }
}
```

#### 与工作流集成
在布局文件中集成 ghost：

```kdl
layout {
    pane split_direction="horizontal" {
        pane
        pane size=1 {
            plugin location="ghost" {
                // 配置选项
            }
        }
    }
}
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `floating` | boolean | true | 是否以浮动窗口显示 |
| `height` | string | "30%" | 浮动窗口高度 |
| `width` | string | "50%" | 浮动窗口宽度 |
| `cwd` | string | 当前目录 | 命令执行的初始目录 |

---

## 与其他功能的对比

| 功能 | ghost | Zellij 原生 Run | 普通面板 |
|------|-------|-----------------|----------|
| 浮动显示 | ✅ | ✅ | ❌ |
| 交互式 | ✅ | ❌ | ✅ |
| 保留历史 | ✅ | ❌ | ✅ |
| 快速调用 | ✅ | ⚠️ | ❌ |
| 自动关闭 | ✅ | ✅ | ❌ |

**ghost 的优势**：
- 专为交互式命令设计
- 更流畅的用户体验
- 可以保持浮动状态

---

## 故障排除

### 插件无法加载
- 确认 `.wasm` 文件路径正确
- 检查 Zellij 版本兼容性
- 查看 Zellij 日志：`zellij --debug`

### 命令不执行
- 检查 shell 环境变量
- 确认工作目录正确
- 尝试使用绝对路径

### 浮动窗口行为异常
- 调整 height 和 width 参数
- 检查显示器分辨率
- 确保 floating 设置为 true

---

## 相关资源

- [官方仓库](https://github.com/vdbulcke/ghost)
- [Zellij 插件开发文档](https://zellij.dev/documentation/plugins.html)

---

*最后更新：2025年2月*
