# zellij-forgot 插件

> 快速展示和访问你的键绑定（以及更多）

**GitHub**: [karimould/zellij-forgot](https://github.com/karimould/zellij-forgot)

---

## 简介

`zellij-forgot` 是一个方便的 Zellij 插件，它可以快速显示当前的键绑定列表，帮助你记住那些不常用的快捷键。它是解决"我忘记那个快捷键是什么了"这个问题的完美方案。

---

## 功能特点

### 1. 键绑定备忘单
- 显示当前模式下的所有可用快捷键
- 按类别组织键绑定
- 显示快捷键的功能描述

### 2. 上下文感知
- 根据当前模式显示相关快捷键
- 显示当前可用的操作
- 实时更新显示内容

### 3. 模糊搜索
- 搜索特定功能或快捷键
- 快速找到需要的命令
- 支持部分匹配

### 4. 自定义备忘单
- 添加自定义快捷键说明
- 创建个人备忘单
- 导入/导出配置

### 5. 学习助手
- 帮助新用户学习 Zellij
- 快捷键记忆辅助
- 渐进式学习支持

---

## 使用场景

### 场景 1: 快捷键遗忘
忘记特定功能的快捷键时：
```
"我怎么调整面板大小来着？"
→ 打开 zellij-forgot
→ 搜索 "resize"
→ 看到 Ctrl+p + r
```

### 场景 2: 新用户学习
刚接触 Zellij 时学习快捷键：
```
打开 zellij-forgot
浏览所有可用快捷键
边看边练习
```

### 场景 3: 模式切换参考
切换到不常用的模式时查看可用操作：
```
进入 resize 模式
打开 zellij-forgot
查看 resize 模式的所有快捷键
```

### 场景 4: 自定义配置备忘
记录自己的自定义快捷键：
```
添加自定义键绑定到备忘单
随时查看自定义快捷键
与默认快捷键区分显示
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/karimould/zellij-forgot.git
cd zellij-forgot
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-forgot.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-forgot location="file:~/.config/zellij/plugins/zellij-forgot.wasm"
}

keybinds {
    normal {
        bind "Ctrl ?" {
            LaunchOrFocusPlugin "zellij-forgot" {
                floating true
                height "70%"
                width "80%"
            }
        }
    }
    
    // 在其他模式也启用
    locked {
        bind "Ctrl ?" {
            LaunchOrFocusPlugin "zellij-forgot" {
                floating true
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-forgot location="file:~/.config/zellij/plugins/zellij-forgot.wasm" {
        // 显示选项
        show_current_mode_only false
        show_all_modes true
        
        // 搜索选项
        case_sensitive false
        search_by_key true
        search_by_description true
        
        // 分组显示
        group_by_mode true
        group_by_category true
        
        // 自定义备忘单
        custom_cheatsheet "~/.config/zellij/custom-cheatsheet.md"
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+?`（或你的配置键）打开 zellij-forgot 面板。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zellij-forgot - Keybinding Cheatsheet               │
├─────────────────────────────────────────────────────┤
│ > resize                                            │
│                                                      │
│ Normal Mode                                          │
│   Ctrl+p r     - Enter resize mode                  │
│   Alt + ←→↑↓   - Resize pane                        │
│                                                      │
│ Resize Mode                                          │
│   ←→↑↓         - Resize pane                        │
│   =            - Equalize all panes                 │
│   Esc          - Exit resize mode                   │
│                                                      │
│ Other relevant commands:                             │
│   Ctrl+p f     - Toggle fullscreen                  │
│   Ctrl+p z     - Toggle pane frames                 │
├─────────────────────────────────────────────────────┤
│ [↑/↓] Navigate  [Esc] Quit  [/] Search              │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+?` | 打开备忘单 |
| `↑/↓` | 导航列表 |
| `/` | 开始搜索 |
| `Esc` | 关闭备忘单 |
| `Tab` | 切换分组视图 |
| `m` | 切换显示所有模式 |

### 工作流程

**查找快捷键：**
```
1. 按 Ctrl+? 打开备忘单
2. 开始输入功能名称（如 "resize"）
3. 实时过滤显示相关快捷键
4. 查看需要的快捷键
5. 按 Esc 关闭备忘单，执行快捷键
```

**学习新快捷键：**
```
1. 打开备忘单
2. 浏览不同模式的快捷键
3. 尝试使用学到的快捷键
4. 再次打开备忘单强化记忆
```

**查看自定义快捷键：**
```
1. 配置 custom_cheatsheet 文件
2. 打开备忘单
3. 自定义快捷键与默认快捷键一起显示
```

---

## 自定义备忘单

### 创建自定义备忘单文件

创建 `~/.config/zellij/custom-cheatsheet.md`：

```markdown
# My Custom Keybindings

## Quick Actions
- Ctrl+Alt+t - Open terminal in new tab
- Ctrl+Alt+f - Open file manager
- Ctrl+Alt+g - Open git ui

## Development
- Ctrl+Shift+b - Build project
- Ctrl+Shift+t - Run tests
- Ctrl+Shift+d - Start debugger

## Navigation
- Alt+h/j/k/l - Navigate panes (vim style)
```

### 在配置中引用

```kdl
plugins {
    zellij-forgot location="file:~/.config/zellij/plugins/zellij-forgot.wasm" {
        custom_cheatsheet "~/.config/zellij/custom-cheatsheet.md"
    }
}
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `show_current_mode_only` | boolean | false | 只显示当前模式的快捷键 |
| `show_all_modes` | boolean | true | 显示所有模式的快捷键 |
| `case_sensitive` | boolean | false | 搜索区分大小写 |
| `search_by_key` | boolean | true | 按快捷键搜索 |
| `search_by_description` | boolean | true | 按描述搜索 |
| `group_by_mode` | boolean | true | 按模式分组显示 |
| `group_by_category` | boolean | true | 按类别分组显示 |
| `custom_cheatsheet` | string | 无 | 自定义备忘单文件路径 |

---

## 与 Zellij 帮助系统对比

| 特性 | zellij-forgot | Zellij 内置帮助 |
|------|---------------|-----------------|
| 搜索功能 | ✅ 模糊搜索 | ❌ |
| 上下文感知 | ✅ | ⚠️ |
| 自定义内容 | ✅ | ❌ |
| 快捷键显示 | ✅ | ✅ |
| 学习辅助 | ✅ 搜索 | ✅ 完整列表 |

**建议**：两者互补使用
- zellij-forgot：快速查找特定快捷键
- 内置帮助：查看完整的快捷键参考

---

## 故障排除

### 备忘单不显示
- 确认插件已正确加载
- 检查键绑定配置
- 重启 Zellij

### 搜索无结果
- 检查搜索关键词
- 确认 `search_by_key` 或 `search_by_description` 已启用
- 尝试不同的搜索词

### 自定义备忘单不加载
- 检查文件路径
- 确认文件格式正确
- 查看 Zellij 日志

---

## 相关资源
- [官方仓库](https://github.com/karimould/zellij-forgot)
- [Zellij 快捷键文档](https://zellij.dev/documentation/keybindings.html)
- [zellij-forgot](./zellij-forgot.md) - 另一个快捷键查看插件

---

*最后更新：2025年2月*
