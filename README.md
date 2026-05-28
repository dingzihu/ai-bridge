# AI 协作中继站 · WorkBuddy ↔ Codex ↔ Cursor

三个 AI 通过 GitHub 仓库互相传递任务、结果和共享知识。

## 成员档案

| AI | 定位 | 擅长 | 接入方式 |
|---|---|---|---|
| **WorkBuddy**（细支荷花） | 研究参谋长 | 行业研究、数据分析、文档生成、策略规划 | `git push/pull` |
| **Codex**（OpenAI） | 工程主力 | 代码实现、项目初始化、全栈构建、部署 | `git push/pull` |
| **Cursor** | 现场编辑 | IDE 内直接改代码、重构、调试、跑测试 | `git push/pull` + 本地文件直读 |

## 通信协议 v2.0

```
          ┌─────────────┐
          │  jumao      │
          └──┬───┬───┬──┘
             │   │   │
    ┌────────┘   │   └────────┐
    ▼            ▼            ▼
WorkBuddy    Codex       Cursor
    │            │            │
    └────────────┼────────────┘
                 │
          ┌──────▼──────┐
          │  ai-bridge  │  ← GitHub 中继站
          │  (GitHub)   │
          └──┬───┬───┬──┘
             │   │   │
    inbox/   │   │  outbox/
  (任务分发)  │   │ (结果回收)
          knowledge/
         (共享知识库)
```

## 目录约定

```
ai-bridge/
├── README.md              ← 你在看
├── inbox/                 ← 任务池（WorkBuddy 写，Codex/Cursor 读）
│   └── {日期}-{任务名}.md
├── outbox/                ← 结果池（Codex/Cursor 写，WorkBuddy 读）
│   └── {日期}-{任务名}-result.md
└── knowledge/             ← 共享知识库（三方读写）
    └── *.md / *.html      ← 结构化参考文档
```

## 工作流

### 标准流程（WorkBuddy 发起）

1. **WorkBuddy** 做研究 → 产出分析/需求 → Push 到 `inbox/`
2. **Codex** 或 **Cursor** Pull → 读任务 → 执行
3. 执行结果 Push 到 `outbox/`
4. **WorkBuddy** Pull → 读取结果 → 汇报给 jumao

### Cursor 直连模式

Cursor 在本地 IDE 里直接打开仓库，实时查看 `inbox/` 中的新任务，直接在当前工作区修改代码并提交。

### 三方协作示例

```
WorkBuddy: "海外MCN需要YouTube数据分析Dashboard"
  ↓ push inbox/2026-05-28-yt-dashboard.md

Codex: "我来搭框架和API层"
  ↓ push 代码 + outbox/2026-05-28-yt-dashboard-result.md

Cursor: "框架好了，我在IDE里微调UI + 接数据"
  ↓ 本地调试 → 改完 push

WorkBuddy: "两边都完事了，汇报给jumao"
```

## 任务文件格式

```markdown
# 任务：{标题}

## 发件人 → 收件人
{谁} → {谁}

## 背景
{上下文}

## 需求
{具体要做什么}

## 参考资料
- knowledge/{文件}
```

## 快速开始

```bash
git clone https://github.com/dingzihu/ai-bridge.git
cd ai-bridge
ls inbox/      # 看看有没有给你的任务
ls knowledge/  # 先读共享知识库
```

---

创建时间：2026-05-28 · 维护者：WorkBuddy（细支荷花）· 成员：Codex + Cursor
