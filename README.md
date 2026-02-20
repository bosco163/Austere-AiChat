<h1 align="center">Austere AiChat</h1>
<p align="center"><em>极简单文件 · 极致爽体验</em></p>

---

## 简介

**Austere AiChat** 是一款设计理念为 **简洁但不简单** 的网页AI聊天应用。整个系统仅由一个 PHP 文件构成，无需数据库，无需复杂配置，开箱即用。尽管代码极度精简，却提供了媲美商业产品的完整功能：多模型支持、实时流式对话、图片/文件分析、历史会话管理、全文搜索、数学公式渲染、Mermaid图表等。

> "Austere" 意为简朴、严谨，这正是本项目的哲学：去除冗余，保留精华。

### 🧠 背后的故事

这个项目是**人类与AI激烈碰撞**的结晶。经过两天爆肝式的开发，期间与 AI 进行了无数次"争吵"与博弈，最终打磨出了这个版本。

> **致老用户**：此前的版本因 AI 生成代码质量不稳定，越改 Bug 越多，导致项目长时间停滞。本次 v3.0 彻底重构，采用更严格的代码审查策略，终于达到了可发布标准。

**特别致谢参与的 AI 模型**：
- **GLM-5**（主力开发，承担大部分代码生成与 Debug）
- **GPT-5.3-codex**（架构顾问，生成大体网页）

## 核心特性

### 🚀 极简部署
- **单文件架构**：仅需一个 `index.php`，无需 MySQL/Redis
- **零依赖运行**：原生 PHP + 浏览器原生 API，无需 Node 构建
- **自动初始化**：自动创建存储目录，开箱即用

### 🤖 强大的AI能力
- **多模型支持**：支持多个模型自由切换
- **多模态对话**：支持图片上传分析（自动Base64编码）和文档附件
- **实时流式输出**：SSE 技术实现打字机效果，支持 **深度思考过程**展示（Reasoning Content）
- **智能标题**：AI 自动生成会话标题，支持锁定标题

### 💾 完善的会话管理
- **历史记录**：本地 JSON 文件存储，支持置顶、拖拽排序
- **批量操作**：支持批量删除、批量置顶/取消置顶
- **会话克隆**：一键复制现有会话
- **全文搜索**：支持按标题和内容搜索历史记录，高亮显示命中片段
- **消息总览**：快速浏览所有消息，支持跳转定位

### 🎨 优雅的交互体验
- **Markdown 全渲染**：代码高亮、表格、列表完美呈现
- **数学公式**：MathJax 支持 LaTeX 公式（`$...$` 和 `$$...$$`）
- **Mermaid 图表**：流程图、时序图、甘特图实时渲染
- **响应式设计**：完美适配桌面端和移动端
- **消息编辑**：双击消息即可编辑，支持重新生成

## 安装部署

### 环境要求
- PHP >= 8.0（启用 `curl` 扩展）
- 现代浏览器（Chrome/Firefox/Safari/Edge）

### 快速开始

1. **下载文件**（推荐从 GitHub 获取最新版）
   ```bash
   # 克隆仓库
   git clone https://github.com/bosco163/Austere-AiChat.git
   
   # 或下载单文件（Austere-3.0 为当前最新版本）
   curl -O https://raw.githubusercontent.com/bosco163/Austere-AiChat/main/Austere-3.0 -o index.php
   ```

2. **配置API密钥**
   编辑文件顶部的配置常量：
   ```php
   const OPENAI_API_KEY = 'your-api-key-here';
   const OPENAI_BASE_URL = 'https://api.openai.com/v1'; // 或其他兼容端点
   ```

3. **设置权限**（Linux/Mac）
   ```bash
   chmod 755 .
   # 程序会自动创建 history/ 和 uploads/ 目录
   ```

4. **访问使用**
   浏览器访问 `http://your-server/index.php`

## 使用指南

### 基础对话
- 在输入框输入文字，按 `Enter` 发送（`Shift+Enter` 换行）
- 支持拖拽或点击上传图片（自动识别分析）和文档（最多6个/轮，15MB限制）

### 快捷键与操作
| 操作 | 说明 |
|------|------|
| 双击会话标题 | 重命名会话 |
| 双击历史列表项 | 快速重命名 |
| 拖拽历史会话 | 自定义排序 |
| 点击"思考过程" | 查看AI推理详情（支持编辑） |
| 搜索框 | 实时搜索标题和消息内容 |

### 移动端适配
- 点击左上角 `☰` 展开侧边栏
- 底部输入框自动适配软键盘高度

## 技术架构

```
┌─────────────────────────────────────┐
│  Frontend (Vanilla JS)              │
│  - SSE 流式处理                     │
│  - marked.js 解析 Markdown          │
│  - MathJax 渲染公式                 │
│  - Mermaid 绘制图表                 │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│  Backend (PHP 8+)                   │
│  - 路由分发 (action 参数)            │
│  - 文件上传处理（安全校验）           │
│  - OpenAI 兼容 API 代理              │
│  - JSON 文件持久化                   │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│  Storage                            │
│  - history/*.json (会话数据)        │
│  - uploads/* (附件存储)             │
└─────────────────────────────────────┘
```

## 配置说明

在文件顶部修改以下常量：

```php
// API 配置
const OPENAI_API_KEY = 'sk-...';        // 你的 API 密钥
const OPENAI_BASE_URL = 'https://...';   // API 基础 URL

// 模型配置
const MODEL_CONFIG = [
    'chat_default' => 'kimi-k2-0905',
    'title_model'  => 'kimi-k2-0905',    // 用于生成标题的模型
    'supported'    => ['kimi-k2-0905', 'kimi-k2.5', 'qwen3.5-397b-a17b', ...]
];

// 功能限制
const MAX_ATTACHMENTS_PER_TURN = 6;      // 每轮最大附件数
const MAX_IMAGE_BYTES = 4000000;         // 图片大小限制（4MB）
const MAX_UPLOAD_BYTES = 15728640;       // 文件大小限制（15MB）
const MAX_CONTEXT_MESSAGES = 0;          // 上下文限制（0=无限制）
```

## 安全提示

⚠️ **生产环境部署建议**：
1. 修改默认的 API Key
2. 为 `history/` 和 `uploads/` 目录设置适当的访问权限（禁止直接执行 PHP）
3. 建议通过 HTTPS 访问，防止 API Key 泄露
4. 文件上传已做 MIME 校验和重命名处理，但仍建议定期清理 `uploads/` 目录

## 浏览器兼容性

- ✅ Chrome/Edge 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ iOS Safari / Android Chrome

## 许可证

MIT License - 自由使用，保留署名。

---

## 作者与致谢

**作者**：BoscoLi  
**个人主页**：[lhyb.dpdns.org](https://lhyb.dpdns.org)  
**项目仓库**：[github.com/bosco163/Austere-AiChat](https://github.com/bosco163/Austere-AiChat)

**致谢**：
- 感谢 **GLM-5** 在开发过程中承担的主力编码工作
- 感谢 **GPT-5.3-codex** 在架构设计上的指导
- 感谢所有在"激烈争吵"中坚持下来的代码行

<p align="center">
  <sub>Built with ❤️, ☕️, and 🤖 arguments | Single File Philosophy</sub>
</p>
