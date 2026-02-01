# zellij-newtab-plus 插件

> 创建命名标签页并使用 zoxide 一键导航

**GitHub**: [AlexZasorin/zellij-newtab-plus](https://github.com/AlexZasorin/zellij-newtab-plus)

---

## 简介

`zellij-newtab-plus` 是一个增强的 Zellij 标签页管理插件，它结合了标签页创建和 [zoxide](https://github.com/ajeetdsouza/zoxide)（一个智能目录跳转工具）的功能。通过一个快捷键，你可以快速创建新标签页并自动导航到常用目录。

---

## 功能特点

### 1. 智能标签页创建
- 创建命名标签页
- 自动根据目录名建议标签名
- 支持自定义标签名

### 2. zoxide 集成
- 自动集成 zoxide 数据库
- 快速跳转到常用目录
- 模糊匹配目录名

### 3. 一键工作流
- 单个快捷键完成创建+导航
- 自动在新标签页中打开 shell
- 自动切换到新标签

### 4. 历史记录
- 记录最近使用的目录
- 快速重新访问
- 优先级排序

### 5. 目录预览
- 显示目录内容预览
- 显示最近访问时间
- 快速了解目录信息

---

## 使用场景

### 场景 1: 快速项目切换
在不同项目间快速切换：
```
快捷键 → 输入 "proj" → 匹配到 ~/projects/web-app
→ 创建新标签页 "web-app" 并自动导航到该目录
```

### 场景 2: 多任务并行
同时处理多个目录的任务：
```
标签 1: ~/work/project-a
标签 2: ~/personal/blog
标签 3: ~/work/project-b

使用 zellij-newtab-plus 快速创建每个项目的标签页
```

### 场景 3: 文档和配置管理
快速访问常用配置目录：
```
快捷键 → 输入 "dot" → 匹配到 ~/.config
快捷键 → 输入 "doc" → 匹配到 ~/Documents
```

### 场景 4: 临时目录工作
快速创建临时工作目录的标签页：
```
下载了一个压缩包到 /tmp/extract-12345
快捷键 → 输入 "extr" → 快速导航到该目录
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- [zoxide](https://github.com/ajeetdsouza/zoxide) 已安装并初始化
- Rust 工具链

### 安装 zoxide

```bash
# macOS
brew install zoxide

# Ubuntu/Debian
apt-get install zoxide

# Cargo
cargo install zoxide

# 初始化 zoxide（添加到 shell 配置）
echo 'eval "$(zoxide init bash)"' >> ~/.bashrc  # 或 ~/.zshrc
echo 'eval "$(zoxide init zsh)"' >> ~/.zshrc
```

### 从源码安装插件

1. 克隆仓库：
```bash
git clone https://github.com/AlexZasorin/zellij-newtab-plus.git
cd zellij-newtab-plus
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-newtab-plus.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-newtab-plus location="file:~/.config/zellij/plugins/zellij-newtab-plus.wasm"
}

keybinds {
    normal {
        bind "Ctrl n" {
            LaunchOrFocusPlugin "zellij-newtab-plus" {
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
    zellij-newtab-plus location="file:~/.config/zellij/plugins/zellij-newtab-plus.wasm" {
        // zoxide 配置
        zoxide_command "zoxide"
        use_zoxide true
        
        // 标签页命名
        auto_name true
        name_template "{dir_name}"
        
        // 显示选项
        show_recent 10
        show_preview true
        show_frecency true
        
        // 布局
        layout "default"  // default, horizontal, vertical
        
        // 快捷键
        key_confirm "Enter"
        key_cancel "Esc"
        key_custom_name "Ctrl+r"
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+n`（或你的配置键）打开 zellij-newtab-plus 面板。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zellij-newtab-plus                                   │
├─────────────────────────────────────────────────────┤
│ > web                                               │
│                                                      │
│ Recent Directories:                                  │
│   1. ~/projects/web-app         [2 days ago]        │
│   2. ~/work/website             [1 week ago]        │
│   3. ~/personal/blog            [3 weeks ago]       │
│                                                      │
│ Matching from zoxide:                                │
│   4. ~/projects/web-frontend    [score: 150]        │
│   5. ~/projects/web-backend     [score: 120]        │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Enter] Create Tab  [Ctrl+r] Custom Name  [Esc] Quit│
└─────────────────────────────────────────────────────┘
```

### 工作流程

**创建新标签页：**
```
1. 按 Ctrl+n 打开面板
2. 开始输入目录名或关键词
3. 从列表中选择匹配的目录
   - 使用 ↑/↓ 导航
   - 或继续输入精确匹配
4. 按 Enter 创建标签页
5. 新标签页自动命名并导航到该目录
```

**自定义标签名：**
```
1. 选择目录
2. 按 Ctrl+r 编辑标签名
3. 输入自定义名称
4. 按 Enter 确认
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+n` | 打开 zellij-newtab-plus |
| `↑/↓` | 导航目录列表 |
| `Enter` | 创建新标签页 |
| `Ctrl+r` | 重命名标签页 |
| `Tab` | 切换视图（最近/全部） |
| `Esc` | 关闭面板 |

---

## zoxide 集成

### zoxide 工作原理

zoxide 会记住你访问过的目录，并根据访问频率（frecency）排序：

```bash
# 第一次访问
cd ~/projects/web-app

# 之后可以直接跳转
z web
# 或
z web-app
```

### zellij-newtab-plus 如何使用 zoxide

1. 读取 zoxide 数据库
2. 根据输入模糊匹配目录
3. 按 frecency 分数排序
4. 显示匹配结果供选择

### 训练 zoxide

让 zoxide 更了解你的习惯：

```bash
# 多访问常用目录
cd ~/work/project-a
cd ~/personal/blog
cd ~/.config/zellij

# zoxide 会自动学习
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `zoxide_command` | string | "zoxide" | zoxide 命令路径 |
| `use_zoxide` | boolean | true | 启用 zoxide 集成 |
| `auto_name` | boolean | true | 自动命名标签页 |
| `name_template` | string | "{dir_name}" | 标签名模板 |
| `show_recent` | integer | 10 | 显示最近目录数 |
| `show_preview` | boolean | true | 显示目录预览 |
| `show_frecency` | boolean | true | 显示 frecency 分数 |
| `layout` | string | "default" | 新标签页布局 |

### 标签名模板变量

| 变量 | 说明 |
|------|------|
| `{dir_name}` | 目录名 |
| `{parent_dir}` | 父目录名 |
| `{full_path}` | 完整路径 |
| `{date}` | 当前日期 |

---

## 与 zoxide 单独使用对比

| 场景 | zellij-newtab-plus | zoxide 单独 |
|------|--------------------|-------------|
| 创建新标签页 | ✅ 自动创建 | ❌ 需要手动 |
| 命名标签页 | ✅ 自动命名 | ❌ 需要手动 |
| 导航到目录 | ✅ | ✅ |
| 切换上下文 | ✅ 更少步骤 | ⚠️ 多步操作 |

---

## 故障排除

### zoxide 命令未找到
- 确认 zoxide 已安装
- 检查 `zoxide_command` 路径
- 确保 zoxide 在 PATH 中

### 目录列表为空
- 使用 zoxide 访问一些目录
- 等待 zoxide 数据库建立
- 检查 zoxide 数据库文件

### 标签页创建失败
- 确认目录存在且有权限
- 检查 Zellij 版本兼容性
- 查看 Zellij 日志

---

## 相关资源
- [官方仓库](https://github.com/AlexZasorin/zellij-newtab-plus)
- [zoxide](https://github.com/ajeetdsouza/zoxide) - 智能目录跳转
- [zsm](./zsm.md) - 另一个 zoxide 集成插件

---

*最后更新：2025年2月*
