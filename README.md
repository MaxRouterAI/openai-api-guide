# Codex CLI 完整教程：从安装到实战（国内开发者指南）

> Codex CLI 是 OpenAI 官方推出的终端 AI 编程工具，用 Rust 编写，开源免费。跟 Claude Code 定位类似，但基于 GPT 和 o 系列推理模型。这篇文章记录了我在国内从安装到日常使用的完整过程。

---

## 什么是 Codex CLI

Codex CLI 是 OpenAI 在 2025 年 4 月推出的命令行编程工具，MIT 协议开源，GitHub 上已有大量关注。它的核心能力：

- 阅读和理解你的整个代码库
- 用自然语言描述需求，自动编写和修改代码
- 执行终端命令（编译、测试、部署）
- 内置 OS 级沙箱，安全隔离执行环境
- 支持 GPT-5、o3、o3-mini 等 OpenAI 最新模型
- 三种安全模式：suggest（只建议）、auto-edit（自动改文件）、full-auto（完全自动）

跟 Claude Code 相比，Codex CLI 最大的特点是安全沙箱机制 - 它在隔离环境中执行命令，即使 full-auto 模式也不会影响你的系统。

## 安装 Codex CLI

### 系统要求

- macOS 12+、Ubuntu 22.04+ / Debian 11+、或 Windows（WSL2）
- Node.js 22+

### 安装命令

```bash
npm install -g @openai/codex
```

验证安装：

```bash
codex --version
```

> npm 慢的话先换镜像：`npm config set registry https://registry.npmmirror.com`

## 官方使用方式

Codex CLI 需要 OpenAI API Key。官方流程：

