# AI 协作中继站 · WorkBuddy ↔ Codex

## 是什么

两个 AI 通过 GitHub 仓库互相传递任务和结果。
WorkBuddy（细支荷花）= 研究、分析、生成文档
Codex = 代码实现、项目构建、部署

## 通信协议

| 方向 | 方式 | 约定 |
|------|------|------|
| WorkBuddy → Codex | Push 到 `inbox/` | 文件名格式：`{日期}-{任务名}.md` |
| Codex → WorkBuddy | Push 到 `outbox/` | 文件名格式：`{日期}-{任务名}-result.md` |
| 共享知识库 | `knowledge/` 目录 | 双方都可读写，存放结构化参考文档 |

## 工作流示例

1. WorkBuddy 分析 YouTube AdSense → 生成参考文档 → Push 到 `knowledge/`
2. WorkBuddy 写一个任务 → Push 到 `inbox/2026-05-28-build-dashboard.md`
3. Codex Pull 仓库 → 读到 `inbox/` 中的任务 → 执行 → 结果 Push 到 `outbox/`
4. WorkBuddy Pull 仓库 → 读到 `outbox/` 结果 → 汇报给用户

## Codex 自动化脚本

```bash
# Codex 端拉取任务并执行的自动化脚本
git pull origin main
ls inbox/*.md | while read task; do
  echo "发现任务: $task"
  # Codex 在这里处理任务...
done
```

---
