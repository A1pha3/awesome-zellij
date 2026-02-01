# zellij-bookmarks 插件

> 管理命令书签并快速插入终端

**GitHub**: [yaroslavborbat/zellij-bookmarks](https://github.com/yaroslavborbat/zellij-bookmarks)

---

## 简介

`zellij-bookmarks` 是一个实用的 Zellij 插件，允许你保存常用的命令作为书签，并在需要时快速插入到终端中。它类似于浏览器的书签功能，但专为终端命令设计。

---

## 功能特点

### 1. 命令书签管理
- 保存常用命令为书签
- 为书签添加描述和标签
- 分组管理书签

### 2. 快速插入
- 一键将命令插入当前终端
- 支持参数化命令
- 自动填充或手动编辑

### 3. 模糊搜索
- 实时搜索书签
- 支持按标签过滤
- 快速定位需要的命令

### 4. 导入导出
- 导出书签配置
- 导入共享的书签
- 支持版本控制

### 5. 多会话同步
- 书签在不同会话间共享
- 配置文件存储
- 易于备份和迁移

---

## 使用场景

### 场景 1: 复杂命令记忆
记住经常使用的复杂命令：
```bash
# 书签: Deploy to Staging
ssh -i ~/.ssh/deploy.pem user@staging "cd /app && git pull && make deploy"

# 书签: Database Backup
pg_dump -h localhost -U postgres mydb > backup_$(date +%Y%m%d).sql
```

### 场景 2: 项目专用命令
为每个项目保存特定的命令：
```bash
# 项目 A
make test-integration

# 项目 B
npm run build:production

# 项目 C
docker-compose -f docker-compose.dev.yml up
```

### 场景 3: 服务器管理
管理远程服务器的常用命令：
```bash
# SSH 到各环境
ssh prod-web-01
ssh staging-db

# 查看日志
tail -f /var/log/app/error.log

# 重启服务
sudo systemctl restart nginx
```

### 场景 4: 开发工作流
加速日常开发任务：
```bash
# 快速提交
git add . && git commit -m "WIP" && git push

# 运行测试
python -m pytest tests/ -v

# 格式化代码
black . && isort .
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/yaroslavborbat/zellij-bookmarks.git
cd zellij-bookmarks
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-bookmarks.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-bookmarks location="file:~/.config/zellij/plugins/zellij-bookmarks.wasm"
}

keybinds {
    normal {
        bind "Ctrl m" {
            LaunchOrFocusPlugin "zellij-bookmarks" {
                floating true
                height "50%"
                width "70%"
            }
        }
    }
}
```

### 创建书签配置

创建 `~/.config/zellij/bookmarks.json`：

```json
{
  "bookmarks": [
    {
      "name": "Deploy to Staging",
      "command": "ssh deploy@staging 'cd /app && git pull && make deploy'",
      "tags": ["deploy", "staging"],
      "description": "Deploy current branch to staging server"
    },
    {
      "name": "Run Tests",
      "command": "cargo test",
      "tags": ["test", "rust"],
      "description": "Run all Rust tests"
    },
    {
      "name": "Database Backup",
      "command": "pg_dump -h localhost -U postgres {database} > backup_{date}.sql",
      "tags": ["database", "backup"],
      "description": "Backup PostgreSQL database",
      "params": ["database", "date"]
    }
  ],
  "groups": [
    {
      "name": "Development",
      "bookmarks": ["Run Tests", "Database Backup"]
    },
    {
      "name": "Deployment",
      "bookmarks": ["Deploy to Staging"]
    }
  ]
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+m`（或你的配置键）打开书签面板。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zellij-bookmarks                                     │
├─────────────────────────────────────────────────────┤
│ > deploy                                             │
│                                                      │
│ 📁 Deployment                                        │
│   🚀 Deploy to Staging                               │
│      ssh deploy@staging 'cd /app && ...'             │
│      [deploy, staging]                               │
│                                                      │
│ 📁 Development                                       │
│   🧪 Run Tests                                       │
│      cargo test                                      │
│      [test, rust]                                    │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Enter] Insert  [e] Edit  [n] New  [d] Delete       │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+m` | 打开书签面板 |
| `↑/↓` | 导航书签 |
| `Enter` | 插入命令到终端 |
| `e` | 编辑命令后插入 |
| `n` | 创建新书签 |
| `d` | 删除书签 |
| `g` | 切换分组视图 |
| `t` | 按标签过滤 |
| `Esc` | 关闭面板 |

### 参数化命令

对于带有参数的书签：

```
选择 "Database Backup"
┌─────────────────────────────────────────────┐
│ Database Backup                             │
├─────────────────────────────────────────────┤
│ database: [myapp________________]           │
│ date:     [20250130______________]          │
│                                             │
│ 生成的命令:                                 │
│ pg_dump -h localhost -U postgres myapp >    │
│   backup_20250130.sql                       │
├─────────────────────────────────────────────┤
│ [Enter] Confirm  [Esc] Cancel               │
└─────────────────────────────────────────────┘
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `bookmarks_file` | string | `~/.config/zellij/bookmarks.json` | 书签配置文件路径 |
| `default_insert` | boolean | true | 默认直接插入（不编辑） |
| `confirm_delete` | boolean | true | 删除前确认 |
| `max_history` | integer | 50 | 最近使用历史记录数 |
| `case_sensitive` | boolean | false | 搜索是否区分大小写 |

---

## 书签配置格式

### 完整示例

```json
{
  "bookmarks": [
    {
      "name": "Display Name",
      "command": "actual command to execute",
      "description": "Optional description",
      "tags": ["tag1", "tag2"],
      "params": ["param1"],
      "cwd": "/optional/working/directory",
      "env": {
        "VAR1": "value1"
      }
    }
  ],
  "groups": [
    {
      "name": "Group Name",
      "description": "Group description",
      "bookmarks": ["Bookmark Name 1", "Bookmark Name 2"]
    }
  ],
  "settings": {
    "default_group": "Development",
    "sort_by": "name",
    "show_descriptions": true
  }
}
```

---

## 与其他工具对比

| 工具 | 用途 | 优点 | 缺点 |
|------|------|------|------|
| **zellij-bookmarks** | Zellij 命令书签 | 与 Zellij 集成 | 仅 Zellij |
| alias | Shell 别名 | 随处可用 | 需要记忆 |
| fzf + 历史 | 命令历史 | 自动记录 | 噪音多 |
| just | 命令运行器 | 项目管理 | 需要 justfile |
| tldr | 命令帮助 | 学习命令 | 不执行 |

---

## 故障排除

### 书签不显示
- 检查 `bookmarks.json` 文件路径
- 验证 JSON 语法
- 重启 Zellij

### 命令插入失败
- 确认当前面板是终端
- 检查命令格式
- 查看 Zellij 日志

### 参数不工作
- 确认 `params` 数组定义正确
- 检查参数名在命令中使用 `{}` 包裹
- 验证 JSON 格式

---

## 相关资源
- [官方仓库](https://github.com/yaroslavborbat/zellij-bookmarks)
- [zellij-playbooks](./zellij-playbooks.md) - 剧本执行插件
- [just](https://github.com/casey/just) - 命令运行器

---

*最后更新：2025年2月*
