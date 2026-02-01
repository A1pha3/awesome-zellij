# monocole 插件

> 文件名和内容的模糊查找器

**GitHub**: [imsnif/monocole](https://github.com/imsnif/monocole)

---

## 简介

`monocole` 是一个强大的 Zellij 插件，提供文件名和文件内容的一体化模糊查找功能。它帮助你在项目中快速定位文件和代码片段，提升开发效率。

---

## 功能特点

### 1. 双模式搜索
- **文件名模式**：按文件名快速查找
- **内容模式**：在文件内容中搜索文本
- **混合模式**：同时匹配文件名和内容

### 2. 实时预览
- 选中结果时即时显示文件内容
- 语法高亮支持
- 显示匹配行的上下文（前后5行）

### 3. 智能模糊匹配
- 智能模糊搜索算法
- 支持拼音和缩写匹配
- 按相关性智能排序

### 4. 高性能搜索
- 使用 ripgrep 后端（如果可用）
- 支持大型代码库
- 增量搜索，实时更新结果

### 5. 键盘驱动
- 完全键盘操作
- Vim 风格的快捷键
- 可自定义键绑定

---

## 使用场景

### 场景 1: 快速打开文件
在大型项目中快速找到并打开文件：
```
输入 "usrsvc" → 匹配到 user_service.rs
```

### 场景 2: 查找代码片段
记得代码内容但不知道在哪个文件：
```
输入 "database connection" → 找到所有包含此文本的文件
```

### 场景 3: 重构辅助
查找所有使用某个函数或变量的地方：
- 搜索函数名
- 查看所有调用位置
- 批量替换

### 场景 4: 探索项目结构
刚接手新项目时了解代码组织：
- 浏览所有文件
- 查看关键文件内容
- 理解项目架构

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链
- ripgrep（可选但推荐）

### 安装 ripgrep（推荐）

```bash
# macOS
brew install ripgrep

# Ubuntu/Debian
apt-get install ripgrep

# Arch Linux
pacman -S ripgrep

# Cargo
cargo install ripgrep
```

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/imsnif/monocole.git
cd monocole
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/monocole.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    monocole location="file:~/.config/zellij/plugins/monocole.wasm"
}

keybinds {
    normal {
        bind "Ctrl o" {
            LaunchOrFocusPlugin "monocole" {
                floating true
                height "80%"
                width "90%"
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    monocole location="file:~/.config/zellij/plugins/monocole.wasm" {
        // 搜索范围
        search_paths ["."]
        exclude_paths ["target", ".git", "node_modules", "dist", "build"]
        
        // 文件类型过滤
        include_extensions ["rs", "js", "ts", "py", "md", "toml", "yaml"]
        exclude_extensions ["lock", "log"]
        
        // 搜索行为
        case_sensitive false
        max_results 100
        preview_lines 5
        
        // 快捷键
        key_quit "Esc"
        key_select "Enter"
        key_next "Ctrl n"
        key_prev "Ctrl p"
        key_toggle_mode "Ctrl t"
    }
}
```

---

## 使用方法

### 启动搜索

1. 按 `Ctrl+o`（或你的配置键）打开 monocole
2. 开始输入搜索词
3. 使用 `Ctrl+n/p` 或方向键导航结果
4. 按 `Enter` 打开选中项
5. 按 `Ctrl+t` 切换搜索模式

### 搜索模式

| 模式 | 快捷键 | 说明 |
|------|--------|------|
| 文件名 | `Ctrl+t` → `f` | 仅搜索文件名 |
| 内容 | `Ctrl+t` → `c` | 在文件内容中搜索 |
| 混合 | `Ctrl+t` → `m` | 同时匹配文件名和内容 |

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+o` | 打开 monocole |
| `Ctrl+n` / `↓` | 下一个结果 |
| `Ctrl+p` / `↑` | 上一个结果 |
| `Enter` | 打开选中文件 |
| `Ctrl+t` | 切换搜索模式 |
| `Esc` | 关闭 monocole |
| `Ctrl+c` | 复制文件路径 |
| `Ctrl+g` | 跳转到特定行 |

### 搜索语法

```
# 文件名搜索
main.rs           → 精确匹配
*.rs              → 通配符
usrsvc            → 模糊匹配 user_service.rs

# 内容搜索
fn main           → 查找函数定义
database.connect  → 查找方法调用
todo|fixme        → 查找 TODO 和 FIXME

# 高级搜索
path:src auth     → 在 src 目录中搜索 auth
type:rs struct    → 在 .rs 文件中搜索 struct
```

---

## 界面说明

```
┌─────────────────────────────────────────────────────┐
│ > database connection                                 │
├─────────────────────────────────────────────────────┤
│ 1. src/db.rs (45)                                    │
│    fn connect_to_database() -> Connection {          │
│        ...                                           │
│    }                                                 │
│                                                      │
│ 2. src/config.rs (12)                                │
│    database_url: String,                             │
│                                                      │
│ 3. tests/db_test.rs (8)                              │
│    setup_database_connection();                      │
└─────────────────────────────────────────────────────┘
[Ctrl+o] Search  [Enter] Open  [Ctrl+t] Mode  [Esc] Quit
```

---

## 性能优化

### 对于大型代码库

1. **使用 ripgrep**：自动检测并使用 ripgrep 作为搜索后端
2. **限制搜索范围**：配置 `search_paths` 缩小搜索范围
3. **排除大目录**：配置 `exclude_paths` 排除不需要的目录
4. **文件类型过滤**：使用 `include_extensions` 减少文件数

---

## 与类似工具对比

| 特性 | monocole | fzf | ripgrep | grep |
|------|----------|-----|---------|------|
| Zellij 集成 | ✅ | ⚠️ | ❌ | ❌ |
| 文件名搜索 | ✅ | ✅ | ❌ | ❌ |
| 内容搜索 | ✅ | ⚠️ | ✅ | ✅ |
| 实时预览 | ✅ | ⚠️ | ❌ | ❌ |
| 模糊匹配 | ✅ | ✅ | ❌ | ❌ |

**monocole 的优势**：
- 与 Zellij 深度集成，无需切换上下文
- 一体化体验，无需组合多个工具
- 实时预览，所见即所得

---

## 故障排除

### 搜索速度慢
- 安装 ripgrep 以获得更好的性能
- 配置 `exclude_paths` 排除大型目录
- 使用 `include_extensions` 限制文件类型

### 找不到文件
- 检查 `search_paths` 配置是否正确
- 确认文件没有被 `exclude_paths` 排除
- 尝试使用不同的搜索词

### 预览不显示
- 检查文件权限
- 确认文件不是二进制文件
- 查看 Zellij 日志

---

## 相关资源

- [官方仓库](https://github.com/imsnif/monocole)
- [ripgrep](https://github.com/BurntSushi/ripgrep)
- [fzf](https://github.com/junegunn/fzf)

---

*最后更新：2025年2月*
