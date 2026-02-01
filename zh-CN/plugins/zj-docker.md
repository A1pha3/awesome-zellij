# zj-docker 插件

> 显示 Docker 容器并执行基本操作

**GitHub**: [dj95/zj-docker](https://github.com/dj95/zj-docker)

---

## 简介

`zj-docker` 是一个 Docker 管理插件，让你在 Zellij 中直接查看和管理 Docker 容器。它提供了一个交互式界面来列出容器、查看日志、执行命令等，无需离开 Zellij 环境。

---

## 功能特点

### 1. 容器列表
- 显示所有 Docker 容器
- 显示容器状态、端口、镜像等信息
- 按状态筛选（运行中、已停止等）

### 2. 容器管理
- 启动/停止容器
- 查看容器日志
- 在容器中执行命令

### 3. 日志查看
- 实时查看容器日志
- 支持日志筛选
- 在新面板中显示

### 4. 快速操作
- 一键执行常用操作
- 快速进入容器 shell
- 快速查看容器详情

### 5. 资源监控
- 显示容器资源使用
- CPU 和内存占用
- 网络 I/O 统计

---

## 使用场景

### 场景 1: 本地开发
管理本地开发环境的容器：
```
查看 web、db、cache 容器状态
快速重启需要的容器
查看应用日志
```

### 场景 2: 调试排查
排查容器化应用问题：
```
查看错误日志
进入容器检查配置
监控资源使用
```

### 场景 3: 容器编排
管理 docker-compose 项目：
```
查看所有服务状态
批量操作容器
快速切换服务
```

### 场景 4: 远程服务器
管理远程服务器的容器：
```
SSH 到远程服务器
在 Zellij 中使用 zj-docker
管理远程容器
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Docker 已安装
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/dj95/zj-docker.git
cd zj-docker
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zj-docker.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zj-docker location="file:~/.config/zellij/plugins/zj-docker.wasm"
}

keybinds {
    normal {
        bind "Ctrl d" {
            LaunchOrFocusPlugin "zj-docker" {
                floating true
                height "70%"
                width "80%"
            }
        }
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+d`（或你的配置键）打开 zj-docker 面板。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zj-docker - Docker Container Manager                 │
├─────────────────────────────────────────────────────┤
│                                                      │
│ NAME          STATUS    PORTS          IMAGE        │
│ web-app       Up 2h     3000->80       node:18     │
│ database      Up 2h     5432->5432     postgres:15 │
│ redis         Up 2h     6379->6379     redis:7     │
│ nginx         Up 2h     80->80         nginx:latest│
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Enter] Shell  [l] Logs  [r] Restart  [s] Stop      │
│ [d] Delete  [Esc] Quit                               │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+d` | 打开 zj-docker |
| `↑/↓` | 导航容器列表 |
| `Enter` | 进入容器 shell |
| `l` | 查看日志 |
| `r` | 重启容器 |
| `s` | 停止容器 |
| `d` | 删除容器 |
| `Esc` | 关闭面板 |

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `docker_command` | string | "docker" | Docker 命令路径 |
| `show_stopped` | boolean | true | 显示已停止容器 |
| `auto_refresh` | boolean | true | 自动刷新 |
| `refresh_interval` | integer | 5 | 刷新间隔（秒） |

---

## 故障排除

### 容器不显示
- 确认 Docker 守护进程运行中
- 检查用户是否在 docker 组
- 尝试 `docker ps` 测试

### 操作失败
- 检查容器权限
- 确认容器存在
- 查看 Docker 日志

---

## 相关资源
- [官方仓库](https://github.com/dj95/zj-docker)
- [Docker 文档](https://docs.docker.com/)

---

*最后更新：2025年2月*
