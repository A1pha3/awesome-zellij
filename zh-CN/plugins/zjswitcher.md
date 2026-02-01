# zjswitcher 插件

> 在普通模式和锁定模式间自动切换

**GitHub**: [WingsZeng/zjswitcher](https://github.com/WingsZeng/zjswitcher)

---

## 简介

`zjswitcher` 是一个自动模式切换插件，它会根据当前面板中运行的程序自动在 Zellij 的普通模式和锁定模式之间切换。这与 [zellij-autolock](./zellij-autolock.md) 类似，但采用了不同的实现方式。

---

## 功能特点

### 1. 自动模式切换
- 自动检测面板中的程序
- 智能切换普通/锁定模式
- 无缝过渡

### 2. 可配置触发器
- 自定义触发锁定的程序列表
- 支持正则匹配
- 灵活配置

### 3. 程序感知
- 识别编辑器（vim、emacs 等）
- 识别其他交互式程序
- 支持自定义程序

### 4. 即时响应
- 焦点变化时立即检测
- 低延迟切换
- 流畅体验

---

## 使用场景

### 场景 1: Vim/Neovim 用户
使用 Vim 时自动锁定：
```
在 Vim 中编辑 → 自动锁定 → 所有键传给 Vim
退出 Vim → 自动解锁 → 可以使用 Zellij 快捷键
```

### 场景 2: 混合工作流
在编辑器和终端间切换：
```
编辑器 → 锁定模式
终端 → 普通模式
自动切换，无需手动操作
```

### 场景 3: 其他编辑器
支持各种编辑器：
```
Emacs
Nano
Helix
自定义编辑器
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/WingsZeng/zjswitcher.git
cd zjswitcher
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zjswitcher.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zjswitcher location="file:~/.config/zellij/plugins/zjswitcher.wasm" {
        triggers ["nvim", "vim", "vi", "emacs", "nano", "helix", "hx"]
    }
}

keybinds {
    locked {
        bind "Ctrl g" { SwitchToMode "normal"; }
    }
}
```

### 高级配置

```kdl
plugins {
    zjswitcher location="file:~/.config/zellij/plugins/zjswitcher.wasm" {
        // 触发锁定的程序
        triggers ["nvim", "vim", "vi"]
        
        // 使用正则匹配
        use_regex false
        
        // 锁定模式
        lock_mode "locked"
        
        // 普通模式
        normal_mode "normal"
        
        // 延迟（毫秒）
        delay 100
    }
}
```

---

## 与 zellij-autolock 对比

| 特性 | zjswitcher | zellij-autolock |
|------|-----------|-----------------|
| 实现方式 | 不同 | 不同 |
| 配置复杂度 | 简单 | 中等 |
| 功能 | 基础 | 丰富 |
| 性能 | 快 | 快 |

**选择建议**：
- 尝试两者，选择更适合的
- 都可以实现自动锁定功能

---

## 相关资源
- [官方仓库](https://github.com/WingsZeng/zjswitcher)
- [zellij-autolock](./zellij-autolock.md) - 类似的自动锁定插件
- [vim-zellij-navigator](./vim-zellij-navigator.md)

---

*最后更新：2025年2月*
