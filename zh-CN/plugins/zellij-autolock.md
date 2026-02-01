# zellij-autolock 插件

> 根据焦点面板中的命令自动锁定 Zellij，为 Vim 等提供无缝导航

**GitHub**: [fresh2dev/zellij-autolock](https://github.com/fresh2dev/zellij-autolock)

**配套插件**: [zellij.vim](https://github.com/fresh2dev/zellij.vim)

---

## 简介

`zellij-autolock` 是一个智能的 Zellij 插件，它会自动检测当前焦点面板中运行的程序，并根据配置自动切换 Zellij 的锁定模式。当你在使用 Vim、Neovim 或其他编辑器时，Zellij 会自动锁定，让你可以直接使用编辑器的快捷键；当你切换到普通终端面板时，Zellij 会自动解锁。

---

## 功能特点

### 1. 智能自动锁定
- 自动检测面板中运行的命令
- 根据配置自动锁定/解锁 Zellij
- 实时响应，无感知切换

### 2. 可配置的触发器
- 自定义触发自动锁定的命令列表
- 支持部分匹配和精确匹配
- 默认支持 `nvim`、`vim`、`emacs` 等编辑器

### 3. 无缝编辑体验
- 在 Vim 中使用所有快捷键无需前缀
- 离开编辑器后自动恢复 Zellij 快捷键
- 与 vim-zellij-navigator 完美配合

### 4. 即时响应
- 焦点变化时立即检测
- 命令启动时自动触发
- 零延迟切换体验

### 5. 配套 Vim 插件
- 提供 [zellij.vim](https://github.com/fresh2dev/zellij.vim) 作为配套
- 从 Vim 内部控制 Zellij
- 双向集成体验

---

## 使用场景

### 场景 1: Vim 深度用户
大部分时间在使用 Vim/Neovim，偶尔切换到终端：
```
在 Vim 中: Zellij 自动锁定，所有键直接传给 Vim
在终端中: Zellij 自动解锁，可以使用 Zellij 快捷键
```

### 场景 2: 混合开发工作流
频繁在编辑器和终端间切换：
```
编辑代码 → Zellij 锁定
运行测试 → Zellij 解锁
查看日志 → Zellij 解锁
回到编辑 → Zellij 锁定
```

### 场景 3: 多编辑器环境
使用不同的编辑器处理不同文件：
- 用 Neovim 编辑代码 → 自动锁定
- 用 Emacs 编辑配置 → 自动锁定
- 用 nano 快速修改 → 自动锁定（如果配置）

### 场景 4: 远程服务器工作
在 SSH 会话中使用 Zellij + Vim：
- 本地和远程一致的体验
- 无需学习两套快捷键

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/fresh2dev/zellij-autolock.git
cd zellij-autolock
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-autolock.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-autolock location="file:~/.config/zellij/plugins/zellij-autolock.wasm" {
        triggers ["nvim", "vim", "emacs"]
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-autolock location="file:~/.config/zellij/plugins/zellij-autolock.wasm" {
        // 触发自动锁定的命令
        triggers ["nvim", "vim", "vi", "emacs", "nano", "helix", "hx"]
        
        // 锁定后切换到的模式
        lock_mode "locked"
        
        // 解锁后切换到的模式
        unlock_mode "normal"
        
        // 是否在启动时立即检查
        immediate_check true
    }
}

keybinds {
    // 锁定模式下的键绑定
    locked {
        // 必须保留一个解锁键！
        bind "Ctrl g" { SwitchToMode "normal"; }
        
        // 可以添加其他锁定模式下的快捷键
        bind "Ctrl q" { Quit; }
    }
}
```

---

## 使用方法

### 工作原理

```
1. 插件持续监控焦点面板
2. 检测到焦点变化或新命令启动
3. 检查面板中运行的命令
4. 如果匹配 triggers 列表 → 自动锁定
5. 如果不匹配 → 自动解锁（如果在锁定模式）
```

### 日常使用

**自动锁定：**
```bash
$ nvim main.rs
# Zellij 自动切换到 locked 模式
# 现在所有键都传给 Neovim
```

**自动解锁：**
```bash
# 在 Neovim 中 :q 退出
# 回到普通 shell
# Zellij 自动切换到 normal 模式
# 现在可以使用 Zellij 快捷键
```

**手动解锁：**
```
在锁定模式下按 Ctrl+g（或你配置的键）
临时切换到 normal 模式使用 Zellij 功能
```

---

## 推荐完整配置

### 与 vim-zellij-navigator 一起使用

```kdl
// ~/.config/zellij/config.kdl

plugins {
    zellij-autolock location="file:~/.config/zellij/plugins/zellij-autolock.wasm" {
        triggers ["nvim", "vim", "vi"]
    }
    
    vim-zellij-navigator location="file:~/.config/zellij/plugins/vim-zellij-navigator.wasm"
}

keybinds {
    normal {
        // 导航
        bind "Ctrl h" {
            MessagePlugin "vim-zellij-navigator" { move_focus "left" }
        }
        bind "Ctrl j" {
            MessagePlugin "vim-zellij-navigator" { move_focus "down" }
        }
        bind "Ctrl k" {
            MessagePlugin "vim-zellij-navigator" { move_focus "up" }
        }
        bind "Ctrl l" {
            MessagePlugin "vim-zellij-navigator" { move_focus "right" }
        }
    }
    
    locked {
        // 在锁定模式下也启用导航
        bind "Ctrl h" {
            MessagePlugin "vim-zellij-navigator" { move_focus "left" }
        }
        bind "Ctrl j" {
            MessagePlugin "vim-zellij-navigator" { move_focus "down" }
        }
        bind "Ctrl k" {
            MessagePlugin "vim-zellij-navigator" { move_focus "up" }
        }
        bind "Ctrl l" {
            MessagePlugin "vim-zellij-navigator" { move_focus "right" }
        }
        
        // 手动解锁
        bind "Ctrl g" { SwitchToMode "normal"; }
    }
}
```

### Vim 配置（可选）

如果你使用配套插件 [zellij.vim](https://github.com/fresh2dev/zellij.vim)：

```lua
-- init.lua (Neovim)
require('zellij').setup({
    -- 配置选项
})

-- 快捷键示例
vim.keymap.set('n', '<leader>zz', '<cmd>ZellijNewPane<cr>')
vim.keymap.set('n', '<leader>zh', '<cmd>ZellijMoveLeft<cr>')
vim.keymap.set('n', '<leader>zj', '<cmd>ZellijMoveDown<cr>')
vim.keymap.set('n', '<leader>zk', '<cmd>ZellijMoveUp<cr>')
vim.keymap.set('n', '<leader>zl', '<cmd>ZellijMoveRight<cr>')
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `triggers` | list | `["nvim", "vim"]` | 触发自动锁定的命令 |
| `lock_mode` | string | "locked" | 锁定后切换到的模式 |
| `unlock_mode` | string | "normal" | 解锁后切换到的模式 |
| `immediate_check` | boolean | true | 启动时立即检查当前面板 |

---

## 故障排除

### 自动锁定不工作
- 确认插件已正确加载
- 检查 `triggers` 列表包含你的编辑器
- 确认命令名称匹配（使用 `ps` 查看）

### 无法手动解锁
- 确认配置了解锁快捷键（如 `Ctrl+g`）
- 检查没有与其他快捷键冲突
- 尝试不同的解锁键

### 锁定/解锁过于频繁
- 检查 `triggers` 配置是否过于宽泛
- 避免包含 shell 或常见命令
- 使用精确匹配而非部分匹配

---

## 相关资源
- [官方仓库](https://github.com/fresh2dev/zellij-autolock)
- [zellij.vim](https://github.com/fresh2dev/zellij.vim) - 配套 Vim 插件
- [vim-zellij-navigator](./vim-zellij-navigator.md) - 导航插件
- [zjswitcher](./zjswitcher.md) - 另一个自动切换模式插件

---

*最后更新：2025年2月*
