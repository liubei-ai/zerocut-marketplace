<div align="center">

# zerocut-marketplace

**Zerocut AI 视频制作技能市场 · Claude Code Plugin Marketplace**

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](.claude-plugin/marketplace.json)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Plugin-orange.svg)](https://claude.ai/code)

</div>

## 🎬 关于本项目

`zerocut-marketplace` 是一个面向 [Claude Code](https://claude.ai/code) 的插件市场，封装了 **Zerocut AI** 平台的完整视频制作能力。通过安装 `zerocut-plugin`，你可以在 Claude Code 中直接调用一键成片、视频编辑、素材创作、文档生成等 AI 能力，无需手动拼接工具调用流程。

## ✨ 技能列表

安装后将获得以下 9 个技能（斜杠命令）：

| 技能 | 触发场景 |
|------|---------|
| `/一键成片` | 围绕主题快速生成完整视频，包含分镜、配音、音乐与合成 |
| `/素材创作` | 生成图片、视频、音频、MV 等单项素材 |
| `/编辑视频` | 替换视频主体、修改视频元素、拼接延长视频 |
| `/内容剪辑` | 使用 ffmpeg 进行拼接、剪切、字幕、水印等后期处理 |
| `/转为横屏视频` | 将任意视频转换为 16:9 横屏格式 |
| `/创作文档` | 基于生成素材，用 pandoc 导出 PPTX / DOCX / PDF |
| `/漫剧分镜师` | 为漫剧剧本生成分镜脚本 |
| `/内容生成失败分析` | 分析工具调用失败原因并提供解决方案 |
| `/core-rules` | 核心执行规则（自动应用，无需手动触发） |

## 🚀 快速开始

### 前置要求

- [Claude Code](https://claude.ai/code) 已安装
- 已配置 Zerocut MCP 工具（技能依赖 Zerocut 平台的 MCP Server）

### 安装方式一：通过 npx skills

```bash
npx skills add liubei-ai/zerocut-marketplace
```

### 安装方式二：通过 Claude Code Plugin 市场

```
# 在 Claude Code 中运行
/plugin marketplace add liubei-ai/zerocut-marketplace
/plugin install zerocut-plugin@zerocut-marketplace
```

### 安装方式三：本地安装

```bash
git clone https://github.com/liubei-ai/zerocut-marketplace.git
# 在 Claude Code 中运行
/plugin marketplace add ./zerocut-marketplace
/plugin install zerocut-plugin@zerocut-marketplace
```

## 📖 使用示例

安装完成后，在 Claude Code 中直接使用技能：

```
# 一键生成一支30秒的产品宣传视频
/一键成片 为我们的新款耳机生成一支30秒宣传片，风格科技感，竖屏

# 生成单张产品图
/素材创作 生成一张白色耳机的产品主图，纯白背景，高质感

# 将已有视频转为横屏
/转为横屏视频 [拖入视频文件]
```

## 📁 项目结构

```
zerocut-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # 市场配置入口
└── plugins/
    └── zerocut-plugin/
        ├── .claude-plugin/
        │   └── plugin.json       # 插件描述
        └── skills/               # 9 个技能目录
            ├── 一键成片/SKILL.md
            ├── 素材创作/SKILL.md
            ├── 编辑视频/SKILL.md
            └── ...
```

## 🤝 贡献

欢迎提交 Issue 或 Pull Request 来改进技能提示词、新增技能或修复问题。

## 📄 许可证

[MIT](LICENSE) © Liubei AI
