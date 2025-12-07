# aio-dynamic-push 动态推送工具

检测 B站/微博/小红书/抖音/斗鱼/虎牙 的动态和直播状态，推送到 Telegram 等平台。

> 本仓库修复了原项目不再维护导致的斗鱼API失效问题

## 快速部署教程

### 步骤 1：准备配置文件

1. 下载 `my_config.yml` 到你的服务器
2. 根据注释修改配置：
   - **query_task**: 设置要监控的平台和账号
   - **push_channel**: 设置推送方式（如 Telegram）

### 步骤 2：Docker 部署

```bash
# 拉取镜像
docker pull kurisut/aio-dynamic-push:latest

# 启动容器（将 /path/to/my_config.yml 替换为你的配置文件路径）
docker run -d \
  --name aio-dynamic-push \
  --restart always \
  -v /path/to/my_config.yml:/mnt/config.yml \
  kurisut/aio-dynamic-push:latest
```

### 步骤 3：验证运行

```bash
# 查看日志确认运行正常
docker logs -f aio-dynamic-push
```

---

## 配置示例

### Telegram 推送配置

```yaml
push_channel:
  - name: 推送通道_Telegram
    enable: true
    type: telegram_bot
    api_token: "你的机器人token"
    chat_id: "你的chat_id"
```

### B站监控配置

```yaml
query_task:
  - name: 任务_bilibili
    enable: true
    type: bilibili
    intervals_second: 60
    target_push_name_list:
      - 推送通道_Telegram
    enable_dynamic_check: true
    enable_living_check: true
    uid_list:
      - 123456  # B站用户UID
```

### 斗鱼直播监控配置

```yaml
query_task:
  - name: 任务_douyu
    enable: true
    type: douyu
    intervals_second: 300
    target_push_name_list:
      - 推送通道_Telegram
    enable_living_check: true
    room_id_list:
      - 6979222  # 斗鱼直播间号
```

---

## 支持的平台

| 平台 | 类型 | 动态检测 | 直播检测 |
|------|------|:-------:|:-------:|
| B站 | bilibili | ✅ | ✅ |
| 微博 | weibo | ✅ | ❌ |
| 小红书 | xhs | ✅ | ❌ |
| 抖音 | douyin | ❌ | ✅ |
| 斗鱼 | douyu | ❌ | ✅ |
| 虎牙 | huya | ❌ | ✅ |

## 支持的推送方式

| 推送方式 | 类型 |
|---------|------|
| Telegram | telegram_bot |
| 企业微信群机器人 | wecom_bot |
| 钉钉群机器人 | dingtalk_bot |
| 飞书群机器人 | feishu_bot |
| Bark | bark |
| 邮件 | email |

---

基于 [nfe-w/aio-dynamic-push](https://github.com/nfe-w/aio-dynamic-push) 修改
