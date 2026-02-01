# zellij-playbooks 插件

> 直接在终端中浏览、选择和执行剧本文件中的命令

**GitHub**: [yaroslavborbat/zellij-playbooks](https://github.com/yaroslavborbat/zellij-playbooks)

---

## 简介

`zellij-playbooks` 是一个强大的命令管理插件，它允许你定义和组织常用的命令到"剧本"（playbook）文件中。通过这个插件，你可以浏览、搜索和执行这些预定义的命令，非常适合管理复杂的开发工作流和运维任务。

---

## 功能特点

### 1. 剧本文件管理
- 使用 YAML/TOML 定义命令剧本
- 按项目或类别组织命令
- 支持变量和模板

### 2. 交互式浏览
- 可视化浏览所有可用命令
- 按类别筛选
- 模糊搜索命令

### 3. 命令执行
- 一键执行选中的命令
- 在当前面板或新面板中运行
- 显示执行输出

### 4. 变量支持
- 命令中支持变量占位符
- 执行时提示输入变量值
- 环境变量集成

### 5. 命令历史
- 记录执行历史
- 快速重新执行
- 查看执行结果

---

## 使用场景

### 场景 1: 项目命令集合
为每个项目定义专用的命令集：
```yaml
# playbook.yaml
commands:
  - name: Build
    cmd: cargo build --release
    
  - name: Test
    cmd: cargo test
    
  - name: Deploy to Staging
    cmd: ./scripts/deploy.sh staging
```

### 场景 2: 运维任务
管理服务器运维命令：
```yaml
commands:
  - name: View Logs
    cmd: ssh {{server}} "tail -f /var/log/app.log"
    
  - name: Restart Service
    cmd: ssh {{server}} "sudo systemctl restart {{service}}"
    
  - name: Check Disk
    cmd: ssh {{server}} "df -h"
```

### 场景 3: 开发工作流
加速日常开发任务：
```yaml
commands:
  - name: Quick Commit
    cmd: git add . && git commit -m "{{message}}" && git push
    
  - name: Run Linter
    cmd: cargo clippy && cargo fmt
    
  - name: Database Migrate
    cmd: diesel migration run
```

### 场景 4: 环境管理
管理不同环境的配置：
```yaml
commands:
  - name: Start Dev Environment
    cmd: docker-compose -f docker-compose.dev.yml up
    
  - name: Start Test Environment
    cmd: docker-compose -f docker-compose.test.yml up
    
  - name: Clean Environment
    cmd: docker-compose down -v
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/yaroslavborbat/zellij-playbooks.git
cd zellij-playbooks
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-playbooks.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-playbooks location="file:~/.config/zellij/plugins/zellij-playbooks.wasm"
}

keybinds {
    normal {
        bind "Ctrl y" {
            LaunchOrFocusPlugin "zellij-playbooks" {
                floating true
                height "60%"
                width "70%"
            }
        }
    }
}
```

### 创建剧本文件

创建 `~/.config/zellij/playbooks.yaml`：

```yaml
# playbooks.yaml
playbooks:
  - name: Development
    description: Development commands
    commands:
      - name: Build
        cmd: cargo build --release
        description: Build the project in release mode
        
      - name: Test
        cmd: cargo test --all
        description: Run all tests
        
      - name: Lint
        cmd: cargo clippy -- -D warnings
        description: Run linter
        
  - name: Deployment
    description: Deployment commands
    commands:
      - name: Deploy to Staging
        cmd: ./deploy.sh staging {{branch}}
        description: Deploy to staging server
        variables:
          - name: branch
            default: main
            
      - name: Deploy to Production
        cmd: ./deploy.sh production {{branch}}
        description: Deploy to production
        confirmation: true
        
  - name: Docker
    description: Docker commands
    commands:
      - name: Start Services
        cmd: docker-compose up -d
        description: Start all services
        
      - name: View Logs
        cmd: docker-compose logs -f {{service}}
        description: View service logs
        variables:
          - name: service
            default: app
```

---

## 使用方法

### 启动插件

按 `Ctrl+y`（或你的配置键）打开 playbooks 面板。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zellij-playbooks                                     │
├─────────────────────────────────────────────────────┤
│ > build                                              │
│                                                      │
│ 📁 Development                                       │
│   🔨 Build                Build the project...      │
│   🧪 Test                 Run all tests             │
│   📐 Lint                 Run linter                │
│                                                      │
│ 📁 Deployment                                        │
│   🚀 Deploy to Staging    Deploy to staging...      │
│   🚀 Deploy to Production Deploy to production...   │
│                                                      │
│ 📁 Docker                                            │
│   🐳 Start Services       Start all services        │
│   📋 View Logs            View service logs         │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Enter] Execute  [v] View Vars  [Esc] Quit          │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+y` | 打开 playbooks |
| `↑/↓` | 导航命令 |
| `Enter` | 执行选中的命令 |
| `Tab` | 切换分组 |
| `v` | 查看/编辑变量 |
| `/` | 搜索命令 |
| `Esc` | 关闭面板 |

### 工作流程

**执行命令：**
```
1. 按 Ctrl+y 打开 playbooks
2. 使用 ↑/↓ 导航或使用 / 搜索
3. 选中命令后按 Enter
4. 如果命令有变量，输入变量值
5. 命令在当前或新面板中执行
```

**使用变量：**
```
选择 "Deploy to Staging"
┌─────────────────────────────────────────────┐
│ Deploy to Staging                           │
├─────────────────────────────────────────────┤
│ Command: ./deploy.sh staging {{branch}}     │
│                                             │
│ Variables:                                  │
│   branch: [main____________________]        │
│                                             │
├─────────────────────────────────────────────┤
│ [Enter] Execute  [Esc] Cancel               │
└─────────────────────────────────────────────┘
```

---

## 剧本文件格式

### YAML 格式

```yaml
playbooks:
  - name: Playbook Name
    description: Description of this playbook
    commands:
      - name: Command Name
        cmd: command to execute {{variable}}
        description: What this command does
        working_dir: /optional/working/directory
        env:
          VAR1: value1
          VAR2: value2
        variables:
          - name: variable
            description: Description of variable
            default: default_value
            required: false
        confirmation: false  # Ask for confirmation
        new_pane: false  # Run in new pane
