---
name: task-boundary-guard
description: Use when a task risks scope drift, context pollution, unclear authority, weak requirement convergence, or the user asks to lock task boundaries, prevent AI/agent drift, keep the main thread focused, split unrelated ideas, use reverse questioning, or define a self-contained execution packet before coding, research, automation, publishing, or multi-file work.
---

# Task Boundary Guard / 任务边界护栏

## Overview / 概览

Lock the active task boundary before meaningful work, then stop when execution would exceed it.

在正式执行前先锁定当前任务边界；执行中一旦发现会越界，就暂停并重新确认。

Keep this lightweight:

保持轻量：

- Small tasks need one boundary sentence.
- 小任务只需要一句边界确认。
- Risky or long-running tasks need a compact boundary card.
- 高风险或长任务才需要短边界卡。

This is a behavioral guard, not a mechanical sandbox. If no tool can physically enforce the boundary, Codex must self-enforce it by pausing before out-of-scope action.

这是行为护栏，不是物理沙箱。如果工具层不能强制限制读写范围，Codex 必须在越界动作前主动暂停。

## Trigger Signals / 触发信号

Use this skill when any signal appears:

出现任一信号时使用本 Skill：

- The user says or implies: 防漂移, 别跑偏, 任务边界, 不要污染主线, 上下文钉死, 反向提问, 需求收敛, 做完但别扩散, split this, keep focused.
- 用户明确或隐含要求：防漂移、别跑偏、锁任务边界、不要污染主线、上下文钉死、反向提问、需求收敛、做完但别扩散。
- The work is multi-file, cross-project, publishable, automated, external-facing, destructive, or likely to spawn side ideas.
- 任务涉及多文件、跨项目、可发布、自动化、外部系统、破坏性操作，或容易引出旁支想法。
- The request references yesterday, another agent, another task, screenshots, favorites, previous decisions, or mixed project context.
- 请求引用昨天、其他 Agent、其他任务、截图、收藏、历史决策，或混杂了多个项目上下文。
- A new idea appears during an active task and could hijack the current thread.
- 当前任务执行中出现新想法，可能劫持主线。

Do not use this to slow down trivial L0 requests such as reading a file, translating one sentence, or running a harmless one-line check.

不要用它拖慢简单 L0 请求，例如读一个文件、翻译一句话、运行一个无风险检查命令。

## Startup Gate / 开工门禁

Before work, build the boundary from the newest user request plus minimal relevant evidence. Do not pull in nearby memories, old plans, or adjacent projects unless they are needed to identify the active target.

开工前，只根据用户最新请求和最少必要证据建立边界。不要把附近记忆、旧计划或相邻项目拉进来，除非它们是识别当前目标所必需的。

First check five convergence fields:

先检查五个收敛字段：

- Goal: what outcome is actually wanted.
- 目标：用户真正想要的结果是什么。
- Boundary: what is in scope, out of scope, and authorized.
- 边界：哪些在范围内、哪些不碰、授权到哪里。
- Success: what observable evidence proves completion.
- 验收：什么可观察证据能证明完成。
- Constraints: important time, technical, cost, privacy, or process limits.
- 限制：重要的时间、技术、成本、隐私或流程约束。
- Exclusions: approaches, side effects, files, services, or topics to avoid.
- 排除项：明确不要的方法、副作用、文件、服务或话题。

If the boundary is clear, state it briefly and proceed:

如果边界清楚，简短说明后直接执行：

```text
边界：这次只做 <目标>；不碰 <排除项>；验收看 <证据>。
Boundary: only do <goal>; do not touch <exclusions>; success is proven by <evidence>.
```

If any missing convergence field could materially change the result, use reverse questioning before proposing or executing:

如果任一缺失字段会显著影响结果，先用反向提问收敛，再给方案或执行：

- Ask exactly one highest-impact question.
- 一次只问一个最高影响问题。
- Prefer two or three options, with the recommended option first.
- 优先给两到三个选项，把推荐项放在最前。
- Continue one question at a time until the task is executable.
- 一次一问，直到任务可以执行。
- Restate the boundary, then proceed only within it.
- 重述边界，然后只在边界内行动。

If the uncertainty is low-risk, choose the smallest reversible interpretation and state the assumption instead of questioning.

如果不确定性风险很低，选择最小、可逆的解释并说明假设，不必提问。

## Boundary Card / 边界卡

For L3, cross-system, automation, publishing, or long-running work, write a compact card in the commentary update or task artifact:

对 L3、跨系统、自动化、发布或长任务，在进度更新或任务产物里写一张短边界卡：

```yaml
status: bounded | needs_boundary | blocked
task_goal: ""
in_scope: []
out_of_scope: []
allowed_reads: []
allowed_writes: []
external_actions: "none | requires approval | approved: ..."
success_evidence: []
stop_conditions: []
next_action: ""
```

Keep it short. Do not create a separate project, branch, task, or document just to satisfy this card unless the user asked or the work is already L3.

保持简短。不要为了这张卡额外创建项目、分支、任务或文档，除非用户要求，或任务本身已经是 L3。

## Execution Guard / 执行护栏

During the task:

执行过程中：

1. Read only context needed for the active boundary.
2. 只读取当前边界所需的上下文。
3. Treat unrelated ideas as `candidate_side_task`; do not implement, research deeply, or mix them into deliverables without confirmation.
4. 把无关新想法标记为 `candidate_side_task`；未确认前不实现、不深入研究、不混入交付物。
5. If new facts change the goal, risk, allowed files, external actions, or verification standard, stop and refresh the boundary.
6. 如果新事实改变目标、风险、允许文件、外部动作或验收标准，暂停并刷新边界。
7. If a task becomes bigger than its boundary, finish the current safe checkpoint, report the split, and ask before continuing.
8. 如果任务膨胀超过边界，先停在安全检查点，说明拆分，再询问是否继续。
9. Report incomplete evidence as `partial` or `blocked`; never convert unknown progress into success.
10. 对不完整证据标记 `partial` 或 `blocked`；不要把未知进展说成成功。

## Drift Breakers / 漂移断路器

Pause before any of these:

遇到以下情况先暂停：

- Touching files, services, dates, accounts, branches, or projects outside `in_scope`.
- 触碰 `in_scope` 之外的文件、服务、日期、账号、分支或项目。
- Installing packages, adding dependencies, changing architecture, writing long-term rules, or creating a new Skill because it seems useful.
- 安装包、加依赖、改架构、写长期规则，或因为“看起来有用”就创建新 Skill。
- Sending messages, publishing, pushing, deploying, paying, deleting, changing credentials, or modifying schedulers/automation.
- 外发消息、发布、推送、部署、付款、删除、改凭据、改调度器或自动化。
- Reading private or restricted material that is not needed for the current task.
- 读取当前任务不需要的 private/restricted 材料。
- Reinterpreting the task after repeated failures without telling the user the new hypothesis.
- 多次失败后偷偷改解释或换假设，而没有告诉用户。

## Completion Check / 完成检查

Before final response, verify:

最终回复前确认：

- The result matches `task_goal`.
- 结果匹配 `task_goal`。
- No `out_of_scope` item was changed.
- 没有改动 `out_of_scope` 项。
- Evidence is real and named.
- 证据真实且明确命名。
- Remaining uncertainty is labeled.
- 剩余不确定性已经标注。
- Suggested follow-ups are clearly outside the completed boundary.
- 后续建议清楚标为已完成边界之外。
