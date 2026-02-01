# zellij-notepad 插件

> 浮动记事本面板，支持可配置编辑器、位置和时间戳笔记

**GitHub**: [0xble/zellij-notepad](https://github.com/0xble/zellij-notepad)

---

## 简介

`zellij-notepad` 是一个实用的 Zellij 插件，提供了一个浮动记事本面板。它非常适合在开发过程中快速记录想法、TODO 列表或临时笔记，无需离开 Zellij 环境。

---

## 功能特点

### 1. 浮动面板
- 以浮动窗口形式显示
- 不干扰主工作区布局
- 可调节位置和大小

### 2. 可配置编辑器
- 支持配置首选编辑器
- 支持 vim、nano、emacs 等
- 支持 GUI 编辑器（如果可用）

### 3. 时间戳笔记
- 自动添加时间戳
- 记录笔记创建时间
- 便于后续追溯

### 4. 笔记管理
- 快速打开/关闭记事本
- 笔记自动保存
- 支持多个笔记文件

### 5. 自定义位置
- 配置浮动面板位置
- 可设置为全屏或特定区域
- 适应不同工作流

---

## 使用场景

### 场景 1: 开发笔记
在编码时记录想法：
```
想法：重构这个模块
TODO：添加错误处理
记住：检查边界情况
```

### 场景 2: 会议记录
在终端工作时快速记录：
```
会议要点：
- [2025-01-30 10:30] 讨论了新功能
- [2025-01-30 10:45] 确定了时间表
```

### 场景 3: 调试跟踪
记录调试过程中的发现：
```
调试笔记：
- 错误发生在 line 127
- 变量 x 的值为 null
- 需要检查初始化逻辑
```

### 场景 4: 临时存储
临时存储代码片段或命令：
```
临时保存：
sql = "SELECT * FROM users WHERE active = 1"
curl = "curl -X POST https://api.example.com/data"
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- 已安装编辑器（vim、nano 等）
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/0xble/zellij-notepad.git
cd zellij-notepad
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-notepad.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-notepad location="file:~/.config/zellij/plugins/zellij-notepad.wasm"
}

keybinds {
    normal {
        bind "Ctrl n" {
            LaunchOrFocusPlugin "zellij-notepad" {
                floating true
                height "40%"
                width "50%"
                x "25%"
                y "30%"
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-notepad location="file:~/.config/zellij/plugins/zellij-notepad.wasm" {
        // 编辑器配置
        editor "vim"  // 或 "nano", "emacs", "$EDITOR"
        
        // 笔记文件
        notes_file "~/.zellij-notes.md"
        
        // 时间戳格式
        timestamp_format "[%Y-%m-%d %H:%M] "
        
        // 自动添加时间戳
        auto_timestamp true
        
        // 浮动面板配置
        x "25%"
        y "30%"
        width "50%"
        height "40%"
        
        // 快捷键
        key_quit "Ctrl+q"
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+n`（或你的配置键）打开记事本面板。

### 界面说明

打开后，插件会启动配置的编辑器并加载笔记文件：

```
┌─────────────────────────────────────────────────────┐
│ ~/.zellij-notes.md - VIM                            │
├─────────────────────────────────────────────────────┤
│                                                      │
│ [2025-01-30 09:00] 项目构思                          │
│ - 实现用户认证系统                                   │
│ - 添加仪表板页面                                     │
│                                                      │
│ [2025-01-30 10:30] 调试发现                          │
│ - 数据库连接池配置有问题                             │
│ - 需要增加重试逻辑                                   │
│                                                      │
│                                                      │
└─────────────────────────────────────────────────────┘
```

### 工作流程

**快速记录笔记：**
```
1. 按 Ctrl+n 打开记事本
2. 编辑器自动打开笔记文件
3. 在光标位置添加新笔记
4. 保存并退出编辑器
5. 浮动面板自动关闭
```

**持续记录模式：**
```
1. 按 Ctrl+n 打开记事本
2. 编辑笔记
3. 保存文件但不退出编辑器
4. 继续工作
5. 需要时再次按 Ctrl+n 快速回到记事本
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+n` | 打开/关闭记事本 |
| `Ctrl+q` | 退出记事本（在编辑器内） |

编辑器内的快捷键取决于你配置的编辑器。

---

## 笔记文件格式

### 默认格式

```markdown
# Zellij Notes

[2025-01-30 09:15] 今日计划
- [ ] 完成 API 接口
- [ ] 编写单元测试
- [ ] 更新文档

[2025-01-30 11:30] 代码审查反馈
- 优化数据库查询
- 添加输入验证
- 改进错误消息

[2025-01-30 14:00] 学习笔记
学习了 Zellij 插件开发
- WebAssembly 基础
- Zellij API
- Rust 基础
```

### 自定义模板

在笔记文件开头添加模板：

```markdown
# My Development Notes

## Quick Capture

[TIMESTAMP] 

## TODO

- [ ] 

## Ideas


## Snippets

```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `editor` | string | "$EDITOR" 或 "vim" | 编辑器命令 |
| `notes_file` | string | "~/.zellij-notes.md" | 笔记文件路径 |
| `timestamp_format` | string | "[%Y-%m-%d %H:%M] " | 时间戳格式 |
| `auto_timestamp` | boolean | true | 自动添加时间戳 |
| `x` | string | "25%" | 浮动面板 X 位置 |
| `y` | string | "30%" | 浮动面板 Y 位置 |
| `width` | string | "50%" | 浮动面板宽度 |
| `height` | string | "40%" | 浮动面板高度 |

### 时间戳格式参考

| 格式 | 示例 | 说明 |
|------|------|------|
| `%Y-%m-%d` | 2025-01-30 | 年月日 |
| `%H:%M` | 14:30 | 时分 |
| `%H:%M:%S` | 14:30:45 | 时分秒 |
| `%a` | Thu | 星期几简写 |
| `%b` | Jan | 月份简写 |

---

## 与其他笔记工具对比

| 工具 | 优点 | 缺点 |
|------|------|------|
| **zellij-notepad** | 集成在 Zellij 中 | 功能较简单 |
| Obsidian | 功能丰富 | 需要切换应用 |
| Notion | 协作功能 | 需要切换应用 |
| Vim wiki | 纯文本 | 需要手动管理 |

---

## 故障排除

### 编辑器不启动
- 确认 `editor` 配置正确
- 检查编辑器是否在 PATH 中
- 尝试使用绝对路径

### 笔记不保存
- 确认 `notes_file` 路径有写入权限
- 检查目录是否存在
- 在编辑器中手动保存测试

### 浮动面板位置不对
- 调整 `x`、`y`、`width`、`height` 配置
- 使用百分比或像素值
- 考虑不同屏幕尺寸

---

## 相关资源
- [官方仓库](https://github.com/0xble/zellij-notepad)
- [Vim](https://www.vim.org/)
- [Nano](https://www.nano-editor.org/)

---

*最后更新：2025年2月*
