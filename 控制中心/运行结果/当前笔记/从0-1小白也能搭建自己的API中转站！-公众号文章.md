文件写入权限正在等待你批准。我先在聊天中直接展示改写后的文章：

---

# 你的 API Key 正在被同事白嫖，而你连谁在薅都不知道

这是每个做 AI 应用的人都绕不开的场景。

你接 GPT，要读 OpenAI 的文档。接 Claude，又要读 Anthropic 的。接 DeepSeek、Gemini、千问，每家请求格式都不一样，密钥散落在各处，管理一塌糊涂。更糟的是，团队五个人共用一张卡，月底账单到了你才发现有人拿你的 Key 去跑批量翻译，几千块额度烧得干干净净——而你甚至查不出是谁。

这不是技术问题，这是架构问题。

NewAPI 把这件事变成了过去式。一个接口，一个密钥，统一管理所有上游模型。

---

## NewAPI 到底是什么

先说清楚一个误解：NewAPI 不是模型。它不提供 AI 能力，它是一个 **AI API 网关**。

你可以把它理解为三层架构中的"中间层"：

- **上游模型**（GPT、Claude、DeepSeek 等）是"供应商"——你把它们的 API Key 配置进去
- **NewAPI** 是"中间层"——统一接收请求，按规则转发到对应模型
- **下游用户** 是你的应用或团队成员——他们只用知道你的域名和 Token，不用关心背后接了哪些模型

它承担了四个核心职责：

- **模型路由**：一个请求入口，自动分发到不同的上游模型
- **Key 管理**：你只维护一个主 Key，然后给每个用户发子 Key
- **额度分配**：按用户、按项目设置 Token 配额，谁用超了立刻掐断
- **权限控制**：谁可以用哪个模型、能用多少，精确到每个子 Key

一句话：你拥有了一个自己的 OpenAI 兼容 API 服务，背后可以是任何模型。

---

## 谁真正需要它

三种人最适合自己搭 NewAPI：

- **独立开发者 / 小团队**：几个人用 ChatGPT 和 Claude，不想把 Key 贴在群里传来传去
- **AI 产品 Maker**：你的产品要调多个模型，不想在每个 SDK 之间来回写适配层
- **API 转售商**：你有上游渠道，想包装成服务对外出售

如果你只是一个人偶尔用用 ChatGPT 网页版，这个方案对你来说是杀鸡用牛刀。但如果你在认真做 AI 产品——继续读下去。

---

## 从零到上线，八步部署指南

以下步骤基于 Ubuntu 22.04，需要一台带公网 IP 的 Linux 服务器。最低配置 1 核 2G 即可跑起来。

### 第一步：准备服务器环境

```bash
apt update && apt install -y git curl nginx certbot python3-certbot-nginx
```

这些基础工具分别负责：拉代码（git）、装 Docker（curl）、反向代理（nginx）、自动 HTTPS（certbot）。

### 第二步：安装 Docker

```bash
curl -fsSL https://get.docker.com | sh
systemctl start docker
systemctl enable docker
docker --version  # 确认安装成功
```

NewAPI 以 Docker 容器运行，所以 Docker 是必备依赖。`systemctl enable docker` 确保服务器重启后 Docker 自动启动。

### 第三步：拉取代码

```bash
git clone https://github.com/QuantumNous/new-api.git
cd new-api
```

### 第四步：配置环境变量

复制示例配置并打开编辑：

```bash
cp .env.example .env
nano .env
```

核心字段如下：

```bash
# 数据库连接（推荐 MySQL）
SQL_DSN="root:你的强密码@tcp(localhost:3306)/newapi"

# Redis 连接
REDIS_CONN_STRING="redis://localhost:6379"

# 流式响应超时（秒）
STREAMING_TIMEOUT=300

# Session 加密密钥
SESSION_SECRET="生成的随机字符串"

# Redis 加密密钥
CRYPTO_SECRET="生成的随机字符串"
```

生成随机密钥用 `openssl rand -hex 32`，运行两次，分别填入 `SESSION_SECRET` 和 `CRYPTO_SECRET`。别偷懒用同一个值。

### 第五步：启动服务

```bash
docker compose up -d
```

等待镜像拉取完成，大约 5-10 分钟。完成后执行 `docker compose ps`，看到 `new-api` 状态为 `running` 即成功。

