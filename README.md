<h1 align="center">Austere AiChat</h1>
<p align="center"><em>单文件架构 · 极致简体验</em></p>

---

## 简介

**Austere AiChat** 是一款极简但功能完整的网页 AI 聊天应用。仅一个 PHP 文件（<100KB），无需数据库，开箱即用。支持多模型切换、流式对话、图片/文件分析、历史会话管理、Markdown 全渲染（含数学公式与 Mermaid 图表）。

> **v3.0+ 说明**：彻底重构版本，修复历史遗留问题，代码质量大幅提升。

<details>
<summary><b>📦 v3.2.4 更新内容（点击展开）</b></summary>

- 修复总览搜索高亮问题
- 修复暂停回复不成功并且回复丢失的问题
- 修复输出途中局部窗口滚动会被重置的问题（表格，LateX，details标签...）
- 新增总览消息渲染，添加复制按钮
- 网页风格改成黑白纯色风格
- Ai消息去掉框
- 新增“[<|>]”按钮：宽屏
- 去掉 上下＝左右 的局部窗口鼠标滚动操控逻辑
</details>

<details>
<summary><b>📦 v3.2.3 更新内容（点击展开）</b></summary>

- 修改字串溢出处理逻辑（窗口滚动 → 隔行）

</details>

<details>
<summary><b>📦 v3.2.2 更新内容（点击展开）</b></summary>

- 新增窗口全屏按钮（总览/编辑/思考过程 窗口）
- 修正 `normalizeLatexText()` 中的正则错误
- 修改聊天记录栏双击标题处理逻辑（编辑 → 重新生成）
- 新增拖拽文件到输入框直接上传功能
- 发送请求 GET → POST（消息长度无限制）
- 修复消息渲染溢出问题（溢出部分出现左右滚动窗口，支持鼠标滚动操控：上下＝左右）
- 思考过程窗口在 AI 思考途中也支持编辑

</details>

---

## 快速开始

### 环境要求
- PHP >= 8.0（启用 `curl` 扩展）
- 现代浏览器（Chrome/Firefox/Safari/Edge）

### 部署方式（压缩包）

```bash
# 1. 上传 Austere-3.2.1.zip 到服务器并解压
unzip Austere-3.2.1.zip -d /var/www/austere/
cd /var/www/austere/

# 2. 配置 API 密钥（编辑 index.php 顶部）
# const OPENAI_API_KEY = 'sk-your-key-here';
# const OPENAI_BASE_URL = 'https://api.openai.com/v1';

# 3. 设置目录权限（程序会自动创建 history/ 和 uploads/）
chmod 755 .

# 4. 浏览器访问
# http://your-domain/index.php
```

---

## 版本升级指南

**核心原则**：单文件替换，数据自动保留。历史记录存储在 `history/*.json`，附件存储在 `uploads/`，**升级时只需替换 `index.php`，保留这两个目录即可**。

### 标准更新流程

```bash
# 进入项目目录
cd /var/www/austere/

# 1. 备份当前版本（保险起见）
cp index.php "index.php.backup.$(date +%Y%m%d)"

# 2. 解压新版本（直接覆盖 index.php）
unzip -o Austere-3.2.1.zip

# 3. 确保数据目录权限正常
chmod 755 history uploads 2>/dev/null || true

# 完成！刷新浏览器即可
```

### 跨版本升级注意事项

| 升级路径 | 特别提示 |
|---------|---------|
| **3.2 → 3.2.1** | 直接覆盖，完全兼容 |
| **3.1 → 3.2.x** | 建议检查配置中的 `MAX_CONTEXT_MESSAGES`（3.2+ 优化了上下文逻辑） |
| **3.0 → 3.2.x** | 直接覆盖，JSON 格式兼容 |
| **1.0 → 3.2.x** | **大版本跨越**，建议：<br>1. 完整备份旧目录<br>2. 测试历史文件兼容性（如无法读取，需手动迁移或重新开始）<br>3. 重新配置 API Key（配置格式已变更） |

