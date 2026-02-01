# zellij-favs 插件

> 保存收藏会话并清除其他会话

**GitHub**: [JoseMM2002/zellij-favs](https://github.com/JoseMM2002/zellij-favs)

---

## 简介

`zellij-favs` 是一个会话管理插件，允许你标记常用的 Zellij 会话为"收藏"，并快速清理其他不再需要的会话。它帮助你保持会话列表整洁，同时保留重要的工作环境。

---

## 功能特点

### 1. 收藏会话
- 标记重要会话为收藏
- 收藏会话显示特殊标识
- 收藏信息持久化保存

### 2. 快速清理
- 一键删除所有非收藏会话
- 确认机制防止误删
- 批量操作支持

### 3. 会话筛选
- 按收藏状态筛选会话
- 显示/隐藏非收藏会话
- 快速切换视图

### 4. 优先级排序
- 收藏会话优先显示
- 自定义排序规则
- 常用会话快速访问

### 5. 可视化标识
- 收藏会话特殊标记
- 清晰的视觉区分
- 一目了然的管理

---

## 使用场景

### 场景 1: 项目会话管理
为重要项目保留会话，清理临时会话：
```
收藏：project-a, project-b, infrastructure
临时：test-1, test-2, old-project（可被清理）
```

### 场景 2: 长期工作流
保留长期运行的开发环境：
```
收藏：main-dev, documentation, monitoring
清理：昨天的一次性查询会话
```

### 场景 3: 团队协作
标记团队共享的会话：
```
收藏：shared-docs, team-knowledge-base
个人：my-experiments（可随时清理）
```

### 场景 4: 定期维护
每周清理一次临时会话：
```
1. 查看所有会话
2. 标记重要的为收藏
3. 一键清理其他会话
4. 保持列表整洁
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/JoseMM2002/zellij-favs.git
cd zellij-favs
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-favs.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-favs location="file:~/.config/zellij/plugins/zellij-favs.wasm"
}

keybinds {
    normal {
        bind "Ctrl f" {
            LaunchOrFocusPlugin "zellij-favs" {
                floating true
                height "60%"
                width "70%"
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-favs location="file:~/.config/zellij/plugins/zellij-favs.wasm" {
        // 收藏保存路径
        favorites_file "~/.config/zellij/favorites.json"
        
        // 显示选项
        show_favorites_first true
        show_non_favorites true
        
        // 清理选项
        confirm_cleanup true
        backup_before_cleanup false
        
        // 视觉效果
        favorite_indicator "★"
        non_favorite_indicator "○"
        
        // 快捷键
        key_toggle_favorite "f"
        key_cleanup "c"
        key_switch "Enter"
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+f`（或你的配置键）打开 zellij-favs 面板。

### 界面说明

```
┌─────────────────────────────────────────────────────┐
│ zellij-favs - Session Favorites                      │
├─────────────────────────────────────────────────────┤
│                                                      │
│ ★ project-a           [2 tabs, 5 panes]             │
│ ★ project-b           [3 tabs, 4 panes]             │
│ ★ documentation       [1 tab, 2 panes]              │
│                                                      │
│ ○ test-session-1      [1 tab, 1 pane]               │
│ ○ tmp-query           [1 tab, 1 pane]               │
│ ○ old-experiment      [2 tabs, 3 panes]             │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [f] Toggle Favorite  [c] Cleanup Others  [Enter] Go │
│ [Esc] Quit                                           │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+f` | 打开 zellij-favs |
| `↑/↓` | 导航会话列表 |
| `f` | 切换收藏状态 |
| `c` | 清理所有非收藏会话 |
| `Enter` | 切换到选中的会话 |
| `d` | 删除单个会话 |
| `Esc` | 关闭面板 |

### 工作流程

**标记收藏会话：**
```
1. 按 Ctrl+f 打开面板
2. 导航到要收藏的会话
3. 按 f 标记为收藏（显示 ★）
4. 按 Esc 关闭面板
```

**清理临时会话：**
```
1. 按 Ctrl+f 打开面板
2. 确认重要的会话已标记为收藏
3. 按 c 清理所有非收藏会话
4. 确认删除操作
5. 所有 ○ 会话被删除，★ 会话保留
```

**快速切换会话：**
```
1. 按 Ctrl+f 打开面板
2. 使用 ↑/↓ 选择目标会话
3. 按 Enter 切换到该会话
```

---

## 配置文件

### favorites.json 示例

```json
{
  "favorites": [
    "project-a",
    "project-b",
    "documentation"
  ],
  "settings": {
    "confirm_cleanup": true,
    "show_favorites_first": true
  },
  "metadata": {
    "project-a": {
      "added_at": "2025-01-15T10:30:00Z",
      "description": "Main development session"
    },
    "project-b": {
      "added_at": "2025-01-20T14:00:00Z",
      "description": "Secondary project"
    }
  }
}
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `favorites_file` | string | `~/.config/zellij/favorites.json` | 收藏配置文件路径 |
| `show_favorites_first` | boolean | true | 收藏会话优先显示 |
| `show_non_favorites` | boolean | true | 显示非收藏会话 |
| `confirm_cleanup` | boolean | true | 清理前确认 |
| `backup_before_cleanup` | boolean | false | 清理前备份 |
| `favorite_indicator` | string | "★" | 收藏标识符 |
| `non_favorite_indicator` | string | "○" | 非收藏标识符 |

---

## 与其他会话管理工具对比

| 工具 | 功能 | 优势 |
|------|------|------|
| **zellij-favs** | 收藏+清理 | 快速整理 |
| zellij-choose-tree | 树形浏览 | 详细视图 |
| zellij-sessionizer | 创建会话 | 项目关联 |
| zsm | zoxide 集成 | 智能切换 |

**建议组合使用**：
- zellij-favs：日常清理和标记
- zsm/zellij-sessionizer：快速切换

---

## 故障排除

### 收藏不保存
- 检查 `favorites_file` 路径
- 确认目录有写入权限
- 验证 JSON 格式

### 会话不显示
- 确认 Zellij 版本兼容
- 检查插件是否正确加载
- 重启 Zellij

### 清理失败
- 检查会话是否仍在运行
- 确认有足够的权限
- 尝试手动删除

---

## 相关资源
- [官方仓库](https://github.com/JoseMM2002/zellij-favs)
- [zellij-choose-tree](./zellij-choose-tree.md) - 会话树形浏览
- [zellij-sessionizer](./zellij-sessionizer.md) - 会话创建
- [zsm](./zsm.md) - zoxide 集成会话切换

---

*最后更新：2025年2月*
