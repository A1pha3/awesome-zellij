# zellij-choose-tree 插件

> 受 tmux choose-tree 启发的快速会话切换

**GitHub**: [laperlej/zellij-choose-tree](https://github.com/laperlej/zellij-choose-tree)

---

## 简介

`zellij-choose-tree` 是一个受 tmux `choose-tree` 命令启发的 Zellij 插件，提供了一个树形视图来浏览和切换所有 Zellij 会话、窗口（标签页）和面板。它是管理多个会话的理想工具。

---

## 功能特点

### 1. 树形结构视图
- 层次化显示会话、标签页和面板
- 清晰的父子关系展示
- 类似文件管理器的浏览体验

### 2. 快速导航
- 快速在会话间切换
- 直接在树中选择目标面板
- 支持模糊搜索

### 3. 可视化概览
- 一目了然地查看所有活动会话
- 显示每个会话的标签和面板数量
- 高亮当前活动会话

### 4. 会话管理
- 从树中直接删除会话
- 重命名会话
- 创建新会话

### 5. tmux 用户友好
- 熟悉的 choose-tree 界面
- 类似的快捷键
- 平滑迁移体验

---

## 使用场景

### 场景 1: 多项目管理
同时处理多个项目，每个项目一个会话：
```
Session: project-a
  Tab: backend
  Tab: frontend
Session: project-b
  Tab: docs
Session: personal
  Tab: notes
```

### 场景 2: 服务器管理
管理多个远程服务器的会话：
```
Session: prod-web-01
  Tab: logs
  Tab: shell
Session: staging-db
  Tab: psql
  Tab: monitoring
```

### 场景 3: 开发环境概览
快速了解当前所有打开的工作环境：
- 查看哪些会话正在运行
- 快速跳转到需要的会话
- 清理不需要的会话

### 场景 4: tmux 迁移
从 tmux 迁移到 Zellij 的用户：
- 熟悉的 choose-tree 界面
- 相同的工作流
- 无缝过渡

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/laperlej/zellij-choose-tree.git
cd zellij-choose-tree
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-choose-tree.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-choose-tree location="file:~/.config/zellij/plugins/zellij-choose-tree.wasm"
}

keybinds {
    normal {
        bind "Ctrl t" {
            LaunchOrFocusPlugin "zellij-choose-tree" {
                floating true
                height "80%"
                width "80%"
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-choose-tree location="file:~/.config/zellij/plugins/zellij-choose-tree.wasm" {
        // 显示选项
        show_sessions true
        show_tabs true
        show_panes false
        
        // 树形展开
        expand_all false
        expand_current_session true
        
        // 快捷键
        key_quit "Esc"
        key_select "Enter"
        key_toggle_expand "Space"
        key_delete "d"
        key_rename "r"
        key_new "n"
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+t`（或你的配置键）打开 choose-tree 面板。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zellij-choose-tree                                   │
├─────────────────────────────────────────────────────┤
│ > myproject                                          │
│                                                      │
│ ▶ project-a                                          │
│   ├─ 1: backend                                      │
│   │  └─ bash                                         │
│   └─ 2: frontend                                     │
│      ├─ vim                                          │
│      └─ npm run dev                                  │
│                                                      │
│ ▶ project-b *                                        │
│   ├─ 1: docs                                         │
│   └─ 2: tests                                        │
│                                                      │
│ ▶ personal                                           │
│   └─ 1: notes                                        │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Enter] Switch  [Space] Toggle  [d] Delete          │
│ [r] Rename  [n] New  [Esc] Quit                     │
└─────────────────────────────────────────────────────┘
* = current session
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+t` | 打开 choose-tree |
| `↑/↓` | 导航树节点 |
| `→` | 展开节点 |
| `←` | 折叠节点 |
| `Space` | 切换展开/折叠 |
| `Enter` | 切换到选中的会话/标签/面板 |
| `d` | 删除选中的会话 |
| `r` | 重命名选中的会话 |
| `n` | 创建新会话 |
| `Esc` | 关闭面板 |

### 工作流程

**切换会话：**
```
1. 按 Ctrl+t 打开 choose-tree
2. 使用方向键导航到目标会话
3. 按 Enter 切换到该会话
4. 会话自动切换，面板关闭
```

**切换到特定面板：**
```
1. 按 Ctrl+t 打开 choose-tree
2. 导航到会话
3. 按 → 或 Space 展开会话
4. 继续导航到具体面板
5. 按 Enter 切换到该面板
```

**管理会话：**
```
1. 按 Ctrl+t 打开 choose-tree
2. 导航到要删除的会话
3. 按 d 删除
4. 按 r 重命名会话
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `show_sessions` | boolean | true | 显示会话节点 |
| `show_tabs` | boolean | true | 显示标签节点 |
| `show_panes` | boolean | false | 显示面板节点 |
| `expand_all` | boolean | false | 默认展开所有节点 |
| `expand_current_session` | boolean | true | 展开当前会话 |
| `show_preview` | boolean | true | 显示预览窗口 |

---

## 与 zellij-sessionizer 对比

| 特性 | zellij-choose-tree | zellij-sessionizer |
|------|--------------------|--------------------|
| 视图类型 | 树形结构 | 列表 |
| 显示层级 | 会话→标签→面板 | 会话 |
| 会话创建 | ✅ | ✅ |
| 基于文件夹 | ❌ | ✅ |
| 目标用户 | 管理现有会话 | 创建新会话 |

**建议同时使用**：
- zellij-choose-tree：管理现有会话
- zellij-sessionizer：基于文件夹创建新会话

---

## 故障排除

### 树不显示
- 确认 Zellij 版本兼容
- 检查插件是否正确加载
- 重启 Zellij

### 无法切换会话
- 确认目标会话存在
- 检查会话权限
- 查看 Zellij 日志

### 显示混乱
- 调整面板大小
- 检查终端字体
- 尝试不同的终端模拟器

---

## 相关资源
- [官方仓库](https://github.com/laperlej/zellij-choose-tree)
- [zellij-sessionizer](./zellij-sessionizer.md) - 会话创建插件
- [zellij-favs](./zellij-favs.md) - 收藏会话插件
- [tmux choose-tree](https://man7.org/linux/man-pages/man1/tmux.1.html)

---

*最后更新：2025年2月*
