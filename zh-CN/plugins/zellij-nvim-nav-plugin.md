# zellij-nvim-nav-plugin 插件

> 另一个用于与 Neovim/Vim 窗口无缝导航的插件

**GitHub**: [sharph/zellij-nvim-nav-plugin](https://github.com/sharph/zellij-nvim-nav-plugin)

---

## 简介

`zellij-nvim-nav-plugin` 是另一个解决 Vim/Neovim 与 Zellij 面板之间导航问题的插件。它提供了与 [vim-zellij-navigator](./vim-zellij-navigator.md) 类似的功能，但采用了不同的实现方式，可能更适合某些用户的使用习惯。

---

## 功能特点

### 1. 无缝导航
- 在 Neovim 窗口和 Zellij 面板间无缝移动
- 统一的导航体验
- 无需记忆两套快捷键

### 2. 智能焦点管理
- 自动检测当前环境
- 智能传递焦点
- 平滑过渡

### 3. 双向支持
- 支持从 Vim 到 Zellij 的导航
- 支持从 Zellij 到 Vim 的导航
- 完整的工作流覆盖

### 4. 多种导航模式
- 方向键导航（h/j/k/l）
- 可选支持数字前缀
- 可自定义导航键

### 5. 低延迟
- 快速响应
- 最小化延迟
- 流畅的导航体验

---

## 使用场景

### 场景 1: 编辑器 + 终端布局
左侧 Vim 编辑器，右侧终端：
```
┌──────────────────┬──────────────────┐
│                  │                  │
│   Neovim         │   Terminal       │
│   (多个窗口)     │   Panel          │
│                  │                  │
└──────────────────┴──────────────────┘

使用 Ctrl+h/l 在两者之间自由导航
```

### 场景 2: 复杂 IDE 布局
多个面板组成的开发环境：
```
┌────────┬──────────────────┬──────────┐
│        │                  │          │
│ File   │   Neovim         │ Terminal │
│ Tree   │   Editor         │ Panel    │
│        │                  │          │
└────────┴──────────────────┴──────────┘

自由在所有面板间导航
```

### 场景 3: 远程开发
SSH 到远程服务器使用 Zellij + Neovim：
- 一致的体验
- 无需重新学习快捷键

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Neovim 或 Vim
- Rust 工具链

### 步骤 1: 安装 Zellij 插件

1. 克隆仓库：
```bash
git clone https://github.com/sharph/zellij-nvim-nav-plugin.git
cd zellij-nvim-nav-plugin
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-nvim-nav-plugin.wasm ~/.config/zellij/plugins/
```

### 步骤 2: 安装 Neovim 插件

使用你的插件管理器安装：

**Packer：**
```lua
use 'sharph/zellij-nvim-nav-plugin'
```

**Lazy.nvim：**
```lua
{ 'sharph/zellij-nvim-nav-plugin' }
```

**vim-plug：**
```vim
Plug 'sharph/zellij-nvim-nav-plugin'
```

---

## 配置方法

### Zellij 配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-nvim-nav-plugin location="file:~/.config/zellij/plugins/zellij-nvim-nav-plugin.wasm"
}

keybinds {
    normal {
        bind "Ctrl h" {
            MessagePlugin "zellij-nvim-nav-plugin" {
                navigate "left"
            }
        }
        bind "Ctrl j" {
            MessagePlugin "zellij-nvim-nav-plugin" {
                navigate "down"
            }
        }
        bind "Ctrl k" {
            MessagePlugin "zellij-nvim-nav-plugin" {
                navigate "up"
            }
        }
        bind "Ctrl l" {
            MessagePlugin "zellij-nvim-nav-plugin" {
                navigate "right"
            }
        }
    }
    
    // 在锁定模式下也启用
    locked {
        bind "Ctrl h" {
            MessagePlugin "zellij-nvim-nav-plugin" {
                navigate "left"
            }
        }
        bind "Ctrl j" {
            MessagePlugin "zellij-nvim-nav-plugin" {
                navigate "down"
            }
        }
        bind "Ctrl k" {
            MessagePlugin "zellij-nvim-nav-plugin" {
                navigate "up"
            }
        }
        bind "Ctrl l" {
            MessagePlugin "zellij-nvim-nav-plugin" {
                navigate "right"
            }
        }
        
        bind "Ctrl g" { SwitchToMode "normal"; }
    }
}
```

### Neovim 配置

```lua
-- init.lua
require('zellij-nvim-nav-plugin').setup({
    -- 配置选项
    nav_keys = {
        left = '<C-h>',
        down = '<C-j>',
        up = '<C-k>',
        right = '<C-l>',
    }
})
```

或者手动配置：

```lua
-- init.lua
vim.keymap.set('n', '<C-h>', function()
    require('zellij-nvim-nav-plugin').navigate_left()
end, { silent = true })

vim.keymap.set('n', '<C-j>', function()
    require('zellij-nvim-nav-plugin').navigate_down()
end, { silent = true })

vim.keymap.set('n', '<C-k>', function()
    require('zellij-nvim-nav-plugin').navigate_up()
end, { silent = true })

vim.keymap.set('n', '<C-l>', function()
    require('zellij-nvim-nav-plugin').navigate_right()
end, { silent = true })
```

---

## 使用方法

### 基本导航

| 快捷键 | 操作 |
|--------|------|
| `Ctrl+h` | 向左移动 |
| `Ctrl+j` | 向下移动 |
| `Ctrl+k` | 向上移动 |
| `Ctrl+l` | 向右移动 |

### 导航示例

**从 Vim 到 Zellij：**
```
当前在 Neovim 的右侧窗口
按 Ctrl+l → 焦点移动到右侧的 Zellij 面板
```

**从 Zellij 到 Vim：**
```
当前在 Zellij 面板（左侧是 Neovim）
按 Ctrl+h → 焦点移动到 Neovim
```

**在 Vim 内部：**
```
当前在 Neovim 的左侧窗口
按 Ctrl+l → 焦点移动到 Neovim 的右侧窗口
```

---

## 与 vim-zellij-navigator 对比

| 特性 | zellij-nvim-nav-plugin | vim-zellij-navigator |
|------|------------------------|---------------------|
| 导航方式 | 消息传递 | 命令执行 |
| Vim 插件 | 专用插件 | 专用插件 |
| 配置复杂度 | 中等 | 中等 |
| 响应速度 | 快 | 快 |
| 社区活跃度 | 较新 | 较成熟 |

**选择建议：**
- 尝试两者，看哪个更适合你的工作流
- 都可以与 zellij-autolock 配合使用

---

## 故障排除

### 导航不工作
- 确认 Zellij 插件和 Vim 插件都已安装
- 检查键绑定配置
- 确认插件已加载

### 焦点传递失败
- 确认 Zellij 面板中运行的是 Vim
- 检查 Vim 插件是否正确加载
- 查看日志

### 快捷键冲突
- 检查其他插件的快捷键
- 检查终端模拟器的快捷键
- 修改配置中的快捷键

---

## 相关资源
- [官方仓库](https://github.com/sharph/zellij-nvim-nav-plugin)
- [vim-zellij-navigator](./vim-zellij-navigator.md) - 类似功能的插件
- [zellij-autolock](./zellij-autolock.md) - 自动锁定插件

---

*最后更新：2025年2月*
