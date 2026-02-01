# vim-zellij-navigator 插件

> 在 Zellij 中与 Vim 无缝导航

**GitHub**: [hiasr/vim-zellij-navigator](https://github.com/hiasr/vim-zellij-navigator)

---

## 简介

`vim-zellij-navigator` 解决了 Vim/Neovim 用户在使用 Zellij 时的一个常见痛点：在 Vim 分屏和 Zellij 面板之间导航时的不连贯性。它允许你使用统一的快捷键（`Ctrl+h/j/k/l`）在 Vim 窗口和 Zellij 面板之间无缝导航。

---

## 功能特点

### 1. 统一导航体验
- 使用相同的快捷键在 Vim 和 Zellij 之间导航
- `Ctrl+h/j/k/l` 统一导航方向
- 自动检测当前焦点在 Vim 还是 Zellij

### 2. 智能边界检测
- 当导航到 Vim 窗口边界时，自动切换到 Zellij 面板
- 当在 Zellij 中导航到 Vim 面板时，焦点正确传递给 Vim
- 无需手动切换或记忆两套快捷键

### 3. 支持 Neovim 和 Vim
- 兼容 Neovim
- 兼容传统 Vim
- 支持 Tmux Navigator 风格的配置

### 4. 零配置（可选）
- 安装后即可使用
- 提供推荐的键绑定配置
- 支持自定义快捷键

### 5. 模式感知
- 自动检测当前 Zellij 模式
- 在锁定模式下也能正常工作
- 与 zellij-autolock 完美配合

---

## 使用场景
### 场景 1: Vim + 终端开发工作流
左侧是 Vim 编辑器，右侧是终端面板：
```
┌─────────────────┬───────────────┐
│                 │               │
│   Vim (3窗口)   │   Terminal    │
│                 │   Panel       │
│                 │               │
└─────────────────┴───────────────┘

Ctrl+l (在 Vim 最右窗口) → 跳转到 Terminal
Ctrl+h (在 Terminal) → 跳回 Vim
```

### 场景 2: 多面板 IDE 布局
模拟 IDE 布局，代码、终端、文件树一体化：
```
┌──────────┬─────────────────┬──────────┐
│          │                 │          │
│  File    │   Vim Editor    │  Terminal│
│  Tree    │   (多个窗口)    │  Panel   │
│          │                 │          │
└──────────┴─────────────────┴──────────┘

自由在各个面板间导航，无需思考当前在哪个环境
```

### 场景 3: 全栈开发
同时查看前端、后端和数据库面板：
- 在 Vim 中编辑代码
- 快速切换到终端运行命令
- 再快速切换回 Vim 继续编辑

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Vim/Neovim 已安装
- Rust 工具链

### 步骤 1: 安装 Zellij 插件

1. 克隆仓库：
```bash
git clone https://github.com/hiasr/vim-zellij-navigator.git
cd vim-zellij-navigator
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/vim-zellij-navigator.wasm ~/.config/zellij/plugins/
```

### 步骤 2: 安装 Vim 插件

#### 使用 vim-plug（Neovim）

```vim
" init.lua (Neovim)
{
    'hiasr/vim-zellij-navigator',
    lazy = false,
}
```

#### 使用 vim-plug（Vim）

```vim
" .vimrc
Plug 'hiasr/vim-zellij-navigator'
```

#### 使用 packer.nvim

```lua
-- init.lua
use {
    'hiasr/vim-zellij-navigator',
}
```

---

## 配置方法

### Zellij 配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    vim-zellij-navigator location="file:~/.config/zellij/plugins/vim-zellij-navigator.wasm"
}

keybinds {
    // 在普通模式下使用导航键
    normal {
        bind "Ctrl h" {
            MessagePlugin "vim-zellij-navigator" {
                move_focus "left"
            }
        }
        bind "Ctrl j" {
            MessagePlugin "vim-zellij-navigator" {
                move_focus "down"
            }
        }
        bind "Ctrl k" {
            MessagePlugin "vim-zellij-navigator" {
                move_focus "up"
            }
        }
        bind "Ctrl l" {
            MessagePlugin "vim-zellij-navigator" {
                move_focus "right"
            }
        }
    }
    
    // 在锁定模式下也启用导航（推荐）
    locked {
        bind "Ctrl h" {
            MessagePlugin "vim-zellij-navigator" {
                move_focus "left"
            }
        }
        bind "Ctrl j" {
            MessagePlugin "vim-zellij-navigator" {
                move_focus "down"
            }
        }
        bind "Ctrl k" {
            MessagePlugin "vim-zellij-navigator" {
                move_focus "up"
            }
        }
        bind "Ctrl l" {
            MessagePlugin "vim-zellij-navigator" {
                move_focus "right"
            }
        }
    }
}
```

### Neovim 配置

#### 使用 Lua（init.lua）

```lua
-- 基础配置
require('vim-zellij-navigator').setup({
    -- 禁用默认键绑定（如果你想自定义）
    disable_nav_default_keybindings = false,
    -- 禁用默认保存键绑定
    disable_nav_default_save_keybindings = false,
})

