# zellij-qr-share 插件

> 在终端中将 Web 令牌显示为 QR 码，用于 Web UI 的快速移动设备认证

**GitHub**: [dbachelder/zellij-qr-share](https://github.com/dbachelder/zellij-qr-share)

---

## 简介

`zellij-qr-share` 是一个实用的 Zellij 插件，它可以生成 QR 码，让你在终端中轻松分享 Web 令牌、URL 或其他文本。特别适用于需要在移动设备上快速认证或访问 Web UI 的场景。

---

## 功能特点

### 1. QR 码生成
- 在终端中生成 QR 码
- 支持各种内容类型
- 自动调整大小以适应终端

### 2. 令牌分享
- 分享 Web 认证令牌
- 分享 URL 链接
- 分享任意文本内容

### 3. 移动端友好
- 生成的 QR 码易于扫描
- 适合手机摄像头识别
- 支持各种 QR 码阅读器

### 4. 多种输入方式
- 从环境变量读取
- 从文件读取
- 直接输入文本

### 5. 可定制显示
- 调整 QR 码大小
- 自定义边距
- 错误纠正级别

---

## 使用场景

### 场景 1: Web UI 认证
为 Web 应用生成认证令牌 QR 码：
```bash
# 生成登录令牌
zellij pipe --plugin zellij-qr-share -- "https://app.example.com/auth?token=abc123"
# 手机扫描 QR 码自动登录
```

### 场景 2: WiFi 密码分享
快速分享 WiFi 密码：
```bash
# 生成 WiFi QR 码
zellij pipe --plugin zellij-qr-share -- "WIFI:T:WPA;S:MyNetwork;P:password123;;"
# 手机扫描自动连接 WiFi
```

### 场景 3: URL 分享
分享长 URL 到移动设备：
```bash
# 生成长链接 QR 码
zellij pipe --plugin zellij-qr-share -- "https://example.com/very/long/url/path"
# 手机扫描快速打开
```

### 场景 4: 密钥分享
分享 API 密钥或访问令牌：
```bash
# 生成 API 密钥 QR 码
export API_KEY="sk-abc123xyz"
zellij pipe --plugin zellij-qr-share -- "$API_KEY"
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/dbachelder/zellij-qr-share.git
cd zellij-qr-share
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-qr-share.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-qr-share location="file:~/.config/zellij/plugins/zellij-qr-share.wasm"
}

keybinds {
    normal {
        bind "Ctrl q" {
            LaunchOrFocusPlugin "zellij-qr-share" {
                floating true
                height "50%"
                width "50%"
            }
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-qr-share location="file:~/.config/zellij/plugins/zellij-qr-share.wasm" {
        // QR 码设置
        qr_size "medium"  // small, medium, large
        error_correction "M"  // L, M, Q, H
        margin 2
        
        // 显示选项
        show_text true
        text_below true
        
        // 内容来源
        default_source "clipboard"  // clipboard, input, env
    }
}
```

---

## 使用方法

### 命令行使用

使用 `zellij pipe` 生成 QR 码：

```bash
# 生成 URL QR 码
zellij pipe --plugin zellij-qr-share -- "https://example.com"

# 生成 WiFi QR 码
zellij pipe --plugin zellij-qr-share -- "WIFI:T:WPA;S:Network;P:Password;;"

# 从环境变量生成
zellij pipe --plugin zellij-qr-share -- "$TOKEN"
```

### 交互式使用

按 `Ctrl+q` 打开插件，然后输入内容：

```
┌─────────────────────────────────────────────────────┐
│ zellij-qr-share                                      │
├─────────────────────────────────────────────────────┤
│                                                      │
│ Enter text to encode:                                │
│ https://example.com/token?auth=abc123               │
│                                                      │
│ [Enter] Generate  [Ctrl+v] Paste  [Esc] Cancel      │
└─────────────────────────────────────────────────────┘
```

### 显示结果

生成的 QR 码：

```
┌─────────────────────────────────────────────────────┐
│                                                      │
│ ████████████████████████████████████████████        │
│ ██  ██  ████  ████  ██      ██  ████  ██  ██        │
│ ██  ██  ████  ████  ██      ██  ████  ██  ██        │
│ ██████  ██  ██  ████████████████  ██  ██████        │
│ ...（QR 码图案）...                                  │
│ ██████  ██  ██  ████████████████  ██  ██████        │
│ ██  ██  ████  ████  ██      ██  ████  ██  ██        │
│ ██  ██  ████  ████  ██      ██  ████  ██  ██        │
│ ████████████████████████████████████████████        │
│                                                      │
│ https://example.com/token?auth=abc123               │
│                                                      │
├─────────────────────────────────────────────────────┤
│ [Esc] Close  [s] Save to File                       │
└─────────────────────────────────────────────────────┘
```

---

## 实用示例

### 生成 WiFi QR 码

```bash
#!/bin/bash
# wifi-qr.sh - 生成 WiFi QR 码

SSID="$1"
PASSWORD="$2"
SECURITY="${3:-WPA}"

WIFI_STRING="WIFI:T:${SECURITY};S:${SSID};P:${PASSWORD};;"

zellij pipe --plugin zellij-qr-share -- "$WIFI_STRING"
```

使用：
```bash
./wifi-qr.sh "MyHomeNetwork" "secretpassword"
```

### 分享开发服务器 URL

```bash
#!/bin/bash
# dev-server-qr.sh

PORT="${1:-3000}"
IP=$(hostname -I | awk '{print $1}')
URL="http://${IP}:${PORT}"

echo "Development server: $URL"
zellij pipe --plugin zellij-qr-share -- "$URL"
```

### 分享 GitHub 仓库

```bash
# 获取当前 git 仓库 URL
REPO_URL=$(git remote get-url origin 2>/dev/null | sed 's/git@github.com:/https:\/\/github.com\//')

if [ -n "$REPO_URL" ]; then
    zellij pipe --plugin zellij-qr-share -- "$REPO_URL"
else
    echo "Not in a git repository"
fi
```

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `qr_size` | string | "medium" | QR 码大小 |
| `error_correction` | string | "M" | 错误纠正级别 |
| `margin` | integer | 2 | 边距大小 |
| `show_text` | boolean | true | 显示原始文本 |
| `text_below` | boolean | true | 文本在 QR 码下方 |
| `default_source` | string | "clipboard" | 默认内容来源 |

### 错误纠正级别

| 级别 | 容错能力 | 适用场景 |
|------|---------|---------|
| L | 7% | 清晰的显示环境 |
| M | 15% | 标准显示（默认） |
| Q | 25% | 可能有部分遮挡 |
| H | 30% | 恶劣的显示环境 |

---

## 故障排除

### QR 码无法扫描
- 增大 `qr_size` 设置
- 提高 `error_correction` 级别
- 确保终端背景与 QR 码对比度高
- 检查终端字体是否等宽

### 内容太长
- QR 码有内容长度限制
- 尝试使用 URL 缩短服务
- 考虑使用多个 QR 码

### 显示异常
- 确认使用等宽字体
- 检查终端尺寸是否足够
- 尝试调整 `margin` 设置

---

## 相关资源
- [官方仓库](https://github.com/dbachelder/zellij-qr-share)
- [QR 码标准](https://www.qrcode.com/)
- [WiFi QR 码格式](https://github.com/zxing/zxing/wiki/Barcode-Contents)

---

*最后更新：2025年2月*
