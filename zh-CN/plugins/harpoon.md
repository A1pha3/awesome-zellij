# harpoon 插件

> 快速导航面板（Neovim Harpoon 的克隆）

**GitHub**: [Nacho114/harpoon](https://github.com/Nacho114/harpoon)

---

## 简介

`harpoon` 是受 Neovim 著名插件 [Harpoon](https://github.com/ThePrimeagen/harpoon) 启发的 Zellij 插件。它允许你快速标记和跳转到常用的面板，大幅提升在多面板工作区中的导航效率。

---

## 功能特点

### 1. 快速标记系统
- 为常用面板添加数字标记（1-9）
- 一键跳转到标记的面板
- 标记持久化，重启后仍然保留

### 2. 快速导航
- 使用数字键直接跳转到对应面板
- 无需查看面板标题或位置
- 类似 Vim 标记（marks）的工作流

### 3. 可视化标记列表
- 显示所有已标记的面板
- 查看标记编号和面板信息
- 支持从列表中移除标记

### 4. 多会话支持
- 每个 Zellij 会话有独立的标记列表
- 支持跨会话同步（可选配置）
- 会话恢复时标记自动加载

### 5. 低干扰设计
- 简洁的界面设计
- 快速显示/隐藏
- 不干扰正常的工作流

---

## 使用场景

### 场景 1: 固定开发面板
在开发过程中固定常用的面板：
```
1: 主编辑器 (Neovim)
2: 测试运行器
3: 数据库客户端
4: 日志监控
5: 文档服务器
```

### 场景 2: 多项目工作流
同时处理多个项目时快速切换：
```
1: 项目 A - 前端
2: 项目 A - 后端
3: 项目 B - 编辑器
4: 项目 B - 终端
```

### 场景 3: DevOps 监控
监控多个服务和日志：
```
1: Docker 日志
2: Kubernetes 状态
3: 应用日志
4: 数据库监控
```

### 场景 4: 教学演示
教学中快速切换到预设的面板：
- 代码示例面板
- 运行结果面板
- 命令行演示面板

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链（用于编译）

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/Nacho114/harpoon.git
cd harpoon
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/harpoon.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    harpoon location="file:~/.config/zellij/plugins/harpoon.wasm"
}

keybinds {
    normal {
        // 打开 harpoon 界面
        bind "Ctrl h" {
            LaunchOrFocusPlugin "harpoon" {
                floating true
                height "40%"
                width "50%"
            }
        }
        
        // 直接跳转到标记 1-9
        bind "Alt 1" { GoToMark 1; }
        bind "Alt 2" { GoToMark 2; }
        bind "Alt 3" { GoToMark 3; }
        bind "Alt 4" { GoToMark 4; }
        bind "Alt 5" { GoToMark 5; }
        bind "Alt 6" { GoToMark 6; }
        bind "Alt 7" { GoToMark 7; }
        bind "Alt 8" { GoToMark 8; }
        bind "Alt 9" { GoToMark 9; }
    }
}
```

### 高级配置

```kdl
plugins {
    harpoon location="file:~/.config/zellij/plugins/harpoon.wasm" {
        // 最大标记数
        max_marks 9
        
        // 是否显示面板标题
        show_titles true
        
        // 是否显示命令
        show_commands true
        
        // 标记保存路径
        save_path "~/.cache/zellij/harpoon"
        
        // 自动保存间隔（秒）
        autosave_interval 30
    }
}
```

---

## 使用方法

### 基本操作

**打开 harpoon 界面：**
```
Ctrl+h    - 打开/关闭 harpoon 标记列表
```

**在界面内的操作：**
```
1-9       - 为当前面板添加/更新对应数字的标记
d         - 删除当前高亮的标记
q/Esc     - 关闭 harpoon 界面
Enter     - 跳转到高亮标记的面板
```

**快速跳转：**
```
Alt+1     - 跳转到标记 1 的面板
Alt+2     - 跳转到标记 2 的面板
Alt+3     - 跳转到标记 3 的面板
...
Alt+9     - 跳转到标记 9 的面板
```

### 工作流程示例

**第一步：标记重要面板**
```
1. 在主编辑器面板中按 Ctrl+h
2. 按 1 标记为 "1"
3. 切换到测试面板
4. 按 Ctrl+h，然后按 2 标记为 "2"
5. 重复标记其他面板
```

**第二步：快速导航**
```
Alt+1     - 瞬间跳转到编辑器
Alt+2     - 瞬间跳转到测试面板
Alt+3     - 瞬间跳转到日志面板
```

**第三步：管理标记**
```
Ctrl+h    - 打开列表
d         - 删除不再需要的标记
Enter     - 跳转到选中面板
```

---

## 工作流示例

### 典型开发工作流

```
面板布局：
┌─────────────┬─────────────┐
│  1: Neovim  │  2: Tests   │
├─────────────┼─────────────┤
│  3: Logs    │  4: Shell   │
└─────────────┴─────────────┘

快捷键：
Alt+1 → 切换到代码编辑
Alt+2 → 查看测试结果
Alt+3 → 查看应用日志
Alt+4 → 执行 shell 命令
```

### 服务器管理工作流

```
面板布局：
┌─────────────┬─────────────┬─────────────┐
│  1: 日志A   │  2: 日志B   │  3: 监控    │
└─────────────┴─────────────┴─────────────┘

快捷键：
Alt+1 → 查看服务 A 日志
Alt+2 → 查看服务 B 日志
Alt+3 → 查看系统监控
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `max_marks` | integer | 9 | 最大标记数量（1-9） |
| `show_titles` | boolean | true | 显示面板标题 |
| `show_commands` | boolean | true | 显示面板运行的命令 |
| `save_path` | string | 默认缓存目录 | 标记保存路径 |
| `autosave_interval` | integer | 30 | 自动保存间隔（秒） |
| `confirm_delete` | boolean | true | 删除前是否确认 |

---

## 与其他导航方式对比

| 导航方式 | 速度 | 精确度 | 记忆负担 | 适用场景 |
|---------|------|--------|----------|----------|
| **harpoon** | ⭐⭐⭐ | ⭐⭐⭐ | 低 | 固定工作流 |
| Alt+方向键 | ⭐⭐ | ⭐⭐ | 中 | 相邻面板 |
| 面板选择器 | ⭐⭐ | ⭐⭐⭐ | 中 | 不确定位置 |
| 直接点击 | ⭐ | ⭐⭐⭐ | 低 | GUI 环境 |

**harpoon 的优势**：
- 最快的导航速度（肌肉记忆）
- 适合有固定工作流的用户
- 不依赖面板视觉位置

---

## 故障排除

### 标记不保存
- 检查 `save_path` 是否有写入权限
- 确认 `autosave_interval` 设置正确
- 尝试手动保存（在界面中按 `s`）

### 跳转失败
- 确认目标面板仍然存在
- 检查标记是否被删除
- 重新添加标记

### 快捷键冲突
- 检查其他 Zellij 插件的快捷键
- 检查终端模拟器的快捷键
- 在配置中修改快捷键

---

## 与 Neovim Harpoon 的关系

| 特性 | Zellij harpoon | Neovim Harpoon |
|------|---------------|----------------|
| 标记面板 | ✅ | - |
| 标记文件 | - | ✅ |
| 快速导航 | ✅ | ✅ |
| 终端集成 | ✅ | ⚠️ |

如果你同时使用 Zellij 和 Neovim，可以：
- 使用 Zellij harpoon 标记终端面板
- 使用 Neovim Harpoon 标记文件
- 两者结合获得最佳体验

---

## 相关资源

- [官方仓库](https://github.com/Nacho114/harpoon)
- [Neovim Harpoon](https://github.com/ThePrimeagen/harpoon)
- [Zellij 面板管理文档](https://zellij.dev/documentation/panes.html)

---

*最后更新：2025年2月*
