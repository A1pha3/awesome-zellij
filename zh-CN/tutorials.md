# Zellij 教程资源

本文档收集 Zellij 相关的学习资源，包括官方教程、插件开发指南和社区资源。

---

## 目录

- [基础教程](#基础教程)
- [插件开发](#插件开发)
- [视频教程](#视频教程)
- [博客文章](#博客文章)
- [社区资源](#社区资源)

---

## 基础教程

### Zellij 基础功能

**来源**: [Zellij 官方网站](https://zellij.dev/tutorials/basic-functionality/)

**内容**：
- 创建和管理面板
- 标签页操作
- 基本快捷键
- 布局基础

**适合**：初次接触 Zellij 的用户

**难度**: ⭐ 入门

### 使用和创建布局

**来源**: [Zellij 官方网站](https://zellij.dev/tutorials/layouts/)

**内容**：
- 理解布局系统
- 使用预设布局
- 创建自定义布局
- 布局配置语法

**适合**：想要自定义工作环境的用户

**难度**: ⭐⭐ 初级

---

## 插件开发

### Zellij 插件开发全面指南

**来源**: [GitHub - Kangaxx-0/first-zellij-plugin](https://github.com/Kangaxx-0/first-zellij-plugin)

**内容**：
- 从零开始开发插件
- WebAssembly 基础
- Zellij API 介绍
- 完整示例项目

**适合**：想开发 Zellij 插件的开发者

**难度**: ⭐⭐⭐ 中级

**涵盖主题**：
1. 环境搭建
2. 第一个插件
3. 事件处理
4. 渲染 UI
5. 用户交互
6. 调试技巧

### 开发 Zellij 插件的经验教训

**来源**: [Nerd Blog](https://blog.nerd.rocks/posts/profiling-zellij-plugins/)

**作者**: zjstatus 创建者

**内容**：
- 插件性能优化
- 调试技巧
- 最佳实践
- 常见陷阱

**适合**：已有插件开发基础的开发者

**难度**: ⭐⭐⭐⭐ 高级

**重点内容**：
- 性能分析
- 内存管理
- 渲染优化
- WebAssembly 限制

---

## 视频教程

### 为 Zellij 开发 WebAssembly Rust 插件

**来源**: [YouTube - RustLab 2023](https://www.youtube.com/watch?v=pgNIcQ8rTXk)

**演讲者**: Aram Drevekenin (Zellij 创建者)

**内容**：
- Zellij 插件架构
- WebAssembly 在 Zellij 中的应用
- Rust 插件开发实践
- 实际演示

**时长**: ~45 分钟

**难度**: ⭐⭐⭐ 中级

**观看建议**：
- 先了解 Zellij 基础
- 有 Rust 基础更佳
- 准备好代码编辑器边看边练

---

## 博客文章

### 如何使用 Zellij Switch 插件

**来源**: [Mostafa Qanbaryan 博客](https://mostafaqanbaryan.com/zellij-attach-plugin/)

**内容**：
- zellij-switch 插件使用指南
- 结合 fzf 和 zoxide
- 实际使用场景
- 配置建议

**适合**：想使用 zellij-switch 的用户

**难度**: ⭐⭐ 初级

---

## 社区资源

### 官方文档

**网站**: [Zellij 文档](https://zellij.dev/documentation/introduction)

**包含**：
- 完整的功能参考
- 配置指南
- 键绑定文档
- 插件 API 文档
- 命令行选项

### 社区聊天

**Discord**: [Zellij Discord](https://discord.com/invite/CrUAFH3)

**Matrix**: [Matrix 聊天室](https://matrix.to/#/#zellij_general:matrix.org)

**用途**：
- 提问和求助
- 分享使用技巧
- 讨论新功能
- 获取支持
- 参与社区

### GitHub 讨论

**地址**: [Zellij Discussions](https://github.com/zellij-org/zellij/discussions)

**用途**：
- 功能讨论
- 问题反馈
- 分享配置
- 展示插件

---

## 学习路径建议

### 初学者路径

1. **观看**: Zellij 基础功能教程
2. **阅读**: 官方文档介绍部分
3. **实践**: 在日常工作中使用 Zellij
4. **探索**: 尝试不同的布局

### 进阶用户路径

1. **学习**: 布局系统深入
2. **配置**: 自定义键绑定
3. **探索**: 安装和配置插件
4. **优化**: 调整配置提高效率

### 开发者路径

1. **阅读**: 插件开发全面指南
2. **观看**: RustLab 演讲视频
3. **实践**: 开发简单插件
4. **深入**: 阅读博客中的经验教训
5. **贡献**: 为社区贡献插件

---

## 推荐配置学习

### 基础配置

**配置项目**：
- 键绑定自定义
- 默认布局设置
- 主题选择

**资源**：
- [键绑定文档](https://zellij.dev/documentation/keybindings.html)
- [布局文档](https://zellij.dev/documentation/layouts.html)
- [主题文档](https://zellij.dev/documentation/themes.html)

### 插件配置

**推荐插件组合**：

**导航类**：
- vim-zellij-navigator - Vim 导航
- zellij-autolock - 自动锁定

**会话管理**：
- zsm - 会话切换
- zellij-favs - 收藏管理

**状态栏**：
- zjstatus - 状态栏增强
- zjstatus-hints - 快捷键提示

---

## 常见问题解答

### 从哪里开始？

**推荐**：
1. 安装 Zellij
2. 完成基础功能教程
3. 阅读快捷键文档
4. 在日常使用中学习

### 如何学习插件开发？

**步骤**：
1. 了解 Rust 基础（如果不会）
2. 阅读插件开发指南
3. 克隆示例插件学习
4. 从简单插件开始
5. 参考现有开源插件

### 遇到问题怎么办？

**途径**：
1. 查阅官方文档
2. 搜索 GitHub Issues
3. 在 Discord/Matrix 提问
4. 查看社区讨论

---

## 贡献资源

如果你发现了好的教程资源，欢迎通过以下方式分享：

- 在 GitHub Discussions 分享
- 在 Discord/Matrix 推荐
- 提交 PR 到 awesome-zellij

---

## 相关链接

- [Zellij 官网](https://zellij.dev/)
- [GitHub 仓库](https://github.com/zellij-org/zellij)
- [插件目录](../plugins/) - 本仓库的中文插件文档
- [集成工具](./integrations.md) - 相关工具介绍

---

*最后更新：2025年2月*
