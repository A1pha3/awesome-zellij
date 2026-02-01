# zellij-pane-picker 插件

> 快速切换、收藏和跳转到面板，支持可自定义键盘快捷键

**GitHub**: [shihanng/zellij-pane-picker](https://github.com/shihanng/zellij-pane-picker)

---

## 简介

`zellij-pane-picker` 是一个功能强大的面板管理插件，它提供了一个直观的界面来浏览、搜索和切换所有打开的面板。除了基本的切换功能，它还支持收藏常用面板和自定义键盘导航，是管理复杂工作区的理想工具。

---

## 功能特点

### 1. 面板列表
- 显示所有打开的面板
- 显示面板标题和编号
- 显示当前活动面板

### 2. 快速切换
- 一键切换到任意面板
- 支持模糊搜索面板
- 数字快捷键快速访问

### 3. 收藏面板
- 收藏常用的面板
- 收藏面板优先显示
- 快速访问重要面板

### 4. 自定义导航
- 自定义键盘快捷键
- 支持 Vim 风格导航
- 适应不同用户习惯

### 5. 面板信息
- 显示面板运行的命令
- 显示面板所在标签
- 显示面板内容预览（可选）

---

## 使用场景

### 场景 1: 多面板工作流
在多个功能面板间切换：
```
面板 1: 代码编辑器
面板 2: 测试运行器
面板 3: 日志监控
面板 4: 数据库客户端
```

### 场景 2: 收藏核心面板
标记常用的核心面板：
```
★ 主编辑器
★ 主终端
☆ 临时日志
☆ 一次性查询
```

### 场景 3: 复杂布局导航
在复杂的分屏布局中快速定位：
```
多个标签，每个标签多个面板
通过搜索快速找到需要的面板
无需记住面板位置
```

### 场景 4: 快捷键自定义
自定义符合个人习惯的快捷键：
```
Vim 用户使用 hjkl 导航
Emacs 用户使用 Ctrl+n/p
自定义数字键直接跳转
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/shihanng/zellij-pane-picker.git
cd zellij-pane-picker
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-pane-picker.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-pane-picker location="file:~/.config/zellij/plugins/zellij-pane-picker.wasm"
}

keybinds {
    normal {
        bind "Ctrl p" {
            LaunchOrFocusPlugin "zellij-pane-picker" {
                floating true
                height "60%"
                width "70%"
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-pane-picker location="file:~/.config/zellij/plugins/zellij-pane-picker.wasm" {
        // 导航键（Vim 风格）
        nav_keys {
            up "k"
            down "j"
            left "h"
            right "l"
        }
        
        // 或 Emacs 风格
        // nav_keys {
        //     up "Ctrl p"
        //     down "Ctrl n"
        //     left "Ctrl b"
        //     right "Ctrl f"
        // }
        
        // 收藏快捷键
        key_toggle_star "s"
        key_jump_to_star "Alt+数字"
        
        // 显示选项
        show_preview true
        show_command true
        show_tab_name true
        show_pane_index true
        
        // 搜索
        case_sensitive false
        
        // 布局
        max_items 20
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+p`（或你的配置键）打开 pane picker 面板。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zellij-pane-picker                                   │
├─────────────────────────────────────────────────────┤
│ > main.rs                                            │
│                                                      │
│ 1:1 ★ main.rs         Tab: code    [vim]            │
│ 1:2 ★ bash            Tab: code    [zsh]            │
│                                                      │
│ 2:1   tests           Tab: test    [cargo test]     │
│ 2:2   logs            Tab: test    [tail]           │
│                                                      │
│ 3:1   db-console      Tab: db      [psql]           │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [j/k] Navigate  [Enter] Go  [s] Star  [Esc] Quit    │
│ [Alt+1-9] Jump to Star                              │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+p` | 打开 pane picker |
| `↑/↓` 或 `j/k` | 导航面板列表 |
| `Enter` | 切换到选中面板 |
| `s` | 切换收藏状态 |
| `Alt+1~9` | 跳转到对应的收藏面板 |
| `/` | 开始搜索 |
| `Esc` | 关闭面板 |

### 工作流程

**快速切换面板：**
```
1. 按 Ctrl+p 打开 picker
2. 使用 j/k 或方向键导航
3. 按 Enter 切换到选中面板
4. 按 Esc 关闭 picker
```

**收藏重要面板：**
```
1. 打开 pane picker
2. 导航到要收藏的面板
3. 按 s 标记为收藏（显示 ★）
4. 之后使用 Alt+1~9 快速跳转
```

**搜索面板：**
```
1. 打开 pane picker
2. 按 / 开始搜索
3. 输入面板名或命令
4. 实时过滤列表
5. 按 Enter 切换到匹配面板
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `nav_keys` | map | Vim 风格 | 导航键配置 |
| `key_toggle_star` | string | "s" | 切换收藏快捷键 |
| `key_jump_to_star` | string | "Alt+数字" | 跳转到收藏面板 |
| `show_preview` | boolean | true | 显示面板预览 |
| `show_command` | boolean | true | 显示面板命令 |
| `show_tab_name` | boolean | true | 显示标签名 |
| `show_pane_index` | boolean | true | 显示面板索引 |
| `case_sensitive` | boolean | false | 搜索区分大小写 |
| `max_items` | integer | 20 | 最大显示项目数 |

---

## 与 zjpane 对比

| 特性 | zellij-pane-picker | zjpane |
|------|-------------------|--------|
| 收藏功能 | ✅ | ❌ |
| 自定义快捷键 | ✅ | ⚠️ |
| 搜索功能 | ✅ | ✅ |
| 面板预览 | ✅ | ⚠️ |
| 界面复杂度 | 较复杂 | 简洁 |

**选择建议**：
- 需要收藏功能 → zellij-pane-picker
- 追求简洁 → zjpane

---

## 故障排除

### 面板不显示
- 确认 Zellij 版本兼容
- 检查插件是否正确加载
- 重启 Zellij

### 收藏不保存
- 检查是否有写入权限
- 确认插件配置正确
- 查看日志

### 快捷键冲突
- 检查配置的快捷键
- 与其他插件的快捷键比较
- 修改配置使用不同的键

---

## 相关资源
- [官方仓库](https://github.com/shihanng/zellij-pane-picker)
- [zjpane](./zjpane.md) - 另一个面板导航插件
- [harpoon](./harpoon.md) - 面板标记插件

---

*最后更新：2025年2月*
