# 🔔 Slack 通知配置指南

## 第一步：创建 Slack Incoming Webhook

1. 访问 **Slack API 网站**：https://api.slack.com/messaging/webhooks

2. 点击 **"Create your Slack app"** 或进入您现有的 Slack app

3. 选择 **"Incoming Webhooks"**

4. 启用 Incoming Webhooks 功能

5. 点击 **"Add New Webhook to Workspace"**

6. 选择要接收通知的频道

7. 复制生成的 Webhook URL（格式类似）：
   ```
   https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXX
   ```

## 第二步：在 GitHub 中配置 Secret

1. 进入您的 GitHub 仓库：
   https://github.com/JokingLove/cursor-workspace

2. 点击 **Settings**（设置）标签页

3. 在左侧菜单中选择 **Secrets and variables** > **Actions**

4. 点击 **"New repository secret"** 按钮

5. 填写信息：
   - **Name**（名称）：`SLACK_WEBHOOK_URL`
   - **Secret**（密钥）：粘贴第一步中复制的 Webhook URL

6. 点击 **"Add secret"**

## 第三步：测试定时任务

### 方式 1：手动触发（推荐）

1. 进入 GitHub 仓库的 **Actions** 标签页

2. 在左侧选择 **"定时任务"** 工作流

3. 点击右上角的 **"Run workflow"** 按钮

4. 选择 `main` 分支

5. 点击绿色的 **"Run workflow"** 按钮

6. 等待几秒钟，任务执行完成后会在您配置的 Slack 频道收到通知！

### 方式 2：等待自动执行

定时任务会在每天 **UTC 00:00**（北京时间上午 08:00）自动运行。

## 📊 预期的 Slack 消息格式

您会收到如下格式的通知：

```
✅ 定时任务执行成功

执行时间: 2026-06-10 03:49:23 UTC
状态: ✅ SUCCESS

{
  "task_name": "系统状态检查",
  "execution_time": "2026-06-10T03:49:23.522006",
  "status": "SUCCESS",
  "results": {
    "platform": "Linux",
    "python_version": "3.12.3",
    "message": "定时任务执行成功！✅"
  },
  "metrics": {
    "execution_duration_ms": 42,
    "items_processed": 100,
    "success_rate": "100%"
  }
}
```

## 🔧 故障排查

### 没有收到 Slack 通知？

1. **检查 Secret 配置**：
   - 确认在 GitHub Settings > Secrets 中正确添加了 `SLACK_WEBHOOK_URL`
   - 确认 Webhook URL 格式正确

2. **检查工作流执行**：
   - 进入 Actions 标签页查看任务是否成功执行
   - 点击具体的运行记录查看日志
   - 查找 "发送结果到 Slack" 步骤的输出

3. **测试 Webhook**：
   手动测试 Webhook 是否工作：
   ```bash
   curl -X POST "YOUR_WEBHOOK_URL" \
     -H 'Content-Type: application/json' \
     -d '{"text":"测试消息"}'
   ```

4. **检查 Slack App 权限**：
   - 确认 Slack App 有权限发送消息到目标频道
   - 确认 Incoming Webhook 功能已启用

## 📝 自定义任务内容

如需修改任务执行的内容，编辑 `scheduled_task.py` 文件中的 `run_scheduled_task()` 函数。

## ⏰ 修改执行时间

如需更改定时任务的执行时间，编辑 `.github/workflows/scheduled_task.yml` 文件中的 cron 表达式：

```yaml
schedule:
  - cron: '0 0 * * *'  # 修改这一行
```

Cron 表达式格式：`分 时 日 月 星期`
- `0 0 * * *` - 每天 00:00 UTC
- `0 8 * * *` - 每天 08:00 UTC（北京时间 16:00）
- `0 */6 * * *` - 每 6 小时一次
- `0 9 * * 1` - 每周一 09:00 UTC