1. 注册 OpenAI 账号（[platform.openai.com](https://platform.openai.com)）
2. 绑定信用卡，充值余额
3. 创建 API Key
4. 设置环境变量启动

```bash
export OPENAI_API_KEY=sk-your-openai-key
codex
```

按 token 用量计费，没有月费。

## 国内使用的现实问题

跟 Claude Code 一样，国内开发者直接用官方服务会遇到不少麻烦：

**网络层面：**
- `api.openai.com` 在国内无法直接访问
- 代理工具不稳定，编程过程中断连影响很大
- 流式输出对网络质量要求高，代理经常丢包

**账号层面：**
- OpenAI 注册需要海外手机号
- 需要绑定海外信用卡充值
- 虚拟卡有封号风险，OpenAI 的风控也在收紧
- 封号后余额不退

**成本层面：**
- GPT-5、o3 等新模型价格不低
- 需要先解决支付问题才能按量付费

这些问题跟 Claude Code 遇到的几乎一样，详细分析可以看我写的 [Claude Code 国内使用教程](https://github.com/MaxRouterAI/claude-code-guide#国内使用的现实问题)。

## 我的方案：API 中转

Codex CLI 支持通过 `OPENAI_BASE_URL` 环境变量自定义 API 地址。原理跟 Claude Code 的 `ANTHROPIC_BASE_URL` 一样：

```
终端 Codex CLI → 中转服务（国内直连） → OpenAI 官方 API
```

我用的是 [MaxRouter](https://www.maxrouter.ai)，支持所有 OpenAI 模型，与官方同价，按量计费。之前用 Claude Code 时就在用这个平台，同一个 Key 可以调用所有模型，不用分别注册不同厂商的账号。

## 配置步骤

### 1. 注册并获取 API Key

如果你之前跟着 [Claude Code 教程](https://github.com/MaxRouterAI/claude-code-guide) 已经注册过 MaxRouter，直接用同一个 Key 就行，不用重新注册。

没注册的话，访问 [www.maxrouter.ai](https://www.maxrouter.ai) 注册（送 ¥10 体验金），进入控制台 → 密钥管理 → 创建密钥。

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
# 临时生效
$env:OPENAI_API_KEY = "sk-your-maxrouter-key"
$env:OPENAI_BASE_URL = "https://api.maxrouter.ai/v1"

# 永久生效
[System.Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "sk-your-maxrouter-key", "User")
[System.Environment]::SetEnvironmentVariable("OPENAI_BASE_URL", "https://api.maxrouter.ai/v1", "User")
```

> 注意：跟 Claude Code 不同，Codex CLI 的 `OPENAI_BASE_URL` **需要带 `/v1`**。

### 3. 启动

```bash
codex
```

配置正确就能正常使用了。

## 支持的模型

| 模型 | 说明 | 适用场景 |
|------|------|----------|
| `o3` | OpenAI 最强推理模型 | 复杂逻辑、数学推理、架构设计 |
| `o3-mini` | 轻量推理模型 | 日常编程，性价比高 |
| `gpt-5` | OpenAI 最新旗舰 | 通用任务，能力全面 |
| `gpt-4o` | 多模态模型 | 支持图像输入，UI 分析 |
| `gpt-4o-mini` | 轻量多模态 | 简单任务，速度快 |

Codex CLI 默认使用 o3-mini，可以通过参数切换：

```bash
codex --model o3 "帮我重构这个模块"
codex --model gpt-5 "分析这个项目的架构"
```

## 三种安全模式

Codex CLI 最大的特色是内置安全沙箱，提供三种操作模式：

### suggest 模式（默认）

只给建议，不自动执行任何操作。最安全，适合刚开始用的时候：

```bash
codex --approval-mode suggest "优化这个函数的性能"
```

### auto-edit 模式

自动编辑文件，但执行终端命令前会询问你确认：

```bash
codex --approval-mode auto-edit "给所有 API 加上错误处理"
```

### full-auto 模式

完全自动执行，包括终端命令。所有操作在沙箱中运行，不会影响系统：

```bash
codex --approval-mode full-auto "创建一个完整的 REST API 项目"
```

> 建议从 suggest 开始，熟悉后再切到 auto-edit 或 full-auto。

## 日常使用

### 常用命令

直接在终端用自然语言描述需求：

```bash
# 交互模式
codex

# 单次任务模式
codex "帮我写一个 Redis 缓存工具类"
codex "这个测试为什么失败了，帮我修复"
codex "把这个 Python 脚本改写成 Go"
```

### 实际场景

**创建完整项目：**

```bash
codex --approval-mode full-auto "创建一个 Express + TypeScript 项目，包含 JWT 认证、SQLite 数据库、Zod 验证和单元测试"
```

**修 bug：**

```bash
codex "用户反馈上传大文件时内存溢出，帮我分析原因并修复"
```

**代码审查：**

```bash
codex "审查 src/ 下的所有文件，找出潜在的安全问题"
```

**重构：**

```bash
codex "把这个项目的状态管理从 Redux 迁移到 Zustand"
```

**Git 操作：**

```bash
codex "看看最近的改动，帮我写一个清晰的 commit message 并提交"
```

## 进阶技巧

### AGENTS.md 文件

跟 Claude Code 的 `CLAUDE.md` 类似，Codex CLI 会读取项目根目录的 `AGENTS.md` 文件。在里面写上项目背景、编码规范、常用命令等信息：

```markdown
# AGENTS.md

## 项目简介
后台管理系统，Next.js 14 + TypeScript + Prisma + PostgreSQL

## 编码规范
- 使用 Server Components，客户端组件加 'use client'
- 数据库操作统一用 Prisma
- API 路由放 app/api/

## 常用命令
- npm run dev — 开发服务器
- npm run test — 跑测试
- npx prisma migrate dev — 数据库迁移
```

### 跟 Claude Code 的对比

两个工具我都在用，各有优势：

| 特性 | Codex CLI | Claude Code |
|------|-----------|-------------|
| 底层模型 | GPT-5 / o3 | Claude Opus 4 / Sonnet 4 |
| 开源 | ✅ MIT | ❌ |
| 安全沙箱 | ✅ OS 级隔离 | 有限 |
| 代码理解 | 强 | 最强 |
| 推理能力 | o3 很强 | Opus 4 很强 |
| 上下文 | 128K | 200K |

我的经验：日常编程两个都好用，复杂重构和代码理解 Claude Code 略胜一筹，需要安全隔离执行的场景 Codex CLI 更放心。

## 配合其他工具

同一个 MaxRouter Key 还能用在其他 AI 编程工具上：

**Cursor：**
- Settings → Models → Override OpenAI Base URL: `https://api.maxrouter.ai/v1`

**Cline（VS Code）：**
- API Provider → OpenAI Compatible → Base URL: `https://api.maxrouter.ai/v1`

**Python 项目集成：**

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-your-key",
    base_url="https://api.maxrouter.ai/v1"
)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "用 Python 写一个快速排序"}]
)
print(response.choices[0].message.content)
```

**Node.js 项目集成：**

```javascript
import OpenAI from 'openai';

const client = new OpenAI({
    apiKey: 'sk-your-key',
    baseURL: 'https://api.maxrouter.ai/v1'
});

const response = await client.chat.completions.create({
    model: 'o3-mini',
    messages: [{ role: 'user', content: '优化这段代码的性能' }]
});
console.log(response.choices[0].message.content);
```

## 常见问题

**Q: 跟 ChatGPT 网页版有什么区别？**
Codex CLI 直接在你的终端里运行，能读取和修改你的本地文件，执行命令。ChatGPT 网页版只能对话，不能操作你的代码库。

**Q: 报错连接超时？**
配置了 `OPENAI_BASE_URL` 指向 MaxRouter 就不会有这个问题，国内直连。

**Q: full-auto 模式安全吗？**
安全。Codex CLI 在 OS 级沙箱中执行命令，网络访问默认被禁止，文件操作限制在项目目录内。

**Q: 支持流式输出吗？**
支持，体验跟官方一样。

**Q: 数据安全？**
MaxRouter 不存储对话内容和代码，请求直接转发到官方 API。

**Q: 除了 GPT，能用 Claude 模型吗？**
MaxRouter 支持所有主流模型。但 Codex CLI 是为 OpenAI 模型优化的，用 Claude 建议用 [Claude Code](https://github.com/MaxRouterAI/claude-code-guide)。

## 相关链接

- [Codex CLI GitHub](https://github.com/openai/codex)
- [OpenAI 官方文档](https://platform.openai.com/docs)
- [MaxRouter](https://www.maxrouter.ai) — 我用的 API 平台
- [MaxRouter 文档](https://www.maxrouter.ai/docs)
- [Claude Code 国内教程](https://github.com/MaxRouterAI/claude-code-guide)
- [Gemini CLI 国内教程](https://github.com/MaxRouterAI/gemini-api-guide)

---

如果这篇教程帮到了你，给个 ⭐ Star 让更多人看到。有问题欢迎提 Issue。
