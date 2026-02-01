# zj-quit 插件

> Zellij 的友好退出插件

**GitHub**: [cristiand391/zj-quit](https://github.com/cristiand391/zj-quit)

---

## 简介

`zj-quit` 是一个友好的退出插件，它提供了一个可视化的确认界面来退出 Zellij。这可以防止意外退出，特别是在长时间运行的会话中。

---

## 功能特点

### 1. 退出确认
- 可视化退出确认对话框
- 防止意外退出
- 显示会话信息

### 2. 选项丰富
- 完全退出 Zellij
- 仅分离会话
- 取消退出

### 3. 会话信息
- 显示当前会话名称
- 显示运行中的命令
- 提示未保存的工作

### 4. 快捷键支持
- 快速选择选项
- 键盘驱动
- 符合直觉的操作

---

## 使用场景

### 场景 1: 防止误操作
避免意外按到退出键：
```
需要确认才能退出
避免丢失工作进度
```

### 场景 2: 选择退出方式
选择是退出还是分离：
```
退出 Zellij 完全
分离会话保持后台运行
取消继续工作
```

### 场景 3: 查看会话状态
退出前确认重要信息：
```
查看是否有运行中的任务
确认会话名称
防止误关闭
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/cristiand391/zj-quit.git
cd zj-quit
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zj-quit.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zj-quit location="file:~/.config/zellij/plugins/zj-quit.wasm"
}

keybinds {
    normal {
        bind "Ctrl q" {
            LaunchOrFocusPlugin "zj-quit" {
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

### 启动插件

按 `Ctrl+q`（或你的配置键）打开退出确认对话框。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ Exit Zellij?                                         │
├─────────────────────────────────────────────────────┤
│                                                      │
│ Session: myproject                                   │
│                                                      │
│ ⚠️  Warning: There are running processes            │
│                                                      │
│ [q] Quit Zellij                                      │
│ [d] Detach Session                                   │
│ [c] Cancel                                           │
│                                                      │
└─────────────────────────────────────────────────────┘
```

---

## 相关资源
- [官方仓库](https://github.com/cristiand391/zj-quit)

---

*最后更新：2025年2月*
