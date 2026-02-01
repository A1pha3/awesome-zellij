# multitask 插件

> 作为 Zellij 插件的迷你 CI

**GitHub**: [imsnif/multitask](https://github.com/imsnif/multitask)

---

## 简介

`multitask` 是一个 Zellij 插件，它将你的终端转变为一个迷你持续集成（CI）系统。它允许你定义任务序列，自动运行测试、构建和其他开发任务，并在 Zellij 的多个面板中显示结果。

---

## 功能特点

### 1. 任务定义与管理
- 使用配置文件定义任务
- 支持任务依赖关系
- 任务可以并行或串行执行

### 2. 自动 CI 工作流
- 文件变化时自动触发任务
- 支持多种触发条件（保存、git 提交等）
- 任务失败时自动停止或继续

### 3. 可视化状态面板
- 在专用面板中显示所有任务状态
- 彩色标识（绿色=成功，红色=失败，黄色=运行中）
- 实时更新任务进度

### 4. 多面板输出
- 每个任务在独立面板中运行
- 并行任务同时显示输出
- 支持长时间运行的任务

### 5. 失败处理
- 任务失败时的自定义行为
- 失败通知
- 重试机制

---

## 使用场景

### 场景 1: 本地 CI 模拟
在提交代码前本地运行完整的 CI 流程：
```
lint → test → build → security-check
```

### 场景 2: 实时开发反馈
保存文件时自动运行相关测试：
- 修改 `src/user.rs` → 自动运行用户模块测试
- 修改前端代码 → 自动运行前端测试和构建

### 场景 3: 多项目监控
同时监控多个相关项目的构建状态：
- 前端项目构建
- 后端项目测试
- 共享库编译

### 场景 4: 文档与网站生成
自动生成和部署文档：
```
保存 .md 文件 → 生成 HTML → 部署到预览环境
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/imsnif/multitask.git
cd multitask
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/multitask.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 创建配置文件

在项目根目录创建 `.multitask.toml`：

```toml
# .multitask.toml

# 全局设置
[settings]
# 是否并行运行任务
parallel = false
# 失败时是否停止
fail_fast = true
# 轮询间隔（毫秒）
poll_interval = 1000

# 任务定义
[[tasks]]
name = "lint"
command = "cargo clippy -- -D warnings"
# 此任务的文件监视模式
watch = ["src/**/*.rs"]

[[tasks]]
name = "test"
command = "cargo test"
# 依赖其他任务
depends_on = ["lint"]
watch = ["src/**/*.rs", "tests/**/*.rs"]

[[tasks]]
name = "build"
command = "cargo build --release"
depends_on = ["test"]
watch = ["src/**/*.rs", "Cargo.toml"]

[[tasks]]
name = "docs"
command = "cargo doc --no-deps"
watch = ["src/**/*.rs"]
```

### Zellij 配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    multitask location="file:~/.config/zellij/plugins/multitask.wasm"
}

keybinds {
    normal {
        bind "Ctrl m" {
            LaunchOrFocusPlugin "multitask" {
                floating true
                height "60%"
                width "70%"
            }
        }
    }
}
```

---

## 使用方法

### 启动插件

1. 在项目根目录（包含 `.multitask.toml`）启动 Zellij
2. 按 `Ctrl+m` 打开 multitask 面板
3. 查看任务列表和状态

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ multitask - Mini CI                                  │
├─────────────────────────────────────────────────────┤
│ Tasks:                                              │
│   🟡 lint     - Running...                          │
│   ⏸️  test     - Waiting (depends on: lint)         │
│   ⏸️  build    - Pending                            │
│   ⏸️  docs     - Pending                            │
├─────────────────────────────────────────────────────┤
│ Output:                                             │
│   lint: cargo clippy -- -D warnings                 │
│   Checking project...                               │
└─────────────────────────────────────────────────────┘
[r] Run All  [w] Watch Mode  [s] Stop All  [q] Quit
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `r` | 运行所有任务 |
| `w` | 切换监视模式 |
| `s` | 停止所有运行中的任务 |
| `Enter` | 查看选中任务的输出 |
| `q/Esc` | 关闭面板 |

---

## 配置文件详解

### 任务属性

| 属性 | 类型 | 必需 | 描述 |
|------|------|------|------|
| `name` | string | 是 | 任务名称 |
| `command` | string | 是 | 要执行的命令 |
| `depends_on` | list | 否 | 依赖的其他任务 |
| `watch` | list | 否 | 监视的文件模式 |
| `env` | map | 否 | 环境变量 |
| `cwd` | string | 否 | 工作目录 |
| `timeout` | integer | 否 | 超时时间（秒） |
| `retries` | integer | 否 | 失败重试次数 |

### 完整配置示例

```toml
# .multitask.toml

[settings]
parallel = false
fail_fast = true
poll_interval = 1000
show_output = true

[[tasks]]
name = "format"
command = "cargo fmt --check"
watch = ["src/**/*.rs"]

[[tasks]]
name = "clippy"
command = "cargo clippy --all-targets -- -D warnings"
depends_on = ["format"]
watch = ["src/**/*.rs", "Cargo.toml"]

[[tasks]]
name = "test-unit"
command = "cargo test --lib"
depends_on = ["clippy"]
watch = ["src/**/*.rs", "tests/**/*.rs"]

[[tasks]]
name = "test-integration"
command = "cargo test --test '*'"
depends_on = ["test-unit"]
watch = ["tests/**/*.rs", "src/**/*.rs"]

[[tasks]]
name = "build"
command = "cargo build --release"
depends_on = ["test-integration"]
watch = ["src/**/*.rs", "Cargo.toml"]

[[tasks]]
name = "security-audit"
command = "cargo audit"
watch = ["Cargo.lock"]
retries = 0  # 不重试安全审计

# 前端项目任务
[[tasks]]
name = "frontend-lint"
command = "cd frontend && npm run lint"
watch = ["frontend/src/**/*"]
cwd = "frontend"

[[tasks]]
name = "frontend-test"
command = "npm run test"
depends_on = ["frontend-lint"]
cwd = "frontend"
```

---

## 工作流模式

### 模式 1: 串行 CI 流程

```toml
[settings]
parallel = false
fail_fast = true

[[tasks]]
name = "step1"
command = "echo 'Step 1'"

[[tasks]]
name = "step2"
command = "echo 'Step 2'"
depends_on = ["step1"]

[[tasks]]
name = "step3"
command = "echo 'Step 3'"
depends_on = ["step2"]
```

### 模式 2: 并行构建

```toml
[settings]
parallel = true
fail_fast = false

[[tasks]]
name = "frontend-build"
command = "cd frontend && npm run build"

[[tasks]]
name = "backend-build"
command = "cd backend && cargo build"

[[tasks]]
name = "deploy"
command = "./deploy.sh"
depends_on = ["frontend-build", "backend-build"]
```

### 模式 3: 监视模式开发

```toml
[settings]
poll_interval = 500  # 快速轮询

[[tasks]]
name = "dev-server"
command = "cargo run"
watch = ["src/**/*.rs"]
# 此任务在后台持续运行
persistent = true

[[tasks]]
name = "tests"
command = "cargo test"
watch = ["src/**/*.rs", "tests/**/*.rs"]
```

---

## 与真正 CI 系统的对比

| 特性 | multitask | GitHub Actions | GitLab CI |
|------|-----------|----------------|-----------|
| 本地运行 | ✅ | ❌ | ❌ |
| 文件监视 | ✅ | ⚠️ | ⚠️ |
| 并行执行 | ✅ | ✅ | ✅ |
| 缓存 | ❌ | ✅ | ✅ |
| 并行矩阵 | ❌ | ✅ | ✅ |
| 环境隔离 | ❌ | ✅ | ✅ |

**multitask 的定位**：本地开发辅助工具，不是 CI 替代品。

---

## 故障排除

### 任务不运行
- 检查 `.multitask.toml` 语法
- 确认命令在 shell 中可以运行
- 查看任务依赖是否正确

### 监视模式不触发
- 检查 `watch` 模式语法（使用 glob 模式）
- 确认 `poll_interval` 设置合理
- 查看 Zellij 日志

### 任务输出不显示
- 确认 `show_output` 设置为 true
- 检查面板布局是否正确
- 尝试手动运行任务查看输出

---

## 相关资源

- [官方仓库](https://github.com/imsnif/multitask)
- [GitHub Actions](https://github.com/features/actions)
- [GitLab CI](https://docs.gitlab.com/ee/ci/)
- [cargo-watch](https://github.com/watchexec/cargo-watch)

---

*最后更新：2025年2月*
