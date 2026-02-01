# zellij-switch 插件

> 使用 `zellij pipe` 在 CLI 中切换会话

**GitHub**: [mostafaqanbaryan/zellij-switch](https://github.com/mostafaqanbaryan/zellij-switch)

---

## 简介

`zellij-switch` 是一个命令行会话切换插件，它允许你通过 `zellij pipe` 命令在 Shell 脚本或命令行中切换 Zellij 会话。这对于自动化工作流和从外部工具控制 Zellij 非常有用。

---

## 功能特点

### 1. CLI 会话切换
- 通过命令行切换会话
- 支持脚本调用
- 适合自动化场景

### 2. 模糊搜索
- 支持模糊匹配会话名
- 自动完成部分名称
- 处理重复会话名

### 3. 会话列表
- 获取所有会话列表
- 获取当前会话
- 获取会话详细信息

### 4. 错误处理
- 清晰的错误信息
- 处理边界情况
- 返回状态码

### 5. 轻量级
- 无界面组件
- 快速响应
- 最小资源占用

---

## 使用场景

### 场景 1: Shell 脚本自动化
在脚本中切换会话：
```bash
#!/bin/bash
# 部署脚本

zellij pipe --plugin zellij-switch -- "--session production"
# 执行部署命令
./deploy.sh
```

### 场景 2: 项目切换快捷键
创建快速切换项目的脚本：
```bash
# ~/.local/bin/zj-switch
SESSION=$1
zellij pipe --plugin zellij-switch -- "--session $SESSION"
```

使用：
```bash
zj-switch project-a
```

### 场景 3: IDE 集成
在编辑器中集成会话切换：
```vim
" Vim 中切换到项目会话
command! ZjSwitch call system('zellij pipe --plugin zellij-switch -- "--session " . input("Session: ")')
```

### 场景 4: fzf 集成
与 fzf 结合使用：
```bash
# 使用 fzf 选择会话
SESSION=$(zellij pipe --plugin zellij-switch -- "--list" | fzf)
zellij pipe --plugin zellij-switch -- "--session $SESSION"
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/mostafaqanbaryan/zellij-switch.git
cd zellij-switch
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-switch.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-switch location="file:~/.config/zellij/plugins/zellij-switch.wasm"
}
```

此插件主要通过命令行调用，通常不需要额外的键绑定配置。

---

## 使用方法

### 命令行参数

```bash
# 列出所有会话
zellij pipe --plugin zellij-switch -- "--list"

# 切换到特定会话
zellij pipe --plugin zellij-switch -- "--session session-name"

# 获取当前会话
zellij pipe --plugin zellij-switch -- "--current"

# 模糊搜索切换
zellij pipe --plugin zellij-switch -- "--fuzzy project"
```

### Shell 脚本示例

**项目切换脚本：**
```bash
#!/bin/bash
# zj-project - 快速切换到项目会话

PROJECTS_DIR="$HOME/projects"

# 选择项目
PROJECT=$(ls -1 "$PROJECTS_DIR" | fzf --prompt="Select project: ")

if [ -n "$PROJECT" ]; then
    # 创建或切换到项目会话
    zellij pipe --plugin zellij-switch -- "--session $PROJECT"
    
    # 如果会话是新创建的，导航到项目目录
    if [ $? -eq 0 ]; then
        cd "$PROJECTS_DIR/$PROJECT"
    fi
fi
```

**工作流切换：**
```bash
#!/bin/bash
# zj-workflow - 切换工作流

case "$1" in
    "dev")
        zellij pipe --plugin zellij-switch -- "--session development"
        ;;
    "test")
        zellij pipe --plugin zellij-switch -- "--session testing"
        ;;
    "prod")
        zellij pipe --plugin zellij-switch -- "--session production"
        ;;
    *)
        echo "Usage: $0 {dev|test|prod}"
        exit 1
        ;;
esac
```

### fzf 集成

**高级会话切换：**
```bash
#!/bin/bash
# zj-fzf-switch - 使用 fzf 选择会话

# 获取会话列表
SESSIONS=$(zellij pipe --plugin zellij-switch -- "--list" 2>/dev/null)

if [ -z "$SESSIONS" ]; then
    echo "No Zellij sessions found"
    exit 1
fi

# 使用 fzf 选择
SELECTED=$(echo "$SESSIONS" | fzf --prompt="Zellij session: " --height=10)

if [ -n "$SELECTED" ]; then
    zellij pipe --plugin zellij-switch -- "--session $SELECTED"
fi
```

---

## 返回值

| 返回值 | 含义 |
|--------|------|
| `0` | 成功 |
| `1` | 会话不存在 |
| `2` | 参数错误 |
| `3` | Zellij 未运行 |
| `4` | 其他错误 |

---

## 与其他会话管理工具对比

| 工具 | 使用方式 | 优势 |
|------|---------|------|
| **zellij-switch** | CLI/pipe | 脚本友好 |
| zellij-choose-tree | TUI | 可视化 |
| zsm | CLI | zoxide 集成 |
| zellij-favs | TUI | 收藏管理 |

---

## 故障排除

### 命令无响应
- 确认插件已加载
- 检查 Zellij 版本兼容性
- 确认在 Zellij 会话中运行

### 会话切换失败
- 确认目标会话存在
- 检查会话名称拼写
- 查看返回值

### 权限问题
- 确认有访问 Zellij 的权限
- 检查插件文件权限
- 尝试重新加载 Zellij

---

## 相关资源
- [官方仓库](https://github.com/mostafaqanbaryan/zellij-switch)
- [博客教程](https://mostafaqanbaryan.com/zellij-attach-plugin/)
- [fzf](https://github.com/junegunn/fzf)
- [zsm](./zsm.md) - 另一个 CLI 会话切换工具

---

*最后更新：2025年2月*
