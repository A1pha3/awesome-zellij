# zellij-load 插件

> 显示系统资源，如 CPU、内存和 GPU 使用率

**GitHub**: [Christian-Prather/zellij-load](https://github.com/Christian-Prather/zellij-load)

---

## 简介

`zellij-load` 是一个系统监控插件，在 Zellij 面板中实时显示系统资源使用情况，包括 CPU、内存、GPU 等。它类似于 tmux 的 cpu-usage 插件，让你可以随时监控终端复用器所在系统的性能状态。

---

## 功能特点

### 1. CPU 监控
- 显示总体 CPU 使用率
- 显示各核心使用率（多核 CPU）
- 实时更新图表

### 2. 内存监控
- 显示内存使用率
- 显示已用/可用内存
- 显示交换空间使用情况

### 3. GPU 监控
- 显示 GPU 使用率（需要支持的工具）
- 显示 GPU 内存使用
- 支持 NVIDIA、AMD 等主流显卡

### 4. 可视化图表
- 文本图表显示历史趋势
- 彩色进度条
- 自定义刷新率

### 5. 可定制显示
- 选择显示哪些指标
- 自定义颜色和格式
- 调整更新频率

---

## 使用场景

### 场景 1: 开发时的资源监控
在开发过程中监控系统资源：
```
编译大型项目时查看 CPU 使用率
运行测试时监控内存消耗
训练机器学习模型时查看 GPU 状态
```

### 场景 2: 服务器管理
远程服务器监控：
```
SSH 到服务器
在 Zellij 中运行 zellij-load
实时监控系统资源
发现异常时及时处理
```

### 场景 3: 性能调优
进行性能优化时的参考：
```
运行性能测试
观察 CPU 和内存峰值
优化代码后对比资源使用
```

### 场景 4: 嵌入式开发
在资源受限的环境中：
```
监控内存使用防止溢出
查看 CPU 占用确保实时性
在终端中一站式监控系统
```

---

## 安装方法

### 前提条件
- Zellij >= 0.38.0
- Rust 工具链
- Linux 系统（用于读取 /proc 信息）

### GPU 监控前提
- NVIDIA GPU：需要安装 `nvidia-smi`
- AMD GPU：需要安装 `rocm-smi`
- Intel GPU：需要安装 `intel-gpu-top`

### 从源码安装

1. 克隆仓库：
```bash
git clone https://github.com/Christian-Prather/zellij-load.git
cd zellij-load
```

2. 编译插件：
```bash
cargo build --release --target wasm32-wasi
```

3. 安装插件：
```bash
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasi/release/zellij-load.wasm ~/.config/zellij/plugins/
```

---

## 配置方法

### 基本配置

在 `~/.config/zellij/config.kdl` 中：

```kdl
plugins {
    zellij-load location="file:~/.config/zellij/plugins/zellij-load.wasm"
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
    
    // 添加系统监控面板
    tab name="monitor" {
        pane {
            plugin location="zellij-load"
        }
    }
}
```

### 高级配置

```kdl
plugins {
    zellij-load location="file:~/.config/zellij/plugins/zellij-load.wasm" {
        // 显示选项
        show_cpu true
        show_memory true
        show_gpu true
        show_disk false
        show_network false
        
        // CPU 设置
        show_cores true
        cpu_history_length 20
        
        // 内存设置
        show_swap true
        memory_unit "GB"  // GB, MB, %
        
        // GPU 设置
        gpu_tool "auto"   // auto, nvidia-smi, rocm-smi, intel
        
        // 显示设置
        use_colors true
        color_high "#ff0000"
        color_medium "#ffff00"
        color_low "#00ff00"
        
        // 更新间隔（毫秒）
        update_interval 1000
    }
}
```

---

## 使用方法

### 作为独立面板

创建一个专门的系统监控标签：

```kdl
layout {
    tab name="system" {
        pane split_direction="horizontal" {
            pane size="30%" {
                plugin location="zellij-load"
            }
            pane {
                // 主工作区
            }
        }
    }
}
```

### 界面显示

```
┌─────────────────────────────────────────────────────┐
│ System Monitor                                       │
├─────────────────────────────────────────────────────┤
│                                                      │
│ CPU Usage: 45%                                       │
│ ████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │
│ Core 0: 52%  Core 1: 38%  Core 2: 45%  Core 3: 47% │
│                                                      │
│ Memory: 8.2 GB / 16 GB (51%)                        │
│ ███████████████████████████░░░░░░░░░░░░░░░░░░░░░░ │
│ Swap: 0.5 GB / 2 GB (25%)                           │
│                                                      │
│ GPU (NVIDIA RTX 3080): 67%                          │
│ ███████████████████████████████░░░░░░░░░░░░░░░░░░ │
│ VRAM: 6.1 GB / 10 GB (61%)                          │
│                                                      │
└─────────────────────────────────────────────────────┘
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `r` | 刷新显示 |
| `c` | 切换颜色显示 |
| `u` | 切换单位 |
| `q/Esc` | 退出（如果是浮动面板） |

---

## 配置选项

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `show_cpu` | boolean | true | 显示 CPU 使用率 |
| `show_memory` | boolean | true | 显示内存使用率 |
| `show_gpu` | boolean | true | 显示 GPU 使用率 |
| `show_disk` | boolean | false | 显示磁盘使用率 |
| `show_network` | boolean | false | 显示网络使用情况 |
| `show_cores` | boolean | true | 显示各核心使用率 |
| `cpu_history_length` | integer | 20 | CPU 历史图表长度 |
| `show_swap` | boolean | true | 显示交换空间 |
| `memory_unit` | string | "GB" | 内存单位 |
| `gpu_tool` | string | "auto" | GPU 监控工具 |
| `use_colors` | boolean | true | 使用彩色显示 |
| `update_interval` | integer | 1000 | 更新间隔（毫秒） |

---

## GPU 监控配置

### NVIDIA GPU

确保已安装 `nvidia-smi`：
```bash
# Ubuntu/Debian
apt-get install nvidia-utils

# 验证安装
nvidia-smi
```

### AMD GPU

确保已安装 `rocm-smi`：
```bash
# Ubuntu/Debian（需要 ROCm 仓库）
apt-get install rocm-smi-lib

# 验证安装
rocm-smi
```

### Intel GPU

确保已安装 `intel-gpu-tools`：
```bash
# Ubuntu/Debian
apt-get install intel-gpu-tools

# 验证安装
intel_gpu_top -L
```

---

## 与系统监控工具对比

| 工具 | 优点 | 缺点 |
|------|------|------|
| **zellij-load** | 集成在 Zellij 中 | 功能较简单 |
| htop | 功能丰富 | 需要独立运行 |
| btop | 美观 | 需要独立运行 |
| tmux cpu | 适用于 tmux | 不适用于 Zellij |

**zellij-load 的优势**：
- 无需离开 Zellij 即可监控
- 始终可见（作为面板）
- 与 Zellij 风格一致

---

## 故障排除

### 数据显示为 0
- 确认系统支持（Linux /proc 文件系统）
- 检查是否有读取权限
- 尝试以 root 运行测试

### GPU 不显示
- 确认 `nvidia-smi` 或相应工具已安装
- 检查 GPU 驱动是否正确安装
- 尝试手动运行 GPU 工具

### 更新频率不正常
- 检查 `update_interval` 设置
- 确认系统负载不过高
- 尝试降低更新频率

---

## 相关资源
- [官方仓库](https://github.com/Christian-Prather/zellij-load)
- [htop](https://htop.dev/) - 高级系统监控
- [btop](https://github.com/aristocratos/btop) - 现代系统监控
- [tmux cpu-usage](https://github.com/dracula/tmux)

---

*最后更新：2025年2月*
