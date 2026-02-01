# zellij-datetime 插件

> 在 Zellij 中添加日期和时间面板

**GitHub**: [h1romas4/zellij-datetime](https://github.com/h1romas4/zellij-datetime)

---

## 简介

`zellij-datetime` 是一个简单实用的 Zellij 插件，它添加了一个显示当前日期和时间的面板。这对于需要跟踪时间或记录日志的开发者非常有用。

---

## 功能特点

### 1. 实时时钟显示
- 实时更新的数字时钟
- 显示当前日期和时间
- 秒级精度更新

### 2. 可定制格式
- 自定义时间格式（strftime 格式）
- 自定义日期格式
- 自定义颜色和样式

### 3. 时区支持
- 显示本地时间
- 支持显示多个时区
- 适合远程团队协作

### 4. 多种显示模式
- 时间 + 日期
- 仅时间
- 仅日期
- 自定义组合

### 5. 低资源占用
- 最小化 CPU 使用
- 高效的更新机制
- 不影响系统性能

---

## 使用场景

### 场景 1: 长时间运行的任务
监控长时间运行的构建或测试：
```
开始时间: 09:00:00
当前时间: 11:30:45
已运行: 2小时30分钟
```

### 场景 2: 日志记录
需要记录操作时间：
```
随时查看当前时间
记录操作开始和结束时间
```

### 场景 3: 多时区协作
与分布在不同时区的团队协作：
```
本地: 09:00 CST
纽约: 20:00 EST
伦敦: 01:00 GMT+1
```

### 场景 4: 全屏工作
在全屏终端工作时查看时间：
```
不退出全屏即可查看时间
保持对时间的感知
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/h1romas4/zellij-datetime.git
cd zellij-datetime
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-datetime.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-datetime location="file:~/.config/zellij/plugins/zellij-datetime.wasm"
}

// 在布局中使用
layout {
    default_tab_template {
        pane size=1 borderless=true {
            plugin location="zellij:tab-bar"
        }
        pane
        pane size=1 borderless=true {
            plugin location="zellij:compact-bar"
        }
    }
    
    // 添加时间面板
    tab name="datetime" {
        pane {
            plugin location="zellij-datetime"
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-datetime location="file:~/.config/zellij/plugins/zellij-datetime.wasm" {
        // 时间格式（strftime）
        time_format "%H:%M:%S"
        
        // 日期格式
        date_format "%Y-%m-%d"
        
        // 分隔符
        separator " | "
        
        // 是否显示秒
        show_seconds true
        
        // 是否显示日期
        show_date true
        
        // 时区
        timezone "local"
        
        // 更新间隔（毫秒）
        update_interval 1000
        
        // 颜色
        time_color "#00ff00"
        date_color "#ffff00"
        separator_color "#ffffff"
    }
}
```

---

## 使用方法

### 作为独立面板

创建一个专门显示时间的标签：

```kdl
layout {
    tab name="clock" {
        pane {
            plugin location="zellij-datetime" {
                time_format "%H:%M:%S"
                date_format "%A, %B %d, %Y"
            }
        }
    }
}
```

### 与其他工具集成

在开发布局中作为参考：

```kdl
layout {
    pane split_direction="vertical" {
        pane size="70%" {
            // 主工作区
        }
        pane size="30%" split_direction="horizontal" {
            pane {
                // 终端或日志
            }
            pane size=3 {
                plugin location="zellij-datetime"
            }
        }
    }
}
```

### 界面显示

```
┌─────────────────────────┐
│                         │
│      14:30:45           │
│   2025-01-30            │
│                         │
└─────────────────────────┘
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `time_format` | string | "%H:%M:%S" | 时间格式（strftime） |
| `date_format` | string | "%Y-%m-%d" | 日期格式（strftime） |
| `separator` | string | " | " | 时间和日期分隔符 |
| `show_seconds` | boolean | true | 显示秒 |
| `show_date` | boolean | true | 显示日期 |
| `timezone` | string | "local" | 时区 |
| `update_interval` | integer | 1000 | 更新间隔（毫秒） |
| `time_color` | string | 自动 | 时间颜色 |
| `date_color` | string | 自动 | 日期颜色 |

### 时间格式参考

| 格式 | 示例 | 说明 |
|------|------|------|
| `%H:%M:%S` | 14:30:45 | 24小时制带秒 |
| `%I:%M:%S %p` | 02:30:45 PM | 12小时制带秒 |
| `%H:%M` | 14:30 | 24小时制不带秒 |
| `%Y-%m-%d` | 2025-01-30 | 年月日 |
| `%A, %B %d` | Thursday, January 30 | 星期几，月 日 |
| `%d/%m/%Y` | 30/01/2025 | 日月年格式 |

---

## 与 zellij-what-time 对比

| 特性 | zellij-datetime | zellij-what-time |
|------|-----------------|------------------|
| 位置 | 独立面板 | 状态栏 |
| 显示格式 | 可定制 | 可定制 |
| 多时区 | ✅ | ✅ |
| 位置 | 灵活 | 固定 |

**选择建议**：
- 需要大尺寸时钟显示 → zellij-datetime
- 需要在状态栏显示 → zellij-what-time

---

## 故障排除

### 时间不更新
- 检查 `update_interval` 设置
- 确认系统时间正确
- 查看 Zellij 日志

### 时间格式错误
- 检查 strftime 格式语法
- 确认使用支持的格式说明符
- 查看系统 locale 设置

### 时区不正确
- 检查系统时区设置
- 确认 `timezone` 配置正确
- 使用 `timedatectl` 检查系统时间

---

## 相关资源
- [官方仓库](https://github.com/h1romas4/zellij-datetime)
- [zellij-what-time](./zellij-what-time.md) - 状态栏时间显示
- [strftime 格式参考](https://man7.org/linux/man-pages/man3/strftime.3.html)

---

*最后更新：2025年2月*