-- 或者使用默认配置
require('vim-zellij-navigator').setup()
```

#### 自定义键绑定

```lua
-- 如果你禁用了默认键绑定
vim.keymap.set('n', '<C-h>', require('vim-zellij-navigator').move_left)
vim.keymap.set('n', '<C-j>', require('vim-zellij-navigator').move_down)
vim.keymap.set('n', '<C-k>', require('vim-zellij-navigator').move_up)
vim.keymap.set('n', '<C-l>', require('vim-zellij-navigator').move_right)
```

### Vim 配置

```vim
" .vimrc
" 插件会自动设置键绑定
" 如果需要自定义：
let g:vim_zellij_navigator_no_default_key_mappings = 1

nnoremap <silent> <C-h> :ZellijNavigateLeft<CR>
nnoremap <silent> <C-j> :ZellijNavigateDown<CR>
nnoremap <silent> <C-k> :ZellijNavigateUp<CR>
nnoremap <silent> <C-l> :ZellijNavigateRight<CR>
```

---

## 使用方法

### 基本使用

安装完成后，使用以下快捷键导航：

| 快捷键 | 操作 |
|--------|------|
| `Ctrl+h` | 向左移动焦点 |
| `Ctrl+j` | 向下移动焦点 |
| `Ctrl+k` | 向上移动焦点 |
| `Ctrl+l` | 向右移动焦点 |

### 导航示例

**示例 1: Vim 内部导航**
```
Vim 窗口布局：
┌──────────┬──────────┐
│          │          │
│  窗口 1  │  窗口 2  │
│   ◄──────┼──────►   │
│  (Ctrl+l)│  (Ctrl+h)│
│          │          │
└──────────┴──────────┘

在窗口1按 Ctrl+l → 跳转到窗口2
在窗口2按 Ctrl+h → 跳转到窗口1
```

**示例 2: Vim 到 Zellij 导航**
```
整体布局：
┌──────────────┬──────────────┐
│              │              │
│   Vim        │   Terminal   │
│   (窗口1)    │   (Zellij)   │
│              │              │
│   ◄──────────┼──────────►   │
│  (在 Terminal│ (在 Vim      │
│   按 Ctrl+h) │  最右窗口     │
│              │  按 Ctrl+l)  │
└──────────────┴──────────────┘
```

**示例 3: 复杂布局导航**
```
┌────────┬──────────┬──────────┐
│        │          │          │
│  File  │   Vim    │  Terminal│
│  Tree  │  (多窗口)│  Panel   │
│        │          │          │
└────────┴──────────┴──────────┘

从 File Tree 到 Terminal:
Ctrl+l → Ctrl+l

从 Terminal 回到 Vim:
Ctrl+h
```

---

## 与 zellij-autolock 的集成

推荐与 [zellij-autolock](zellij-autolock.md) 一起使用：

```kdl
// config.kdl
plugins {
    zellij-autolock location="file:~/.config/zellij/plugins/zellij-autolock.wasm" {
        triggers ["nvim", "vim"]
    }
    vim-zellij-navigator location="file:~/.config/zellij/plugins/vim-zellij-navigator.wasm"
}

keybinds {
    locked {
        // 在锁定模式下也能导航
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
        
        // 解锁键
        bind "Ctrl g" { SwitchToMode "normal"; }
    }
}
```

---

## 故障排除

### 导航不工作
1. 确认插件在 Zellij 中正确加载
2. 确认 Vim 插件已安装
3. 检查键绑定冲突

### 焦点传递失败
1. 确认 Zellij 面板运行的是 Vim
2. 检查 Vim 是否为最新版本
3. 查看 Zellij 日志

### 快捷键冲突
- 检查其他 Zellij 插件的快捷键
- 检查终端模拟器的快捷键
- 在配置中修改快捷键

---

## 相关资源
- [官方仓库](https://github.com/hiasr/vim-zellij-navigator)
- [zellij-autolock](./zellij-autolock.md) - 自动锁定插件
- [christoomey/vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator) - Tmux 版本

---

*最后更新：2025年2月*
