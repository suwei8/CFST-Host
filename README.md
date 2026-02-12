# CFST-Host

基于 GitHub Actions 的 Cloudflare 自动测速工具，定时筛选最优香港 Cloudflare 代理 IP。

## 工作原理

```
⏰ 定时触发 → 📥 获取CF全部IPv4段 → 🔐 SSH隧道连接家庭主机
→ 📤 上传IP列表 → 🚀 运行cfst测速(HKG) → 📊 提取最快IP
→ 🚀 推送到 suwei8/blog
```

## Repository Secrets 配置

在 GitHub 仓库 **Settings → Secrets and variables → Actions** 中创建以下 Secrets：

| Secret 名称 | 说明 | 示例 |
|---|---|---|
| `SSH_HOST` | Cloudflare Tunnel SSH 地址 | `wky-ssh.555606.xyz` |
| `SSH_USERNAME` | SSH 用户名 | `root` |
| `SSH_PASSWORD` | SSH 密码 | `your-password` |
| `SPEEDTEST_URL` | 测速文件 URL | `https://xxx/speedtest.bin` |
| `BLOG_DEPLOY_TOKEN` | GitHub PAT (需要 `repo` 权限) | `ghp_xxxx` |

> **注意**: Cloudflare IPs 获取接口 (`/client/v4/ips`) 为公开接口，无需 API Token。

### 创建 BLOG_DEPLOY_TOKEN

1. 前往 [GitHub Settings → Personal Access Tokens](https://github.com/settings/tokens)
2. 点击 **Generate new token (classic)**
3. 勾选 `repo` 权限
4. 生成后复制到 Secrets 中

## 远程主机要求

- `/root/cfst/cfst` - CloudflareSpeedTest 可执行文件
- 测速结果输出到 `/root/cfst/result.csv`

## 定时运行

默认每天 **UTC 02:00（北京时间 10:00）** 自动运行。可在 Actions 页面手动触发。
