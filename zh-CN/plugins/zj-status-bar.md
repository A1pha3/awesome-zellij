# zj-status-bar 插件

> compact-bar 插件的意见分支

**GitHub**: [cristiand391/zj-status-bar](https://github.com/cristiand391/zj-status-bar)

---

## 简介

`zj-status-bar` 是 Zellij 官方 compact-bar 插件的一个意见分支。它提供了作者对状态栏的个人偏好配置，包括不同的默认颜色、格式和行为。

---

## 功能特点

### 1. 个性化设计
- 与原版不同的视觉风格
- 独特的配色方案
- 改进的界面布局

### 2. 增强显示
- 更多信息显示
- 改进的标签样式
- 更好的状态指示

### 3. 自定义配置
- 灵活的配置选项
- 可调整显示内容
- 主题支持

### 4. 与原版的区别
- 不同的默认设置
- 改进的用户体验
- 额外的显示选项

---

## 使用场景

### 场景 1: 视觉偏好
如果你喜欢不同的状态栏外观：
```
与官方 compact-bar 不同的风格
独特的颜色搭配
个性化的布局
```

### 场景 2: 信息密度
需要更多信息显示：
```
显示额外的系统信息
改进的标签指示
更丰富的状态反馈
```

### 场景 3: 尝试替代方案
对比不同状态栏插件：
```
尝试多个状态栏插件
找到最适合的
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/cristiand391/zj-status-bar.git
cd zj-status-bar
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zj-status-bar.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中替换默认状态栏：

```kdl
plugins {
    zj-status-bar location="file:~/.config/zellij/plugins/zj-status-bar.wasm"
}

layout {
    default_tab_template {
        pane size=1 borderless=true {
            plugin location="zellij:tab-bar"
        }
        pane
        pane size=1 borderless=true {
            plugin location="zj-status-bar"
        }
    }
}
```

---

## 与官方 compact-bar 对比

| 特性 | zj-status-bar | compact-bar |
|------|---------------|-------------|
| 视觉风格 | 意见化 | 官方默认 |
| 配色方案 | 独特 | 标准 |
| 信息密度 | 较高 | 标准 |
| 定制性 | 高 | 中 |

---

## 相关资源
- [官方仓库](https://github.com/cristiand391/zj-status-bar)
- [zellij-cb](./zellij-cb.md) - 另一个状态栏插件
- [zjstatus](./zjstatus.md) - 高级状态栏插件

---

*最后更新：2025年2月*
