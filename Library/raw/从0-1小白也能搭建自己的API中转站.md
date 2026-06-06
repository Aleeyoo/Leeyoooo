---
status: processed
wiki: "[[WIKI/自建AI API中转站]]"
---

# X 上的 0x小师妹：”从0-1小白也能搭建自己的API中转站！” / X
https://x.com/0xshimei/status/2057677659178713273?s=20

做 AI 应用最烦的不是模型，是接口。

你接 GPT，要读 OpenAI 的文档。接 Claude，又要读 Anthropic 的。DeepSeek、Gemini、千问，每家格式都不一样，密钥管理一团糟。

团队里 5 个人共用一张卡，谁用多了谁用少了完全不知道。月底账单来了，才发现有人拿你的 Key 去跑批量翻译，额度烧光。

NewAPI 就是来解决这些问题的。

**它解决什么问题**

![图像](https://pbs.twimg.com/media/HI1ZnTsbQAAoUVX?format=png&name=large)

NewAPI 把这些事打包成一个后台，统一管理。

**NewAPI 是什么**

它不是模型本身，是 **AI API 网关**。

你可以把它理解成：

- **模型路由器**：一个入口，接多个上游模型
- **Key 管理器**：一个主 Key 管所有，给用户发子 Key
- **额度分配器**：按用户、按项目分配 Token 额度
- **权限控制器**：谁可以用什么模型，用多少，完全可控

上游模型（GPT、Claude、DeepSeek 等）是"供应商"，NewAPI 是"中间层"，你的应用或用户是"调用方"。

**适合谁用**

![图像](https://pbs.twimg.com/media/HI1Z4qDb0AA2E4p?format=png&name=large)

**最小部署配置**

![图像](https://pbs.twimg.com/media/HI1Z-3ya0AAYedG?format=png&name=large)

**0-1 部署步骤**

**第一步：准备服务器**

> 一台 Linux 服务器（Ubuntu 22.04 推荐），有公网 IP。

安装基础工具：

```bash
apt update && apt install -y git curl nginx certbot python3-certbot-nginx
```

**第二步：安装 Docker**

```bash
curl -fsSL https://get.docker.com | sh
systemctl start docker
systemctl enable docker
docker --version
```

**第三步：拉取 NewAPI**

```bash
git clone https://github.com/QuantumNous/new-api.git
cd new-api
```

**第四步：配置环境变量**

复制示例配置：

```bash
cp .env.example .env
nano .env
```

修改以下字段：

```bash
# 数据库（MySQL 推荐）
SQL_DSN="root:你的强密码@tcp(localhost:3306)/newapi"

# Redis 连接串
REDIS_CONN_STRING="redis://localhost:6379"

# 流式响应超时（秒）
STREAMING_TIMEOUT=300

# Session 加密密钥
SESSION_SECRET="生成的随机字符串"

# Redis 加密密钥（使用 Redis 时必须）
CRYPTO_SECRET="生成的随机字符串"
```

生成随机密钥：

```bash
openssl rand -hex 32
```

运行两次，分别填入 SESSION\_SECRET 和 CRYPTO\_SECRET。

**第五步：启动服务**

```bash
docker compose up -d
```

等待镜像拉取完成（约 5-10 分钟）。

验证：

```bash
docker compose ps
```

看到 new-api 状态为 running 即成功。

**第六步：配置 Nginx 反向代理**

新建配置文件：

```bash
nano /etc/nginx/sites-available/new-api
```

写入 HTTP 反代配置：

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

启用配置：

```bash
ln -s /etc/nginx/sites-available/new-api /etc/nginx/sites-enabled/
nginx -t
systemctl restart nginx
```

**第七步：自动生成 HTTPS**

```bash
certbot --nginx -d 你的域名.com
```

按提示操作，Certbot 会自动配置 HTTPS 和 80 跳转 443。

验证：

```bash
nginx -t
systemctl restart nginx
```

浏览器访问 [https://你的域名.com](https://xn--6qqv7i2xdt95b.com/)，看到 NewAPI 登录页即成功。

**第八步：初始化设置**

首次访问会进入初始化页面：

![图像](https://pbs.twimg.com/media/HI2sUjgakAAI887?format=jpg&name=large)

**后台怎么添加渠道**

渠道 = 上游模型供应商。

登录后台 → 渠道 → 添加渠道。

![图像](https://pbs.twimg.com/media/HI2s7Eoa0AA8JeS?format=jpg&name=large)

添加后点击**测试**，验证是否能正常调用。

> 💡 建议添加 2-3 个不同供应商的渠道，一个挂了自动切换。

**怎么给用户分发 Token**

Token = 用户调用你的 API 的密钥。

令牌 → 添加令牌。

![图像](https://pbs.twimg.com/media/HI2taYdaAAA2ru2?format=jpg&name=large)

点击提交，复制生成的 Key 给用户。

用户使用方式和 OpenAI 完全一样：

```python
import openai

client = openai.OpenAI(
    api_key="用户令牌",
    base_url="https://你的域名.com/v1"
)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Hello"}]
)
```

**怎么测试调用**

用 curl 测试：

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

**盈利方式**

NewAPI 可以做技术底座，但不等于可以无脑转售 API。

对外服务前，先确认：

- 上游 API 条款是否允许转售
- 支付方式（Stripe、支付宝、加密货币）
- 内容安全审核机制
- 日志保留和审计要求
- 用户实名认证
- 税务和当地合规要求

> ⚠️ 不要只卖 API，要包装成服务：

![图像](https://pbs.twimg.com/media/HI2thVOa4AEBRSL?format=jpg&name=large)

**核心结论**

NewAPI 把多模型调用变成一件事：**一个接口，一个密钥，统一管理**。

自用能省钱，团队能省心，商用能变现。

但技术底座只是起点，真正的价值在于你怎么包装它、服务谁、解决什么问题。

2026 年，会搭 NewAPI 的人，比只会调 API 的人，多一条护城河。