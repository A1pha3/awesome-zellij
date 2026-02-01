# zellij-what-time 插件

> 在状态栏显示主机系统日期和/或时间

**GitHub**: [pirafrank/zellij-what-time](https://github.com/pirafrank/zellij-what-time)

---

## 简介

`zellij-what-time` 是一个简洁的 Zellij 状态栏插件，受 [zellij-datetime](./zellij-datetime.md) 启发。它在状态栏中显示当前日期和时间，让你在全屏终端工作时也能随时了解时间。

---

## 功能特点

### 1. 时间显示
- 在状态栏显示当前时间
- 支持 12/24 小时格式
- 可配置显示秒

### 2. 日期显示
- 显示当前日期
- 多种日期格式可选
- 可独立开关

### 3. 状态栏集成
- 作为状态栏的一部分
- 与其他状态信息共存
- 不占用额外空间

### 4. 可定制格式
- 自定义时间格式
- 自定义日期格式
- 自定义分隔符

### 5. 时区支持
- 显示本地时间
- 支持显示其他时区
- 适合远程工作

---

## 使用场景

### 场景 1: 全屏开发
在全屏终端工作时查看时间：
```
无需退出全屏或切换应用
看一眼状态栏即可知道时间
```

### 场景 2: 时间敏感任务
需要时间提醒的工作：
```
会议前 5 分钟提醒
定时任务执行
工作时间追踪
```

### 场景 3: 远程服务器
在远程服务器上工作时：
```
显示服务器本地时间
或显示本地时间
便于协调操作
```

### 场景 4: 演示和直播
在演示或直播时：
```
观众可以看到当前时间
时间戳记录
增加专业感
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/pirafrank/zellij-what-time.git
cd zellij-what-time
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-what-time.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中替换默认状态栏或作为状态栏的一部分：

```kdl
plugins {
    zellij-what-time location="file:~/.config/zellij/plugins/zellij-what-time.wasm"
}

layout {
    default_tab_template {
        pane size=1 borderless=true {
            plugin location="zellij:tab-bar"
        }
        pane
        pane size=1 borderless=true {
            plugin location="zellij:compact-bar"  // 或 zellij-what-time
        }
    }
}
```

**注意**：此插件可以替换 compact-bar，或与其他状态栏插件一起使用。

### 高级配置

```kdl
plugins {
    zellij-what-time location="file:~/.config/zellij/plugins/zellij-what-time.wasm" {
        // 时间格式
        time_format "%H:%M"
        
        // 日期格式
        date_format "%Y-%m-%d"
        
        // 是否显示日期
        show_date true
        
        // 是否显示时间
        show_time true
        
        // 分隔符
        separator " | "
        
        // 时区（默认本地）
        timezone "local"
        
        // 更新间隔（秒）
        update_interval 60
        
        // 对齐方式
        alignment "right"  // left, center, right
    }
}
```

---

## 使用方法

### 作为独立状态栏

```kdl
layout {
    default_tab_template {
        pane size=1 borderless=true {
            plugin location="zellij:tab-bar"
        }
        pane
        pane size=1 borderless=true {
            plugin location="zellij-what-time"
        }
    }
}
```

### 界面显示

```
┌─────────────────────────────────────────────────────────────────┐
│ 1:project │ 2:docs* │ 3:config                   2025-01-30 │ 14:30 │
└─────────────────────────────────────────────────────────────────┘
```

### 格式示例

| 格式配置 | 显示结果 |
|---------|---------|
| `time_format "%H:%M"` | 14:30 |
| `time_format "%I:%M %p"` | 02:30 PM |
| `time_format "%H:%M:%S"` | 14:30:45 |
| `date_format "%Y-%m-%d"` | 2025-01-30 |
| `date_format "%d/%m/%Y"` | 30/01/2025 |
| `date_format "%A, %B %d"` | Thursday, January 30 |

---

## 与 zellij-datetime 对比

| 特性 | zellij-what-time | zellij-datetime |
|------|-----------------|-----------------|
| 位置 | 状态栏 | 独立面板 |
| 空间占用 | 最小 | 可配置 |
| 显示大小 | 紧凑 | 可大字体 |
| 适用场景 | 快速查看 | 详细查看 |

**选择建议**：
- 快速扫一眼 → zellij-what-time
- 详细时钟 → zellij-datetime

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `time_format` | string | "%H:%M" | 时间格式 |
| `date_format` | string | "%Y-%m-%d" | 日期格式 |
| `show_date` | boolean | true | 显示日期 |
| `show_time` | boolean | true | 显示时间 |
| `separator` | string | " | " | 日期和时间分隔符 |
| `timezone` | string | "local" | 时区 |
| `update_interval` | integer | 60 | 更新间隔（秒） |
| `alignment` | string | "right" | 对齐方式 |

---

## 故障排除

### 时间不更新
- 检查 `update_interval` 设置
- 确认系统时间正确
- 重启 Zellij

### 时间格式错误
- 使用正确的 strftime 格式
- 检查格式说明符
- 参考 strftime 文档

### 时区不正确
- 检查系统时区设置
- 设置 `timezone` 配置
- 使用 `timedatectl` 检查

---

## 相关资源
- [官方仓库](https://github.com/pirafrank/zellij-what-time)
- [zellij-datetime](./zellij-datetime.md) - 独立面板时钟
- [strftime 格式](https://man7.org/linux/man-pages/man3/strftime.3.html)

---

*最后更新：2025年2月*
