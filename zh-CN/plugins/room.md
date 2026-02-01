# room 插件

> 快速搜索和切换标签页

**GitHub**: [rvcas/room](https://github.com/rvcas/room)

---

## 简介

`room` 是一个简洁高效的 Zellij 插件，专注于快速搜索和切换标签页。它提供了一个直观的界面来浏览所有打开的标签页，并通过模糊搜索快速跳转到目标标签。

---

## 功能特点

### 1. 标签页列表
- 显示所有打开的标签页
- 显示标签页名称和编号
- 显示当前活动标签

### 2. 模糊搜索
- 实时模糊搜索标签名
- 智能匹配算法
- 快速过滤结果

### 3. 快速切换
- 一键跳转到选中标签
- 支持键盘完全操作
- 低延迟响应

### 4. 标签预览
- 显示标签页的简要信息
- 显示标签页中的面板数量
- 显示正在运行的命令

### 5. 界面简洁
- 极简主义设计
- 不干扰工作流
- 快速打开/关闭

---

## 使用场景

### 场景 1: 多标签开发
同时处理多个功能时快速切换：
```
标签 1: 用户模块开发
标签 2: 支付接口调试
标签 3: 文档编写
```

### 场景 2: 项目管理
为不同项目或任务使用不同标签：
```
标签 1: 项目 A - 后端
标签 2: 项目 A - 前端
标签 3: 项目 B - 调研
```

### 场景 3: 故障排查
在日志、代码、测试间快速切换：
```
标签 1: 应用日志
标签 2: 源代码
标签 3: 测试输出
```

### 场景 4: 学习和参考
同时打开多个学习资源：
```
标签 1: 教程文档
标签 2: 示例代码
标签 3: 练习项目
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/rvcas/room.git
cd room
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/room.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    room location="file:~/.config/zellij/plugins/room.wasm"
}

keybinds {
    normal {
        bind "Ctrl r" {
            LaunchOrFocusPlugin "room" {
                floating true
                height "40%"
                width "50%"
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    room location="file:~/.config/zellij/plugins/room.wasm" {
        // 显示选项
        show_index true
        show_pane_count true
        show_commands true
        
        // 搜索行为
        case_sensitive false
        max_results 20
        
        // 快捷键
        key_quit "Esc"
        key_select "Enter"
        key_next "Ctrl n"
        key_prev "Ctrl p"
    }
}
```

---

## 使用方法

### 启动插件

按 `Ctrl+r`（或你的配置键）打开 room 面板。

### 界面说明

```
┌─────────────────────────────────────┐
│ room - Tab Switcher                 │
├─────────────────────────────────────┤
│ > backend                           │
│                                     │
│ 1. frontend-dev        [3 panes]    │
│    npm run dev                      │
│                                     │
│ 2. backend-api *       [2 panes]    │
│    cargo run                        │
│                                     │
│ 3. documentation       [1 pane]     │
│    vim README.md                    │
└─────────────────────────────────────┘
* = current tab
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+r` | 打开/关闭 room |
| `↑/↓` | 选择标签页 |
| `Enter` | 切换到选中标签页 |
| `/` | 开始搜索 |
| `Esc` | 关闭 room |
| `d` | 删除选中标签页 |
| `r` | 重命名选中标签页 |

### 工作流示例

**快速切换到标签：**
```
1. 按 Ctrl+r 打开 room
2. 输入标签名的一部分（如 "api"）
3. 列表过滤显示匹配的标签
4. 按 Enter 切换到该标签
```

**管理多个项目：**
```
1. 创建命名标签：Ctrl+t, Ctrl+r → r → 输入名称
2. 快速切换：Ctrl+r → 输入项目名 → Enter
3. 关闭不需要的标签：Ctrl+r → 选中 → d
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `show_index` | boolean | true | 显示标签页编号 |
| `show_pane_count` | boolean | true | 显示面板数量 |
| `show_commands` | boolean | true | 显示正在运行的命令 |
| `case_sensitive` | boolean | false | 搜索是否区分大小写 |
| `max_results` | integer | 20 | 最大显示结果数 |
| `floating` | boolean | true | 是否以浮动窗口显示 |

---

## 与其他导航方式对比

| 导航方式 | 速度 | 使用场景 | 优点 |
|---------|------|----------|------|
| **room** | ⭐⭐⭐ | 多标签管理 | 搜索+切换一体 |
| Alt+数字 | ⭐⭐⭐ | 少量标签 | 最快 |
| 标签栏点击 | ⭐⭐ | GUI 用户 | 直观 |
| Ctrl+t → 方向 | ⭐⭐ | 相邻标签 | 顺序切换 |

**room 的优势**：
- 标签多时最有效
- 不需要记住标签位置
- 搜索功能强大

---

## 故障排除

### 标签不显示
- 确认 Zellij 版本兼容
- 检查插件是否正确加载
- 查看 Zellij 日志

### 搜索不工作
- 检查 `case_sensitive` 配置
- 确认标签名输入正确
- 尝试简化搜索词

### 切换失败
- 确认目标标签仍然存在
- 检查是否有权限问题
- 尝试重启 Zellij

---

## 相关资源

- [官方仓库](https://github.com/rvcas/room)
- [Zellij 标签页文档](https://zellij.dev/documentation/tabs.html)
- [zbuffers](./zbuffers.md) - 类似的标签切换插件

---

*最后更新：2025年2月*
