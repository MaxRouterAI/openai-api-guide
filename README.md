# OpenAI Codex CLI 国内使用完整教程（2026 最新）

> Codex CLI 是 OpenAI 官方推出的终端 AI 编程工具，跟 Claude Code 类似但基于 GPT 系列模型。本文记录了国内开发者从安装到配置的完整过程。

---

## 什么是 Codex CLI

Codex CLI 是 OpenAI 在 2025 年推出的开源终端编程工具，用 Rust 编写，MIT 协议开源。它可以：

- 阅读你的代码库，理解项目结构
- 用自然语言描述需求，自动编写和修改代码
- 执行终端命令（编译、测试、部署）
- 通过 OS 级沙箱保证安全性
- 支持 GPT-5、o3 等 OpenAI 最新推理模型

跟 Claude Code 的定位类似，都是终端里的 AI 编程 Agent，区别在于底层模型不同。

## 安装 Codex CLI

### 系统要求

- macOS、Linux、或 Windows（WSL）
- Node.js 22+（推荐）

### 安装命令

```bash
npm install -g @openai/codex
```

安装完成后验证：

```bash
codex --version
```

> ⚠️ 国内 npm 可能比较慢，先换镜像：`npm config set registry https://registry.npmmirror.com`

## 官方使用方式

Codex CLI 需要 OpenAI API Key。官方的方式是：

1. 注册 OpenAI 账号（[platform.openai.com](https://platform.openai.com)）
2. 绑定信用卡，充值余额
3. 创建 API Key
4. 设置环境变量 `OPENAI_API_KEY`

```bash
export OPENAI_API_KEY=sk-your-openai-key
codex
```

## 国内的问题

跟 Claude Code 一样，国内开发者直接用官方服务会遇到障碍：

**网络问题：**
- `api.openai.com` 在国内无法直接访问
- 代理工具不稳定，编程过程中断连很影响体验

**账号问题：**
- OpenAI 注册需要海外手机号
- 需要绑定海外信用卡
- 部分地区 IP 被封，虚拟卡有封号风险

**成本问题：**
- GPT-5、o3 等模型价格不低
- 按量付费需要先解决支付问题

## 我的方案：API 中转

Codex CLI 支持通过 `OPENAI_BASE_URL` 环境变量自定义 API 地址。配合中转服务，国内直连就能用。

我用的是 [MaxRouter](https://www.maxrouter.ai)，支持所有 OpenAI 模型，与官方同价，按量计费。同一个 Key 还能调用 Claude、Gemini、DeepSeek 等 80+ 模型。

## 配置步骤

### 1. 注册 MaxRouter

访问 [www.maxrouter.ai](https://www.maxrouter.ai) 注册，送 ¥10 体验金。

进入控制台 → 密钥管理 → 创建密钥 → 复制 Key。

### 2. 设置环境变量

**macOS / Linux（Zsh）：**

```bash
nano ~/.zshrc

# 添加这两行
export OPENAI_API_KEY=sk-your-maxrouter-key
export OPENAI_BASE_URL=https://api.maxrouter.ai/v1

source ~/.zshrc
```

**macOS / Linux（Bash）：**

```bash
nano ~/.bashrc

export OPENAI_API_KEY=sk-your-maxrouter-key
export OPENAI_BASE_URL=https://api.maxrouter.ai/v1

source ~/.bashrc
```

**Windows PowerShell：**

```powershell
$env:OPENAI_API_KEY = "sk-your-maxrouter-key"
$env:OPENAI_BASE_URL = "https://api.maxrouter.ai/v1"
```

### 3. 启动 Codex CLI

```bash
codex
```

配置正确就能正常使用了。

## 支持的模型

| 模型 | 说明 |
|------|------|
| `gpt-5` | OpenAI 最新旗舰 |
| `o3` | 推理增强模型 |
| `o3-mini` | 轻量推理 |
| `gpt-4o` | 多模态，支持图像 |
| `gpt-4o-mini` | 性价比高 |
| `o1` | 初代推理模型 |

## Codex CLI 常用操作

```
> 帮我把这个项目从 CommonJS 迁移到 ESM

> 这个 API 接口响应太慢，帮我分析瓶颈并优化

> 写一个完整的用户认证模块，包含注册、登录、JWT 刷新

> 审查 src/ 下的代码，找出安全隐患
```

### 安全模式

Codex CLI 有三种安全模式：

- `suggest` — 只建议，不自动执行（最安全）
- `auto-edit` — 自动编辑文件，但执行命令需确认
- `full-auto` — 完全自动（在沙箱中运行）

```bash
codex --approval-mode full-auto "帮我重构这个模块"
```

### AGENTS.md

跟 Claude Code 的 `CLAUDE.md` 类似，Codex CLI 会读取项目根目录的 `AGENTS.md` 文件作为项目说明。

## 配合其他工具

同一个 MaxRouter Key 还能用在：

**Cursor：**
- Override OpenAI Base URL: `https://api.maxrouter.ai/v1`

**ChatBox / NextChat：**
- API Host: `https://api.maxrouter.ai`

**Python / Node.js 项目：**

```python
from openai import OpenAI
client = OpenAI(
    api_key="sk-your-key",
    base_url="https://api.maxrouter.ai/v1"
)
```

## 常见问题

**Q: 跟 Claude Code 比怎么样？**
各有优势。Claude Code 在代码理解和重构上更强，Codex CLI 的沙箱安全机制更完善。建议都试试，看哪个更适合你的场景。

**Q: 支持流式输出吗？**
支持。

**Q: 除了 GPT，能用 Claude 模型吗？**
MaxRouter 支持所有主流模型。但 Codex CLI 本身是为 OpenAI 模型设计的，用 Claude 模型建议用 Claude Code。

## 相关链接

- [Codex CLI GitHub](https://github.com/openai/codex)
- [OpenAI 官方文档](https://platform.openai.com/docs)
- [MaxRouter](https://www.maxrouter.ai) — 我用的 API 平台
- [MaxRouter 文档](https://www.maxrouter.ai/docs)
- [Claude Code 国内教程](https://github.com/MaxRouterAI/claude-code-guide)

---

觉得有用就给个 ⭐ Star，有问题欢迎提 Issue。
