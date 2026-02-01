# gitpod.zellij 插件

> Zellij 的 Gitpod 集成插件，支持 .gitpod.yml 任务集成

**GitHub**: [gitpod-samples/gitpod.zellij](https://github.com/gitpod-samples/gitpod.zellij)

---

## 简介

`gitpod.zellij` 是专门为 Gitpod 云开发环境设计的 Zellij 插件。它能够无缝集成 Gitpod 的 `.gitpod.yml` 任务配置，让你在云端开发时也能享受 Zellij 强大的终端复用功能。

---

## 功能特点

### 1. Gitpod 任务自动集成
- 自动读取 `.gitpod.yml` 中定义的任务
- 在 Zellij 标签页中自动创建并运行任务
- 支持 init、command、before 等任务类型

### 2. 云开发环境优化
- 专为 Gitpod 工作区设计
- 支持 Gitpod 环境变量
- 与 Gitpod 的预构建功能集成

### 3. 任务状态可视化
- 显示任务运行状态
- 区分已完成、运行中和失败的任务
- 支持任务输出查看

### 4. 多任务并行执行
- 同时运行多个独立任务
- 每个任务在单独的面板中运行
- 避免任务间的相互干扰

### 5. 热重载支持
- 修改 `.gitpod.yml` 后自动检测变化
- 支持重新加载任务配置
- 无需重启工作区

---

## 使用场景

### 场景 1: 云 IDE 工作流
在 Gitpod 中使用 Zellij 管理复杂的开发环境：
```yaml
# .gitpod.yml
tasks:
  - name: Install dependencies
    init: npm install
    command: npm run dev
  - name: Database
    command: docker-compose up db
  - name: Documentation
    command: npm run docs:dev
```

### 场景 2: 微服务开发
开发微服务架构时，同时运行多个服务：
- API 服务器
- 前端开发服务器
- 数据库服务
- 消息队列

### 场景 3: 教学演示
在在线教学环境中展示多终端工作流：
- 同时显示代码编辑器、终端、预览
- 预设学生练习任务
- 标准化的学习环境

### 场景 4: CI/CD 预览
在预构建环境中查看 CI 流程：
- 自动运行测试
- 显示构建输出
- 预览部署状态

---

## 安装方法

### 在 Gitpod 工作区中安装

1. 在 `.gitpod.yml` 中添加 Zellij 安装：
```yaml
# .gitpod.yml
tasks:
  - name: Install Zellij
    before: |
      brew install zellij
      # 或
      cargo install zellij
```

2. 添加插件配置：
```yaml
vscode:
  extensions:
    - zellij.zellij
```

### 手动安装插件

1. 克隆仓库：
```bash
git clone https://github.com/gitpod-samples/gitpod.zellij.git
cd gitpod.zellij
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 配置 Zellij 加载插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/gitpod_zellij.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 完整的 .gitpod.yml 示例

```yaml
# .gitpod.yml
image: gitpod/workspace-full

tasks:
  - name: Setup Environment
    init: |
      echo "Setting up..."
      npm install
      cp .env.example .env
    command: |
      echo "Environment ready!"

  - name: Development Server
    command: npm run dev
    openMode: split-right

  - name: Tests
    command: npm run test:watch
    openMode: split-bottom

  - name: Documentation
    init: npm install -g docsify-cli
    command: docsify serve docs

ports:
  - port: 3000
    onOpen: open-preview
  - port: 8080
    onOpen: ignore

vscode:
  extensions:
    - rust-lang.rust-analyzer
    - vadimcn.vscode-lldb
```

### Zellij 配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
// 插件定义
plugins {
    gitpod location="file:~/.config/zellij/plugins/gitpod_zellij.wasm"
}

// 启动时自动加载 Gitpod 任务
layout {
    pane split_direction="horizontal" {
        pane
        pane size=1 {
            plugin location="gitpod" {
                autostart true
                show_completed true
            }
        }
    }
}

// 快捷键绑定
keybinds {
    normal {
        bind "Ctrl g" {
            LaunchOrFocusPlugin "gitpod" {
                floating true
            }
        }
    }
}
```

---

## 使用方法

### 自动模式

当进入 Gitpod 工作区时，插件会自动：
1. 读取 `.gitpod.yml` 文件
2. 解析所有任务定义
3. 在 Zellij 中创建对应的标签页和面板
4. 按正确顺序启动任务

### 手动模式

通过快捷键手动控制：

```
Ctrl+g    - 打开/关闭 Gitpod 任务面板
r         - 重新加载 .gitpod.yml
c         - 清理已完成任务
s         - 显示任务状态
```

### 任务管理

在插件界面中：
- 查看所有任务列表
- 点击任务名称切换到对应面板
- 重新运行失败的任务
- 终止长时间运行的任务

---

## 任务生命周期

```
.gitpod.yml 定义
       ↓
[before] → [init] → [command]
       ↓
  面板创建 → 命令执行 → 状态监控
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `autostart` | boolean | true | 工作区启动时自动运行任务 |
| `show_completed` | boolean | true | 显示已完成的任务 |
| `task_timeout` | integer | 0 | 任务超时时间（秒，0=无限制） |
| `fail_fast` | boolean | false | 第一个任务失败时停止所有任务 |
| `parallel_limit` | integer | 10 | 并行运行的最大任务数 |

---

## 与其他工具对比

| 特性 | gitpod.zellij | Gitpod 原生终端 | Zellij 单独使用 |
|------|--------------|-----------------|-----------------|
| 任务集成 | ✅ 自动 | ⚠️ 手动 | ❌ |
| 可视化 | ✅ | ⚠️ | ✅ |
| 多任务管理 | ✅ | ⚠️ | ✅ |
| 配置复用 | ✅ .gitpod.yml | ✅ | ❌ |

---

## 故障排除

### 任务不自动启动
- 检查 `.gitpod.yml` 语法
- 确认 `autostart` 设置为 true
- 查看 Gitpod 环境变量

### 任务执行失败
- 检查命令在 Gitpod 环境中的可用性
- 确认依赖项已安装
- 查看任务输出日志

### 插件无法加载
- 确认 `.wasm` 文件路径正确
- 检查 Zellij 版本兼容性
- 尝试重新编译插件

---

## 最佳实践

1. **任务命名**：给任务起有意义的名字，便于识别
2. **任务分组**：使用 openMode 控制面板布局
3. **错误处理**：在 init 脚本中添加错误检查
4. **超时设置**：为长时间任务设置合理的超时

---

## 相关资源

- [Gitpod 文档](https://www.gitpod.io/docs)
- [Gitpod 任务配置](https://www.gitpod.io/docs/references/gitpod-yml)
- [官方仓库](https://github.com/gitpod-samples/gitpod.zellij)

---

*最后更新：2025年2月*
