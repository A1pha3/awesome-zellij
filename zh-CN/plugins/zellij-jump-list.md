# zellij-jump-list 插件

> 在面板间导航跳转（类似 Vim、Neovim 和 Emacs 的跳转列表）

**GitHub**: [blank2121/zellij-jump-list](https://github.com/blank2121/zellij-jump-list)

---

## 简介

`zellij-jump-list` 是一个受 Vim、Neovim 和 Emacs 跳转列表（jump list）启发的 Zellij 插件。它记录你在面板之间的跳转历史，让你可以快速返回之前访问过的面板，大大提升多面板工作流的效率。

---

## 功能特点

### 1. 自动记录跳转历史
- 自动记录面板之间的跳转
- 维护完整的访问历史
- 跨标签页记录

### 2. 双向导航
- 跳转到之前访问的面板（后退）
- 跳转到之后访问的面板（前进）
- 类似浏览器的后退/前进体验

### 3. 跳转列表查看
- 查看完整的跳转历史
- 显示面板信息和上下文
- 直接跳转到列表中的任意位置

### 4. 持久化存储
- 会话间保留跳转历史
- 可配置历史记录数量
- 支持清除历史

### 5. 键盘驱动
- 快捷键快速后退/前进
- 无需离开键盘
- 符合 Vim 用户习惯

---

## 使用场景

### 场景 1: 代码审查流程
在不同面板间跳转后返回：
```
在编辑器面板 → 跳转到日志面板查看 → 跳转到终端运行命令
→ 按快捷键返回编辑器面板
```

### 场景 2: 调试工作流
在多个调试面板间切换：
```
代码面板 → 断点面板 → 变量面板 → 控制台面板
→ 使用后退键快速返回代码面板
```

### 场景 3: 文档查阅
查看文档后返回工作区：
```
工作面板 → 文档面板查看 API → 按后退键返回工作面板
```

### 场景 4: 多标签导航
在多个标签和面板间导航：
```
在项目 A 的标签中工作 → 切换到项目 B 查看 → 使用跳转列表返回项目 A
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/blank2121/zellij-jump-list.git
cd zellij-jump-list
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-jump-list.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-jump-list location="file:~/.config/zellij/plugins/zellij-jump-list.wasm"
}

keybinds {
    normal {
        // 后退 - 跳转到之前访问的面板
        bind "Ctrl o" {
            MessagePlugin "zellij-jump-list" {
                jump_back
            }
        }
        
        // 前进 - 跳转到之后访问的面板
        bind "Ctrl i" {
            MessagePlugin "zellij-jump-list" {
                jump_forward
            }
        }
        
        // 打开跳转列表面板
        bind "Ctrl j" {
            LaunchOrFocusPlugin "zellij-jump-list" {
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
    zellij-jump-list location="file:~/.config/zellij/plugins/zellij-jump-list.wasm" {
        // 历史记录数量
        max_history 100
        
        // 是否包含同一面板的重复访问
        include_duplicates false
        
        // 是否持久化存储
        persist_history true
        
        // 历史文件路径
        history_file "~/.cache/zellij/jump-list.history"
    }
}
```

---

## 使用方法

### 基本导航

**后退（跳到之前访问的面板）：**
```
Ctrl+o    - 跳转到之前访问的面板
```

**前进（跳到之后访问的面板）：**
```
Ctrl+i    - 跳转到之后访问的面板
```

### 查看跳转列表

按 `Ctrl+j` 打开跳转列表面板：

```
┌─────────────────────────────────────────────────────┐
│ zellij-jump-list                                     │
├─────────────────────────────────────────────────────┤
│                                                      │
│   ◄ 2. docs/tab-1/pane-1        "README.md"         │
│   ◄ 1. project/src/main.rs      "main.rs"           │
│ ★ 0. project/terminal           "bash"              │
│   ► 1. project/logs             "tail -f app.log"   │
│   ► 2. project/tests            "cargo test"        │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Ctrl+o] Back  [Ctrl+i] Forward  [Enter] Go         │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+o` | 后退 - 跳转到之前访问的面板 |
| `Ctrl+i` | 前进 - 跳转到之后访问的面板 |
| `Ctrl+j` | 打开跳转列表面板 |
| `↑/↓` | 在列表中导航 |
| `Enter` | 跳转到选中的面板 |
| `d` | 从历史中删除选中项 |
| `c` | 清除所有历史 |
| `Esc` | 关闭面板 |

### 工作流程示例

**典型的代码审查流程：**
```
1. 在 main.rs 面板中编辑代码（位置 0）
2. 跳转到日志面板查看输出（位置 1）
3. 跳转到终端面板运行测试（位置 2）
4. 需要返回代码面板:
   - 按 Ctrl+o 一次 → 回到日志面板（位置 1）
   - 按 Ctrl+o 再次 → 回到 main.rs 面板（位置 0）
5. 修复代码后想回到终端面板:
   - 按 Ctrl+i 两次 → 前进到终端面板（位置 2）
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `max_history` | integer | 100 | 最大历史记录数量 |
| `include_duplicates` | boolean | false | 记录同一面板的重复访问 |
| `persist_history` | boolean | true | 会话间持久化历史 |
| `history_file` | string | 默认缓存目录 | 历史文件存储路径 |

---

## 与 Vim 跳转列表对比

| 特性 | zellij-jump-list | Vim 跳转列表 |
|------|------------------|--------------|
| 导航对象 | Zellij 面板 | 代码位置 |
| 快捷键 | Ctrl+o/i | Ctrl+o/i |
| 历史记录 | 面板访问顺序 | 代码跳转位置 |
| 持久化 | 可选 | 默认关闭 |

**相似之处：**
- 相同的快捷键（Ctrl+o/i）
- 相似的后退/前进语义
- 都维护跳转历史

**不同之处：**
- zellij-jump-list 管理面板导航
- Vim jump list 管理代码位置导航
- 两者可以结合使用

---

## 与 harpoon 对比

| 特性 | zellij-jump-list | harpoon |
|------|------------------|---------|
| 自动记录 | ✅ | ❌ |
| 手动标记 | ❌ | ✅ |
| 顺序访问 | ✅ 历史顺序 | ✅ 数字标记 |
| 目标 | 任意面板 | 预设面板 |

**建议组合使用**：
- zellij-jump-list：自动记录所有面板跳转
- harpoon：快速访问固定的重要面板

---

## 故障排除

### 导航不工作
- 确认插件已正确加载
- 检查键绑定配置
- 确认有跳转历史

### 历史不保存
- 检查 `persist_history` 设置为 true
- 确认 `history_file` 路径有写入权限
- 重启 Zellij

### 跳转到已关闭面板
- 插件会自动跳过已关闭的面板
- 如果目标面板不存在，会显示提示
- 历史会自动清理无效条目

---

## 相关资源
- [官方仓库](https://github.com/blank2121/zellij-jump-list)
- [harpoon](./harpoon.md) - 面板标记插件
- [zjpane](./zjpane.md) - 面板导航插件
- [Vim 跳转列表](https://vimhelp.org/motion.txt.html#jump-motions)

---

*最后更新：2025年2月*