> **推荐**：跨大版本升级时，建议先解压到新目录测试，确认无误后再迁移旧数据。

---

## 核心特性

- **🚀 极简部署**：单 PHP 文件，零依赖，无需 Node 构建
- **🤖 多模型支持**：支持多模型切换，思考模型显示推理过程（Reasoning Content）
- **📎 多模态对话**：支持图片/文档分析，**拖拽上传**，自动 Base64 编码
- **⚡ 实时流式**：SSE 打字机效果，支持深度思考过程展示
- **💾 会话管理**：本地 JSON 存储，置顶、拖拽排序、批量操作、**全文搜索**
- **🎨 丰富渲染**：Markdown、代码高亮、MathJax 公式、Mermaid 图表
- **📋 消息总览**：快速浏览所有消息，支持全屏查看、关键词搜索、跳转定位
- **✏️ 灵活编辑**：消息支持编辑、重新生成，思考过程支持实时编辑

---

## 使用指南

| 操作 | 说明 |
|------|------|
| **发送消息** | `Enter` 发送，`Shift+Enter` 换行 |
| **双击会话标题** | **重新生成**该会话最后一条消息（v3.2.1 变更） |
| **拖拽上传** | 直接拖文件到输入框（支持多文件，最多 6 个/轮） |
| **思考过程** | 点击"思考"按钮查看/编辑推理详情（思考途中也可编辑） |
| **全屏查看** | 总览/编辑/思考窗口支持全屏按钮（v3.2.1 新增） |
| **溢出滚动** | 超长代码块左右溢出时，可用鼠标滚轮左右滚动（v3.2.1 优化） |
| **移动端** | 点击左上角 `☰` 展开侧边栏，自适应软键盘 |

---

## 配置说明

编辑 `index.php` 顶部常量：

```php
// API 配置（必填）
const OPENAI_API_KEY = 'sk-your-api-key-here';
const OPENAI_BASE_URL = 'https://api.openai.com/v1';  // 或其他兼容端点

// 模型配置
const MODEL_CONFIG = [
    'chat_default' => 'kimi-k2-0905',
    'title_model'  => 'kimi-k2-0905',    // 用于生成标题的模型
    'supported'    => ['kimi-k2-0905', 'kimi-k2.5', 'qwen3.5-397b-a17b']
];

// 功能限制
const MAX_ATTACHMENTS_PER_TURN = 6;   // 每轮最大附件数
const MAX_IMAGE_BYTES = 4000000;      // 图片大小限制（4MB）
const MAX_UPLOAD_BYTES = 15728640;    // 文件大小限制（15MB）
const MAX_CONTEXT_MESSAGES = 0;       // 上下文限制（0=无限制）
```

---

## 安全提示

⚠️ **生产环境建议**：
1. **修改默认 API Key**，使用 HTTPS 访问防止密钥泄露
2. 设置目录权限：`history/` 和 `uploads/` 禁止执行 PHP（可通过 `.htaccess` 或 Nginx 配置）
3. 定期清理 `uploads/` 目录，防止磁盘占满
4. 文件上传已做 MIME 校验和重命名，但仍建议限制上传大小

---

## 致谢

**作者**：BoscoLi（[lhyb.dpdns.org](https://lhyb.dpdns.org)）
**项目仓库**：[github.com/bosco163/Austere-AiChat](https://github.com/bosco163/Austere-AiChat)

**特别致谢参与的 AI 模型**：
- **GPT-5.4 / GPT-5.3-Codex / GLM-5 / Kimi-K2.5**：主力开发 与 架构顾问 与 Debug

欢迎提建议，持续优化中 ⭐️

---

<p align="center">
  <sub>Built with ❤️ and 🤖 | MIT License | Single File Philosophy</sub>
</p>