```

### TOML 格式

```toml
[[playbooks]]
name = "Development"
description = "Development commands"

[[playbooks.commands]]
name = "Build"
cmd = "cargo build --release"
description = "Build the project"
new_pane = true
```

---

## 与 zellij-bookmarks 对比

| 特性 | zellij-playbooks | zellij-bookmarks |
|------|------------------|------------------|
| 命令组织 | 按剧本分组 | 按标签分组 |
| 变量支持 | ✅ | ⚠️ |
| 描述信息 | ✅ 详细 | ✅ 简洁 |
| 环境变量 | ✅ | ❌ |
| 确认提示 | ✅ | ❌ |
| 目标场景 | 复杂工作流 | 快速命令 |

**选择建议**：
- 复杂命令需要变量 → zellij-playbooks
- 简单快速命令 → zellij-bookmarks

---

## 故障排除

### 剧本不加载
- 检查剧本文件路径
- 验证 YAML/TOML 语法
- 确认文件编码为 UTF-8

### 命令执行失败
- 检查命令语法
- 确认工作目录正确
- 检查环境变量

### 变量不工作
- 使用 `{{variable}}` 语法
- 确认变量在 `variables` 中定义
- 检查变量名称拼写

---

## 相关资源
- [官方仓库](https://github.com/yaroslavborbat/zellij-playbooks)
- [zellij-bookmarks](./zellij-bookmarks.md) - 简单命令书签
- [jbz](./jbz.md) - Just 命令包装器

---

*最后更新：2025年2月*
