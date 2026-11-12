# TrackTXV

腾讯视频追更日历追踪器 —— 免登录抓取腾讯视频公开更新数据，每日定时抓取、Webhook 推送、WebUI 查看。

**镜像即交付**：运行只需拉取 Docker 镜像，无需任何源码。

```bash
docker pull fromeverywhere/tracktxv:0.4
```

## 快速开始

```bash
docker run -d --name tracktxv \
  -p 8080:8080 \
  -e TXV_WEBHOOK_URL="https://你的webhook地址" \
  -v tracktxv_data:/data \
  fromeverywhere/tracktxv:0.4
```

或使用 [docker-compose.yml](docker-compose.yml)：

```bash
docker compose up -d
```

打开 `http://<主机>:8080`，初始账号 `admin`、初始密码 `tracktxv`（**部署后请立即修改**）。

## 功能

- **今日更新**：免登录抓取腾讯视频公开数据源，按播出状态分组展示
  （追更 / 完结 / SVIP抢先 / 点映 / 即将上映（含上映日期））
- **分类**：电视剧 / 动漫 / 综艺
- **直达链接**：封面页与分集直达 URL，支持跳转与一键复制
- **每日推送**：按 cron（默认每天 09:00）抓取一次并推送 Webhook（支持 text / json 报文）
- **历史**：保留每日快照，可查近 60 天
- **WebUI**：登录保护、明暗主题、卡片式布局

## 环境变量

| 变量 | 默认值 | 说明 |
| --- | --- | --- |
| `TXV_WEBHOOK_URL` | 空 | Webhook 推送地址（留空则不推送） |
| `TXV_WEBHOOK_FORMAT` | `text` | 报文格式：`text`（文本）/ `json` |
| `TXV_WEBHOOK_TITLE` | 腾讯视频·今日追更 | 推送标题 |
| `TXV_CRON` | `0 9 * * *` | 每日抓取计划（标准 5 字段 cron） |
| `TXV_TIMEZONE` | `Asia/Shanghai` | 时区 |
| `TXV_RUN_ON_START` | `true` | 启动时立即抓取一次 |
| `TXV_AUTH_USER` | `admin` | 登录账号 |
| `TXV_AUTH_PASSWORD` | `tracktxv` | 登录密码（请修改） |
| `TXV_DATA_DIR` | `/data` | 数据目录（建议挂卷持久化） |
| `TXV_PROXY` | 空 | 网络代理（可选） |

## API

除 `/api/health`、`/api/session`、`/api/login` 外均需登录会话（cookie）。

- `GET /api/health` — 健康检查
- `POST /api/login` / `POST /api/logout` — 登录 / 登出
- `GET /api/today` — 今日更新数据
- `GET /api/history?days=7` — 历史快照
- `POST /api/trigger` — 立即抓取并推送
- `POST /api/test-webhook` — 测试推送通道

## 数据来源说明

数据取自腾讯视频公开接口，仅供个人追更提醒使用，请勿用于商业用途。
