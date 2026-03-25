# OpenAI API 国内使用完整指南【2026 年最新】

> 国内开发者使用 OpenAI API（GPT-5、GPT-4o、o3 等）的完整解决方案 — 无需魔法，统一接口，按量计费

## 📌 你遇到的问题

- ❌ OpenAI 官网国内无法访问，注册需要海外手机号
- ❌ API 直连超时、被封 IP
- ❌ 需要绑定海外信用卡才能使用
- ❌ 不知道如何稳定、低成本地调用 GPT 系列模型

## 🚀 解决方案：通过 MaxRouter 中转

[MaxRouter](https://www.maxrouter.ai) 提供 OpenAI API 中转服务，完全兼容 OpenAI SDK，国内直连。

**核心优势：**
- ✅ 国内直连，无需代理
- ✅ 100% 兼容 OpenAI SDK，改一行 `base_url` 即可
- ✅ 按量计费，无月费，注册即送 ¥10 体验金
- ✅ 支持 GPT-5、GPT-4o、o3、o1 等全系列模型
- ✅ 同时支持 Claude、Gemini、DeepSeek 等 80+ 模型

## 🤖 支持的 OpenAI 模型

| 模型 | 说明 |
|------|------|
| `gpt-5` | OpenAI 最新旗舰模型 |
| `gpt-4o` | 多模态模型，支持文本/图像/音频 |
| `gpt-4o-mini` | 轻量版，性价比高 |
| `o3` | 推理增强模型 |
| `o3-mini` | 轻量推理模型 |
| `o1` | 初代推理模型 |

## 📋 快速开始

### 第一步：注册账号

访问 [www.maxrouter.ai](https://www.maxrouter.ai)，注册即送 ¥10 体验金。

### 第二步：获取 API Key

登录 [控制台 → 令牌管理](https://www.maxrouter.ai/console/tokens)，创建 API Key。

### 第三步：替换 base_url 开始使用

**Python：**

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-your-key",
    base_url="https://api.maxrouter.ai/v1"  # 只需改这一行
)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "你好，请介绍一下你自己"}]
)
print(response.choices[0].message.content)
```

**Node.js：**

```javascript
import OpenAI from 'openai';

const client = new OpenAI({
    apiKey: 'sk-your-key',
    baseURL: 'https://api.maxrouter.ai/v1'  // 只需改这一行
});

const response = await client.chat.completions.create({
    model: 'gpt-4o',
    messages: [{ role: 'user', content: '你好' }]
});
console.log(response.choices[0].message.content);
```

**cURL：**

```bash
curl https://api.maxrouter.ai/v1/chat/completions \
  -H "Authorization: Bearer sk-your-key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user", "content": "你好"}]
  }'
```

## 🔧 开发工具配置

### Cursor

- Override OpenAI Base URL: `https://api.maxrouter.ai/v1`
- API Key: `sk-your-key`

### ChatBox

- API Host: `https://api.maxrouter.ai`
- API Key: `sk-your-key`
- 模型: `gpt-4o`

### NextChat（ChatGPT Next Web）

- 接口地址: `https://api.maxrouter.ai`
- API Key: `sk-your-key`

## 📡 流式输出（Streaming）

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-your-key",
    base_url="https://api.maxrouter.ai/v1"
)

stream = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "写一首关于编程的诗"}],
    stream=True
)

for chunk in stream:
    content = chunk.choices[0].delta.content
    if content:
        print(content, end="", flush=True)
```

## ❓ 常见问题

### Q: 跟 OpenAI 官方有什么区别？
A: 功能完全一致，MaxRouter 只做请求转发。你的代码只需改 `base_url`，其余完全不变。

### Q: 支持 Function Calling / Tools 吗？
A: 支持。所有 OpenAI API 功能（Function Calling、JSON Mode、Vision、Streaming 等）都完整支持。

### Q: 支持图片输入吗？
A: 支持。GPT-4o 的多模态能力完整可用，可以发送图片进行分析。

### Q: 除了 OpenAI，还支持什么模型？
A: 同时支持 Claude（Anthropic）、Gemini（Google）、DeepSeek、通义千问、GLM 等 80+ 模型，全部通过统一的 OpenAI 兼容接口调用。

## 🔗 相关链接

- 🌐 官网：[www.maxrouter.ai](https://www.maxrouter.ai)
- 📖 完整文档：[www.maxrouter.ai/docs](https://www.maxrouter.ai/docs)
- 🤖 模型列表：[www.maxrouter.ai/models](https://www.maxrouter.ai/models)
- 💰 价格查询：[www.maxrouter.ai/pricing](https://www.maxrouter.ai/pricing)

---

**⭐ 觉得有用？给个 Star 支持一下！**
