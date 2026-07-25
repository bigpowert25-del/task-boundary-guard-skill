# Task Boundary Guard Skill / 任务边界护栏 Skill

A lightweight Codex Skill for locking task boundaries before work starts and preventing AI coding agents from drifting during execution.

一个轻量 Codex Skill，用于在任务开工前锁定边界，并在执行过程中防止 AI Coding Agent 跑偏。

## Why / 为什么需要

AI agents often drift when a task begins with unclear scope, mixed context, or attractive side ideas.

当任务边界不清、上下文混杂，或中途出现新想法时，AI Agent 很容易把当前主线带偏。

This Skill adds a small startup gate:

这个 Skill 增加了一个轻量开工门禁：

- keep the default mode as execution, not interrogation
- 默认模式是执行，不是审问
- define the goal
- 明确目标
- define what is in and out of scope
- 明确哪些能做、哪些不能碰
- define observable success evidence
- 明确什么证据算完成
- use one-question-at-a-time reverse questioning when the boundary is unclear
- 边界不清时，用一次一问的反向提问先收敛
- stop before out-of-scope reads, writes, publishing, installs, or long-term rule changes
- 在越界读取、写入、发布、安装依赖或写长期规则前暂停

## What It Does / 它做什么

For small tasks, it asks the agent to state one short boundary sentence and proceed.

对小任务，它要求 Agent 用一句话锁边界，然后直接执行。

It uses a simple flow budget:

它使用一个简单的流畅性预算：

- L0: no question; optionally state one boundary sentence and finish.
- L0：不提问；必要时一句话定边界，然后完成。
- L1: proceed with the smallest reversible assumption.
- L1：用最小可逆假设继续。
- L2: ask one question only when the answer changes the direction or deliverable.
- L2：只有答案会改变方向或交付物时，才问一个问题。
- L3: use a boundary card and ask only what prevents irreversible, external, or cross-project mistakes.
- L3：使用边界卡；只问避免不可逆、外部或跨项目错误所必需的问题。

For risky or long-running tasks, it asks the agent to write a compact boundary card:

对高风险或长任务，它要求 Agent 写一张短边界卡：

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

## Install / 安装

Clone this repository, then copy the Skill folder into your Codex skills directory:

克隆本仓库，然后把 Skill 文件夹复制到你的 Codex skills 目录：

```bash
mkdir -p ~/.codex/skills
cp -R task-boundary-guard ~/.codex/skills/
```

Open a new Codex task or restart Codex so the Skill index refreshes.

打开一个新的 Codex 任务，或重启 Codex，让 Skill 索引刷新。

## Usage / 使用

You can invoke it explicitly:

可以显式调用：

```text
Use $task-boundary-guard to lock the task boundary before starting this work.
```

Or say it naturally:

也可以自然表达：

```text
先帮我锁定任务边界，避免执行时跑偏。
```

```text
这个任务不要污染主线，先收敛边界再做。
```

## Good For / 适合场景

- multi-file development
- 多文件开发
- cross-project cleanup
- 跨项目整理
- automation or scheduling work
- 自动化或调度任务
- publishable work
- 可发布工作
- tasks that reference prior context, screenshots, favorites, or another agent
- 引用历史上下文、截图、收藏或其他 Agent 的任务
- long-running tasks where side ideas can hijack the thread
- 容易被旁支想法劫持的长任务

## Not For / 不适合

- translating one sentence
- 翻译一句话
- reading one file
- 读取一个文件
- running a harmless one-line check
- 运行一个无风险单行检查
- tasks that are already small, clear, and reversible
- 已经很小、很清楚、可逆的任务

## License / 许可证

MIT
