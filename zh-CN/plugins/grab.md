# grab 插件

> 面向 Rust 开发者的模糊查找器（文件、结构体、枚举、函数）

**GitHub**: [imsnif/grab](https://github.com/imsnif/grab)

---

## 简介

`grab` 是专门为 Rust 开发者设计的 Zellij 模糊查找插件。它允许你在代码库中快速查找文件、结构体、枚举和函数定义，大大提升代码导航效率。

---

## 功能特点

### 1. Rust 语义感知
- 智能识别 Rust 代码结构
- 支持结构体（struct）、枚举（enum）、函数（fn）、 trait、impl 块等
- 理解 Rust 模块系统

### 2. 多维度搜索
- **文件搜索**：按文件名快速查找
- **符号搜索**：查找类型定义、函数、常量等
- **内容搜索**：在文件内容中搜索
- **混合搜索**：同时匹配文件名和内容

### 3. 实时预览
- 选中结果时即时显示文件内容
- 语法高亮显示
- 显示匹配行的上下文

### 4. 模糊匹配算法
- 智能模糊匹配
- 支持拼音和缩写
- 按相关性排序结果

### 5. 键盘驱动
- 完全键盘操作
- Vim 风格的导航
- 可自定义快捷键

---

## 使用场景

### 场景 1: 大型代码库导航
在大型 Rust 项目中快速找到所需的类型或函数：
```
输入 "user auth" → 找到 UserAuth 结构体和相关函数
```

### 场景 2: 学习新代码库
刚接手一个新项目，需要了解其结构：
- 快速浏览所有模块
- 查找 trait 实现
- 定位关键业务逻辑

### 场景 3: 重构辅助
重构时需要找到所有使用某个类型的地方：
- 搜索结构体定义
- 查看所有实现
- 找到引用位置

### 场景 4: API 探索
探索 crate 的公共 API：
- 列出所有公开函数
- 查看结构体字段
- 理解 trait 关系

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链
- ripgrep（推荐用于更好的性能）

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/imsnif/grab.git
cd grab
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/grab.wasm ~/.config/zellij/plugins/
```

### 可选：安装 ripgrep
```bash
# macOS
brew install ripgrep

# Ubuntu/Debian
apt-get install ripgrep

# Arch Linux
pacman -S ripgrep
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    grab location="file:~/.config/zellij/plugins/grab.wasm"
}

keybinds {
    normal {
        bind "Ctrl f" {
            LaunchOrFocusPlugin "grab" {
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
    grab location="file:~/.config/zellij/plugins/grab.wasm" {
        // 搜索范围
        search_paths ["src", "lib", "tests"]
        exclude_paths ["target", ".git", "node_modules"]
        
        // 显示设置
        preview_lines 5
        max_results 50
        
        // 文件类型过滤
        file_extensions ["rs", "toml"]
        
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

### 启动搜索

1. 按 `Ctrl+f`（或你的配置键）打开 grab
2. 开始输入搜索词
3. 使用 `Ctrl+n/p` 或方向键导航结果
4. 按 `Enter` 打开选中项

### 搜索语法

| 搜索类型 | 语法示例 | 说明 |
|---------|---------|------|
| 文件名 | `main.rs` | 查找特定文件 |
| 结构体 | `struct User` | 查找结构体定义 |
| 函数 | `fn login` | 查找函数定义 |
| 枚举 | `enum Status` | 查找枚举定义 |
| 模糊匹配 | `usrauth` | 匹配 UserAuth |
| 多关键词 | `user service` | 同时包含两者 |

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+f` | 打开 grab |
| `Ctrl+n` / `↓` | 下一个结果 |
| `Ctrl+p` / `↑` | 上一个结果 |
| `Enter` | 打开选中项 |
| `Esc` | 关闭 grab |
| `Ctrl+c` | 复制选中项路径 |
| `Ctrl+t` | 切换搜索类型 |

---

## 搜索模式详解

### 模式 1: 文件模式
```
> main
  src/main.rs
  tests/main_test.rs
  examples/main_example.rs
```

### 模式 2: 符号模式
```
> user auth
  ▶ struct UserAuth (src/auth.rs:15)
  ▶ impl UserAuth (src/auth.rs:25)
  ▶ fn authenticate_user (src/auth.rs:45)
```

### 模式 3: 内容模式
```
> database connection
  ▶ src/db.rs:42 - fn connect_to_database()
  ▶ src/config.rs:15 - database_url: String
  ▶ tests/db_test.rs:8 - setup_database_connection()
```

---

## 与 Zellij 集成

### 在布局中使用

```kdl
layout {
    pane split_direction="horizontal" {
        pane
        floating_panes {
            pane {
                plugin location="grab"
                x "10%"
                y "10%"
                width "80%"
                height "80%"
            }
        }
    }
}
```

### 与编辑器集成

在 Neovim/Vim 中使用 Zellij + grab：

```vim
" 在 Vim 中打开 grab
nnoremap <leader>fg :!zellij action launch-or-focus-plugin grab<CR>
```

---

## 性能优化

### 对于大型代码库

1. **使用 ripgrep**：安装 ripgrep 以获得更快的搜索速度
2. **限制搜索范围**：配置 `search_paths` 排除不需要的目录
3. **缓存结果**：使用 SSD 和足够的 RAM
4. **文件类型过滤**：使用 `file_extensions` 减少扫描文件数

---

## 与类似工具对比

| 特性 | grab | fzf | fd | LSP 符号搜索 |
|------|------|-----|-----|-------------|
| Rust 感知 | ✅ | ❌ | ❌ | ✅ |
| 模糊匹配 | ✅ | ✅ | ❌ | ⚠️ |
| 实时预览 | ✅ | ⚠️ | ❌ | ✅ |
| Zellij 集成 | ✅ | ⚠️ | ❌ | ❌ |
| 零配置 | ✅ | ✅ | ✅ | ❌ |

**grab 的优势**：
- 专为 Rust 设计，理解代码结构
- 与 Zellij 无缝集成
- 无需配置即可开箱即用

---

## 故障排除

### 搜索速度慢
- 检查是否安装了 ripgrep
- 减少搜索范围
- 排除大型目录（target、node_modules）

### 找不到符号
- 确认文件是有效的 Rust 代码
- 检查文件是否在搜索路径中
- 尝试使用更简单的搜索词

### 插件崩溃
- 更新到最新版本
- 检查 Zellij 兼容性
- 查看日志：`zellij --debug`

---

## 相关资源

- [官方仓库](https://github.com/imsnif/grab)
- [ripgrep](https://github.com/BurntSushi/ripgrep)
- [Rust 语言](https://www.rust-lang.org/)

---

*最后更新：2025年2月*
