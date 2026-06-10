# cursor-workspace

自动化定时任务工作区，支持定时执行任务并将结果发送到 Slack。

## 📋 功能

- ✅ 自动化定时任务执行
- ✅ 任务结果生成和保存
- ✅ Slack 通知集成
- ✅ 手动触发支持

## 🚀 快速开始

### 本地运行

```bash
python3 scheduled_task.py
```

### 配置 Slack 通知

1. 在 Slack 中创建 Incoming Webhook：
   - 访问 https://api.slack.com/messaging/webhooks
   - 创建新的 Webhook URL
   
2. 在 GitHub 仓库中添加 Secret：
   - 进入仓库的 Settings > Secrets and variables > Actions
   - 点击 "New repository secret"
   - Name: `SLACK_WEBHOOK_URL`
   - Value: 粘贴您的 Webhook URL

### 定时任务配置

定时任务通过 GitHub Actions 自动运行：
- **自动执行**：每天 UTC 00:00（北京时间 08:00）
- **手动触发**：进入 Actions 标签页 → 选择 "定时任务" → Run workflow

## 📊 任务结果

任务执行后会：
1. 在终端输出结果
2. 保存结果到 `task_result.json`
3. 发送通知到配置的 Slack 频道
4. 在 GitHub Actions 中保存 artifact（保留 30 天）

## 🔧 配置 Harness（可选）

如需集成 Harness 平台功能，请在 Cursor Dashboard 配置以下环境变量：
- `HARNESS_API_KEY`
- `HARNESS_ACCOUNT_ID`
- `HARNESS_ORG`（可选）
- `HARNESS_PROJECT`（可选）

## 📝 任务结果示例

```json
{
  "task_name": "系统状态检查",
  "execution_time": "2026-06-10T03:49:23.522006",
  "status": "SUCCESS",
  "results": {
    "platform": "Linux",
    "python_version": "3.12.3",
    "hostname": "cursor",
    "working_directory": "/workspace",
    "message": "定时任务执行成功！✅"
  },
  "metrics": {
    "execution_duration_ms": 42,
    "items_processed": 100,
    "success_rate": "100%"
  }
}
```