### 第六步：配置 Nginx 反向代理

新建配置文件 `/etc/nginx/sites-available/new-api`，写入：

```nginx
server {
    listen 80;
    server_name 你的域名.com;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

启用并重载：

```bash
ln -s /etc/nginx/sites-available/new-api /etc/nginx/sites-enabled/
nginx -t
systemctl restart nginx
```

### 第七步：开启 HTTPS

```bash
certbot --nginx -d 你的域名.com
```

Certbot 会自动申请 SSL 证书并配置 80 跳转 443，全程交互式确认即可。

浏览器访问 `https://你的域名.com`，看到 NewAPI 登录页即部署成功。

### 第八步：初始化设置

首次访问会进入初始化页面，按提示设置管理员账号。这是你后台的大门——密码设强一点。

---

## 后台操作：渠道、令牌、测试

部署完成只是第一步。接下来要让这个网关真正"接入模型、分发 Token"。

### 添加渠道

渠道 = 上游模型供应商。登录后台，进入「渠道」→「添加渠道」，填入对应 API Key。

**建议至少添加 2-3 个不同供应商的渠道**。一个挂了，请求可以自动切换到备选——这是用自己的 Key 直连做不到的。

添加后点击「测试」，确认能正常调用再继续。

### 分发 Token

进入「令牌」→「添加令牌」，设置用户名、额度上限、可用模型，提交后复制生成的 Key 给用户。

用户接入你的 API，代码和用 OpenAI 完全一样：

```python
import openai

client = openai.OpenAI(
    api_key="你的用户令牌",
    base_url="https://你的域名.com/v1"
)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Hello"}]
)
```

对用户来说，他感知不到背后到底是 GPT、Claude 还是 DeepSeek——他用的就是"OpenAI 格式"的接口。模型的切换和路由是你的事。

### 测试全链路

用 curl 做一次端到端验证：

```bash
curl https://你的域名.com/v1/chat/completions \
  -H "Authorization: Bearer 用户令牌" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

返回正常 JSON = 全流程跑通。

---

## 如果想靠它赚钱

技术底座搭好了，但技术不等于生意。

NewAPI 可以做商业化的底层，但对外服务前，你需要逐项确认：

- **上游条款**：API 供应商是否允许转售？（很多官方 API 禁止）
- **支付方式**：怎么收钱？（Stripe、支付宝、加密货币）
- **内容安全**：有没有敏感词过滤、内容审核机制？
- **日志审计**：请求日志保留多久？出事了能不能溯源？
- **实名认证**：用户身份如何验证？
- **税务合规**：收入怎么报税？当地法规是怎样的？

**不要只卖 API。要包装成服务。**

卖"GPT-4 访问权限"是价格战，没有任何壁垒。但卖"行业写作工具""电商客服机器人""法律文档分析平台"——这才有定价权。技术底座只是起点，溢价来自你对场景的理解。

---

## 下一步

读完这篇文章之后，你可以立刻做三件事：

1. **先判断需求**：你是自己用、给团队用、还是想商业化？不同的目标决定了不同的配置策略和投入程度
2. **买一台廉价 VPS 走一遍部署流程**：Vultr、DigitalOcean、阿里云轻量服务器，最低配一个月不到 30 块，走通全流程就是最大的收获
3. **接入两个模型做对比测试**：一个 GPT-4o，一个 Claude Sonnet，通过 NewAPI 用同一段代码分别调一次——你会立刻理解"网关"的价值所在

2026 年，会搭 NewAPI 的人，比只会调 API 的人，多一条护城河。

---

改写要点说明：

- **开头**：用"Key 被白嫖、账单烧光查不到人"制造冲突，替代原文的平铺直叙
- **结构**：五个二级标题，逻辑线是"是什么 → 谁需要 → 怎么搭 → 怎么用 → 怎么变现"
- **补充**：每一步都加了"为什么"的解释，不只有命令；新增了"下一步"的三个可执行动作
- **节奏**：技术步骤紧凑，商业部分留白，符合公众号"干货+思考"的阅读预期

如果你批准写入权限，我会把这篇保存到 `Library/raw/从0-1小白也能搭建自己的API中转站-公众号版.md`。也可以按你的要求调整标题、语气或增删章节。
