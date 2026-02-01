# zbuffers 插件

> 在标签页间切换的简洁方式，受 Emacs vertico-buffers 和 Zellij session-manager 启发

**GitHub**: [Strech/zbuffers](https://github.com/Strech/zbuffers)

---

## 简介

`zbuffers` 是一个优雅的 Zellij 标签页切换插件，灵感来自 Emacs 的 vertico-buffers 和 Zellij 自带的 session-manager。它提供了一个直观的界面来查看、搜索和切换所有打开的标签页。

---

## 功能特点

### 1. 标签页缓冲区列表
- 显示所有打开的标签页（类似缓冲区列表）
- 显示标签页编号和名称
- 高亮当前活动标签

### 2. 模糊搜索
- 实时模糊搜索标签名
- 快速过滤大量标签
- 智能排序算法

### 3. 快速切换
- 一键切换到任意标签
- 支持数字快捷键直接跳转
- 无需离开键盘

### 4. 标签管理
- 关闭不需要的标签
- 重命名标签
- 查看标签详情

### 5. Emacs 风格
- 受 Emacs vertico 启发
- 熟悉的 minibuffer 体验
- 适合 Emacs 用户

---

## 使用场景

### 场景 1: Emacs 用户迁移
从 Emacs 转来的用户熟悉的缓冲区管理：
```
类似 Emacs C-x C-b 的体验
但更加现代化和可视化
```

### 场景 2: 多项目开发
同时处理多个项目，每个项目在独立标签：
```
1. Project-A-Backend
2. Project-A-Frontend
3. Project-B-Docs
4. Personal-Notes
```

### 场景 3: 标签快速清理
快速查看和关闭不需要的标签：
- 打开 zbuffers
- 浏览所有标签
- 一键关闭不需要的

### 场景 4: 会话恢复后
恢复 Zellij 会话后快速了解当前状态：
- 查看所有恢复的标签
- 快速跳转到工作标签

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/Strech/zbuffers.git
cd zbuffers
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zbuffers.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zbuffers location="file:~/.config/zellij/plugins/zbuffers.wasm"
}

keybinds {
    normal {
        bind "Ctrl b" {
            LaunchOrFocusPlugin "zbuffers" {
                floating true
                height "50%"
                width "60%"
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zbuffers location="file:~/.config/zellij/plugins/zbuffers.wasm" {
        // 显示设置
        show_index true
        show_pane_count true
        show_activity_indicator true
        
        // 搜索设置
        case_sensitive false
        max_results 30
        
        // 快捷键
        key_quit "Esc"
        key_select "Enter"
        key_next "Ctrl n"
        key_prev "Ctrl p"
        key_delete "Ctrl d"
        key_rename "Ctrl r"
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+b`（或你的配置键）打开 zbuffers 面板。

### 界面说明

```
┌─────────────────────────────────────────────┐
│ zbuffers - Tab Buffers                       │
├─────────────────────────────────────────────┤
│ > api                                        │
│                                              │
│   1  frontend-dev        [3 panes]    *      │
│   2  backend-api         [2 panes]           │
│   3  documentation       [1 pane]            │
│   4  test-results        [4 panes]           │
│                                              │
├─────────────────────────────────────────────┤
│ C-n/p: Navigate  Enter: Switch  C-d: Delete │
│ C-r: Rename      Esc: Quit                   │
└─────────────────────────────────────────────┘
* = current tab
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+b` | 打开/关闭 zbuffers |
| `Ctrl+n` / `↓` | 下一个标签 |
| `Ctrl+p` / `↑` | 上一个标签 |
| `Enter` | 切换到选中标签 |
| `Ctrl+d` | 删除选中标签 |
| `Ctrl+r` | 重命名选中标签 |
| `Esc` | 关闭 zbuffers |
| `1-9` | 直接跳转到对应编号标签 |

### 工作流程

**切换标签：**
```
1. 按 Ctrl+b 打开 zbuffers
2. 使用 Ctrl+n/p 或方向键选择标签
3. 按 Enter 切换到该标签
4. 按 Esc 关闭 zbuffers（或保持打开）
```

**搜索标签：**
```
1. 按 Ctrl+b 打开 zbuffers
2. 开始输入标签名
3. 列表实时过滤
4. 按 Enter 切换到匹配的标签
```

**管理标签：**
```
1. 按 Ctrl+b 打开 zbuffers
2. 选中要删除的标签
3. 按 Ctrl+d 删除
4. 或按 Ctrl+r 重命名
```

---

## 与 session-manager 对比

| 特性 | zbuffers | Zellij session-manager |
|------|----------|------------------------|
| 作用范围 | 当前会话的标签 | 所有会话 |
| 搜索功能 | ✅ 模糊搜索 | ✅ 模糊搜索 |
| 管理功能 | ✅ 关闭、重命名 | ✅ 切换、删除 |
| 界面风格 | Emacs vertico | 简洁列表 |
| 目标用户 | Emacs 用户 | 所有用户 |

**建议同时使用**：
- zbuffers：管理当前会话的标签
- session-manager：管理所有会话

---

## 故障排除

### 标签不显示
- 确认 Zellij 版本兼容
- 检查插件是否正确加载
- 尝试重启 Zellij

### 搜索无结果
- 检查 `case_sensitive` 设置
- 确认标签名输入正确
- 检查 `max_results` 设置

### 快捷键冲突
- 检查与 Emacs 键绑定的冲突
- 修改 Zellij 配置中的快捷键
- 检查终端模拟器设置

---

## 相关资源
- [官方仓库](https://github.com/Strech/zbuffers)
- [Emacs vertico](https://github.com/minad/vertico)
- [Zellij session-manager](https://github.com/zellij-org/zellij)
- [room](./room.md) - 类似的标签切换插件

---

*最后更新：2025年2月*
