# jbz (Just Bacon Zellij) 插件

> 在 bacon 中包装显示你的 just 命令

**GitHub**: [nim65s/jbz](https://github.com/nim65s/jbz)

---

## 简介

`jbz` (Just Bacon Zellij) 是一个将 [just](https://github.com/casey/just)（一个命令运行器）与 [bacon](https://dystroy.org/bacon/)（一个后台 Rust 代码检查工具）结合在一起的 Zellij 插件。它让你能够在一个浮动的 Zellij 面板中方便地查看和管理 just 命令。

---

## 功能特点

### 1. Just 命令集成
- 自动检测项目中的 `justfile`
- 列出所有可用的 just 命令
- 一键运行选中的命令

### 2. Bacon 实时反馈
- 集成 bacon 的实时输出显示
- 代码编译和检查结果的即时反馈
- 错误和警告的彩色高亮显示

### 3. 浮动面板显示
- 以浮动面板形式显示
- 不干扰主编辑工作流
- 可随时关闭或保持打开

### 4. 命令历史
- 保存最近运行的命令
- 快速重新运行历史命令
- 查看命令输出历史

### 5. 项目感知
- 自动识别当前项目的 justfile
- 支持工作区级别的命令
- 多项目环境支持

---

## 使用场景

### 场景 1: Rust 开发工作流
在 Rust 项目中使用 just 管理构建任务：
```justfile
# justfile
build:
    cargo build --release

test:
    cargo test

lint:
    cargo clippy

dev:
    bacon
```

### 场景 2: 多语言项目管理
一个仓库包含多个子项目：
```justfile
# justfile
frontend-dev:
    cd frontend && npm run dev

backend-dev:
    cd backend && cargo run

db-up:
    docker-compose up db
```

### 场景 3: 自动化工作流
常用开发任务的自动化：
```justfile
# justfile
setup:
    cp .env.example .env
    npm install
    cargo build

serve: build
    cargo run --bin server

watch:
    bacon
```

### 场景 4: CI/CD 本地模拟
在本地模拟 CI 流程：
```justfile
# justfile
ci:
    just lint
    just test
    just build
    just check
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- 已安装 [just](https://github.com/casey/just)
- 已安装 [bacon](https://dystroy.org/bacon/)（用于 Rust 项目）
- Rust 工具链

### 安装 just

```bash
# macOS
brew install just

# Ubuntu/Debian
apt-get install just

# Cargo
cargo install just
```

### 安装 bacon

```bash
# Cargo
cargo install bacon
```

### 安装 jbz 插件

1. 克隆仓库：
```bash
git clone https://github.com/nim65s/jbz.git
cd jbz
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/jbz.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    jbz location="file:~/.config/zellij/plugins/jbz.wasm"
}

keybinds {
    normal {
        bind "Ctrl j" {
            LaunchOrFocusPlugin "jbz" {
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
    jbz location="file:~/.config/zellij/plugins/jbz.wasm" {
        // justfile 文件名
        justfile_names ["justfile", "Justfile", ".justfile"]
        
        // bacon 配置
        bacon_config "~/.config/bacon/prefs.toml"
        
        // 默认命令
        default_command "dev"
        
        // 历史记录数量
        history_size 20
        
        // 自动运行默认命令
        autostart false
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+j`（或你的配置键）打开 jbz 面板。

### 界面说明

```
┌─────────────────────────────────────┐
│ jbz - Just Command Manager          │
├─────────────────────────────────────┤
│ Available Commands:                 │
│   1. build    - Build the project   │
│   2. test     - Run tests           │
│   3. lint     - Run clippy          │
│   4. dev      - Start bacon         │
│   5. serve    - Run server          │
├─────────────────────────────────────┤
│ Recent: build, test, lint           │
├─────────────────────────────────────┤
│ [Enter] Run  [h] History  [q] Quit  │
└─────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Enter` | 运行选中的命令 |
| `↑/↓` | 选择命令 |
| `h` | 查看历史 |
| `r` | 重新运行上次命令 |
| `c` | 取消当前运行 |
| `q/Esc` | 关闭面板 |

---

## justfile 示例

### 基础 Rust 项目

```justfile
# 默认命令（运行 jbz 时默认执行）
default:
    just --list

# 构建项目
build:
    cargo build --release

# 运行测试
test:
    cargo test

# 代码检查
lint:
    cargo clippy -- -D warnings
    cargo fmt --check

# 开发模式（使用 bacon）
dev:
    bacon

# 清理构建产物
clean:
    cargo clean

# 完整检查
check: lint test
    @echo "All checks passed!"
```

### 多项目工作区

```justfile
# 前端项目
frontend-dev:
    cd frontend && npm run dev

frontend-build:
    cd frontend && npm run build

frontend-test:
    cd frontend && npm test

# 后端项目
backend-dev:
    cd backend && cargo run

backend-test:
    cd backend && cargo test

# 数据库
db-up:
    docker-compose up -d postgres

db-migrate:
    cd backend && cargo run --bin migrate

# 完整开发环境
dev: db-up
    just frontend-dev &
    just backend-dev
```

---

## 与 bacon 的集成

### bacon 配置

创建 `~/.config/bacon/prefs.toml`：

```toml
# 默认任务
default_job = "check"

[jobs.check]
command = ["cargo", "check", "--all-targets"]
need_stdout = false

[jobs.clippy]
command = ["cargo", "clippy", "--all-targets"]
need_stdout = false

[jobs.test]
command = ["cargo", "test"]
need_stdout = true
```

### 在 justfile 中使用

```justfile
# 运行 bacon 检查
watch:
    bacon

# 运行特定 bacon 任务
watch-clippy:
    bacon clippy

# 运行测试并监控
watch-test:
    bacon test
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `justfile_names` | list | ["justfile"] | 要查找的 justfile 名称 |
| `bacon_config` | string | 默认路径 | bacon 配置文件路径 |
| `default_command` | string | 无 | 打开时默认运行的命令 |
| `history_size` | integer | 20 | 保存的历史命令数 |
| `autostart` | boolean | false | 是否自动运行默认命令 |
| `show_output` | boolean | true | 是否显示命令输出 |
| `floating` | boolean | true | 是否以浮动窗口显示 |

---

## 与其他工具对比

| 工具组合 | 使用场景 | 优点 | 缺点 |
|---------|---------|------|------|
| **jbz** | Zellij + just + bacon | 一体化体验 | 仅 Zellij |
| just + terminal | 通用 | 简单直接 | 无集成 |
| make | 传统项目 | 广泛使用 | 语法复杂 |
| npm scripts | Node.js | 生态成熟 | 仅限 JS |
| cargo xtask | Rust 专用 | 纯 Rust | 学习曲线 |

---

## 故障排除

### 找不到 justfile
- 确认项目根目录存在 justfile
- 检查 `justfile_names` 配置
- 尝试手动指定路径

### bacon 不工作
- 确认 bacon 已安装
- 检查 bacon 配置文件
- 查看 bacon 日志

### 命令运行失败
- 在普通终端中测试 just 命令
- 检查环境变量
- 确认工作目录正确

---

## 相关资源

- [官方仓库](https://github.com/nim65s/jbz)
- [just 文档](https://just.systems/man/en/)
- [bacon 文档](https://dystroy.org/bacon/)
- [Rust 构建工具](https://www.arewewebyet.org/topics/build/)

---

*最后更新：2025年2月*
