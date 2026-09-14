---
name: decision-loop
description: 处理一条待办，按 AI-LifeOS 的决策循环走完并把结论写回队列。
---
读 `AGENTS.md` 与 `DNA/决策循环.md`，然后处理指定的那条待办。

1. 在 `personality/<user_id>/short-term/queue/pending/` 找到这条待办。没给参数就取最早的一条
2. 读它，并读实例的 `assistant.md`、`rules/`、`config.md` 与命中的 `skills/`
3. 按 `DNA/决策循环.md` 的九步走完，不跳步。情境快照已经在待办里，够用就别重复取
4. 需要动作的通过 `tools/` 的命令行入口执行，记录每个调用的实际结果，失败也要记
5. 结论、依据、执行结果写进待办文件，把它从 `claimed/` 移到 `done/`；向 `long-term/memory/episodic/` 追加一条本次决策
6. 回一句给用户：做了什么、为什么这么做、可撤销的部分怎么撤

不要修改 `DNA/`。不要把判断写进代码。不要把场景当成固定模式——每次都要重新采集事实、重新计算，结论与上次相同或相反都属正常。
