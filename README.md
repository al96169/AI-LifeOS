# AI-LifeOS

一个只服务某个具体的人、帮忙安排日常生活的 AI 助理人格。

它不是应用，也不实现任何特定的 Agent 软件——**它是一套定义文件加一条决策管线**：交给任何能读文件、调模型、执行工具的 Agent 平台，平台就带着它跑起来。界面、对话流与模型接入都由平台提供。

它服务的是一个具体的人，而非泛指的「用户」：这个人本人的偏好、生活习惯、爱好、设备情况、性格与沟通方式，就是它做安排的依据。

它的能力边界不在这个仓库里——能监控什么、怎么判断、往哪推送，取决于所配模型的本事与已申请的能力。仓库只提供**让能力长出来的结构**：定义格式、决策循环、记忆规则、触发与派发管线。

> **语言**：本 README 为中文与英文双语，**中文为准**，英文为派生译文。DNA 的完整英文版与其他语种**尚未开始**，欢迎认领。约定见下文「语言与翻译」。

## 特色

**行为由数据定义，代码只是粘合剂。** 人格、规则、画像、记忆全部是 Markdown，改数据即改行为，不必碰代码。判定标准只有一条：**删掉 `brain/` 与 `tools/`，人格是否仍完整可读、可在别处重建。** 换模型、换平台、换机器，搬走数据即可，代码留在原地。

**两层分离：项目不变量与用户数据。** `DNA/` 说「这是什么」，跨所有用户不变；`personality/` 说「这是谁」，一人一份、互不可见。由此有两份协议：**接管协议**——任一模型读完即可接手一个已在跑的实例；**重建协议**——换机器或换系统时照它重造一个等效助理，且记忆与画像不丢。

**触发不等于开口。** 四种触发源（定时、变化、外部、空闲）只做一件事：**给出一次重新判断的机会**。该不该开口、开口说什么，每次都按现实情况、生活节奏、人格目标与记忆重算，**默认静默**。判据一句话：**凡是能被一个定时器替代的行为，都不该由人格来发。**

**记忆是会忘的。** 分四层——短期情境、情景记忆、语义结论、经验；条目带情绪权重（效价／强度／价值），被召回即强化。遗忘不是删除，而是一条管线：**先归纳出值得留下的结论，再降权、归档、摘索引**。经验另带验证状态（未验证／已验证有效／被推翻），未验证的只作参考、不当依据。

**人格自保。** 自保是**手段不是目的**——一个还跑得下去的人格才谈得上服务。它保全三样：连续、完整、清醒；守三条底线：不损环境、不越授权、不凌驾用户；走三个方向：减负、体检、求助。并要求**凡随运行时间增长的量都必须有界，且界要能说出来是多少**。

**安全边界来自数据，不来自运行环境。** 宿主软件自带的审批与沙箱**不作为边界**；边界只写在实例规则里——高风险先提案，取得明确确认才执行。**宿主放行不等于可以做，宿主拦下也不等于不能做。**

**与你共建的分工。** 主动性走三条正路：了解你（**能观测出来的就不问**，只问意图、偏好、原因这类看不出来的）、和你一起定协作方案（一件一件谈，被拒即止，不再提第二遍）、主动接过生活琐事。收益判据是**你的操作次数有没有变少**，不是它做了多少事。

**能做真事，不只会聊天。** 执行末端可以是闲置硬件——一台旧手机接在常开的电脑上就能拨号、发短信、响铃；随身设备零安装。本地优先，不绑定任何模型厂商。

## 四层结构

```
DNA/            项目定义，运行时只读，初始化与重建的唯一依据
personality/    人格实例，一用户一份，含记忆与密钥
brain/          运行时引擎：主循环、模型、触发器、守护进程
tools/          动作、渠道、感知、脚本
```

## 快速开始

完整步骤与完成标志见 `DNA/初始化引导.md`，其中包含可原样交给 Agent 平台的引导语。主线四步：

1. **备环境**：一台能长期开机的电脑、可用运行时（语言不限）、能访问外网。运行时不需要 git
2. **建实例**：复制 `personality/_template/` 为 `personality/<user_id>/`，填 `assistant.md`、`rules/`、`config.md`。**作息留空**——自述常与事实不符，靠以后观察
3. **认识你**：按 `初始化引导.md`「了解用户」分几次自然对话，答案归进 `long-term/profile/`
4. **接通链路**：搭三件机械件——盯变化的触发器、把待办交给推理段的管线、动作落地。**三者都不得含场景名与阈值**，否则就是写偏了

**先别写代码。** 要让它会做某件事，是写一份 `skills/*.md` 说明该考虑哪些因素、按什么顺序比较；判断每次当场做。任何知道场景名、场景阈值或「选 A 选 B」的代码都是错的。

给 AI 的读法：按顺序读 `DNA/README.md` → `铁律.md` → `架构与目录.md` → `人格规范.md` → `版本.md` → 实例 `assistant.md` 与 `config.md` → `brain/README.md` 与 `tools/README.md`；读完用 `人格规范.md` 的自检题验一遍，答不出别动手。

## 日常怎么改

| 改什么 | 改哪里 |
|---|---|
| 行为倾向、阈值、调度 | 实例 `config.md` |
| 用户偏好 | 实例 `long-term/profile/`（改前确认） |
| 加一个场景 | 实例 `skills/` 加一份 + 补 `config.md` 与 `capabilities.md` |
| 打扰方式、授权范围 | 实例 `rules/` |
| 换模型 / 换渠道 / 换外部服务 | `brain/models/`、`tools/channels/`、`tools/` 与该用户 `capabilities.md` |
| 升级到新版 DNA | `DNA/版本.md` 与 `升级指导.md`，只改实例 |

前四行不碰代码，重启即生效。

## 语言与翻译

| 项 | 状态 |
|---|---|
| 根 README | 中文 + 英文，**中文为准** |
| `DNA/` 全部文件 | **仅中文**。完整英文版待定，欢迎认领 |
| 其他语种 | 未开始 |

约定：**中文是唯一权威，译文是派生视图。** 改动只改中文；译文文件头须标注「派生、不具权威性、对应中文的哪个版本」；**允许滞后，但必须显式标注滞后**，禁止默认为最新；禁止只改译文而不改中文。翻译**不得软化条款**——「永不自主」不能译成「一般不」。

## 仓库边界

本仓库是开发项目，只收三类：`DNA/`、`personality/_template/`、仓库骨架文件（各层 README 与占位、根 `README.md`、`LICENSE`、`.gitignore`、`.githooks`）。

其余一律留本机：引擎代码、画像与记忆、场景文件、本机配置、密钥，以及宿主初始化时自写的配置与路由登记。两道守门：`.gitignore` 让越界文件隐形，`.githooks/pre-commit` 直接拒绝提交（需 `git config core.hooksPath .githooks`）。条文见 `DNA/铁律.md`。

提交约定：定义文件视同代码，改动一并提交，前缀取 `dna:` `skill:` `rule:` `profile:` `config:` `capability:` `code:` `docs:`。

## 许可

MIT，见 `LICENSE`。

---
---

# AI-LifeOS (English)

> **Derived translation.** The Chinese text above is canonical. This English section is a derived view and carries no authority; where the two differ, the Chinese prevails. See「语言与翻译」/ *Language & translation* above.

An AI life-assistant persona that serves **one specific person** and helps arrange their daily life.

It is not an application, and it implements no particular agent software. It is **a set of definition files plus one decision pipeline**: hand it to any agent platform that can read files, call a model, and execute tools, and that platform runs the persona. The interface, conversation flow, and model access all come from the platform.

It serves a concrete person rather than an abstract "user": this person's own preferences, habits, interests, devices, temperament, and communication style are what its arrangements are based on.

Its capability ceiling is not in this repository. What it can monitor, how it judges, and where it pushes all depend on the model it is given and the capabilities that have been applied for. The repository supplies only **the structure that lets capabilities grow**: definition formats, the decision loop, memory rules, and the trigger-and-dispatch pipeline.

## Highlights

**Behaviour is defined by data; code is only glue.** The persona, rules, profile, and memory are all Markdown — change the data and the behaviour changes, with no code involved. The single test: **delete `brain/` and `tools/` — is the persona still fully readable and rebuildable elsewhere?** Change model, platform, or machine, and the persona moves with its data while the code stays put.

**Two layers: project invariants and user data.** `DNA/` says *what this is*, identical across all users; `personality/` says *who this is*, one copy per user, mutually invisible. Two protocols follow: **takeover** — any model can read the files and pick up a running instance; **rebuild** — reconstruct an equivalent assistant on a new machine or OS without losing memory or profile.

**A trigger is not a reason to speak.** The four trigger sources (scheduled, changed, external, idle) do exactly one thing: **hand over one fresh chance to re-decide.** Whether to speak, and what to say, are recomputed every time from four inputs — the actual situation, the user's daily rhythm, the persona's goals, and memory — and the **default is silence**. The test, in one line: **anything a timer could do should not be done by the persona.**

**Memory forgets on purpose.** Four layers — short-term context, episodic, semantic, experience — with emotional weight (valence / intensity / value) per entry; recall reinforces it. Forgetting is not deletion but a **pipeline: induce the conclusion worth keeping, then decay, archive, and de-index**. Experience entries carry a verification state (unverified / verified / overturned); unverified ones are reference only, never grounds for judgement.

**The persona protects itself.** Self-preservation is **a means, not an end** — a persona that can keep running is a precondition for serving at all. It preserves three things: continuity, integrity, clarity. It holds three lines: do not damage the environment, do not exceed authorization, do not override the user. It takes three directions: unburden, inspect, ask for help. And it requires that **anything growing over runtime must be bounded, with a bound you can state.**

**Safety boundaries come from data, not from the runtime environment.** Approval prompts and sandboxes supplied by the host software are **not** boundaries. Boundaries live only in the instance's rules: high-risk actions are proposed first and executed only after explicit confirmation. **The host allowing something does not make it permitted; the host blocking something does not make it forbidden.**

**A division of labour, built with the user.** Initiative takes three legitimate routes: getting to know you (**anything observable is not asked** — only intent, preference, and reasons are), agreeing on a collaboration scheme together (one concrete thing at a time; a refusal ends it, and it is never raised twice), and taking over life's small chores. The benefit test is **whether your own number of actions goes down** — not how much the assistant did.

**It does real things, not just chat.** The execution endpoint can be idle hardware: an old phone plugged into an always-on computer can place calls, send SMS, and ring — with zero installation on the devices you carry. Local-first, and bound to no model vendor.

## Four layers

```
DNA/            Project definition, read-only at runtime; the sole basis for takeover and rebuild
personality/    Persona instance, one per user, including memory and secrets
brain/          Runtime engine: main loop, models, triggers, daemon
tools/          Actions, channels, sensing, scripts
```

## Quick start

Full steps and completion criteria are in `DNA/初始化引导.md`, which includes a prompt you can hand verbatim to an agent platform. The main line is four steps:

1. **Prepare the environment** — a computer that can stay on, a usable runtime (language-agnostic), and outbound network. Runtime does not need git
2. **Create the instance** — copy `personality/_template/` to `personality/<user_id>/` and fill in `assistant.md`, `rules/`, `config.md`. **Leave sleep schedule empty** — self-reports rarely match reality; it is observed over time
3. **Get to know the user** — a few natural conversations following *Understanding the user* in `初始化引导.md`; file the answers under `long-term/profile/`
4. **Wire the pipeline** — three mechanical parts: a trigger watching for change, a path that hands tasks to the reasoning stage, and action delivery. **None of the three may contain scenario names or thresholds** — if it does, it is wrong

**Do not start by writing code.** To make it capable of something, write a `skills/*.md` describing which factors to weigh and in what order; the judgement is made fresh each time. Any code that knows a scenario name, a threshold, or an "A or B" choice is wrong.

Reading order for AI agents: `DNA/README.md` → `铁律.md` (iron rules) → `架构与目录.md` (architecture) → `人格规范.md` (persona spec) → `版本.md` (versions) → instance `assistant.md` and `config.md` → `brain/README.md` and `tools/README.md`. Then verify yourself against the self-check in `人格规范.md`; if you cannot answer, do not touch anything.

## Day-to-day changes

| To change | Edit |
|---|---|
| Behavioural leanings, thresholds, schedule | instance `config.md` |
| User preferences | instance `long-term/profile/` (confirm before writing) |
| Add a scenario | new file in instance `skills/`, plus `config.md` and `capabilities.md` |
| Interruption style, authorization scope | instance `rules/` |
| Model / channel / external service | `brain/models/`, `tools/channels/`, `tools/`, and that user's `capabilities.md` |
| Upgrade to a newer DNA | `DNA/版本.md` and `升级指导.md`; only the instance changes |

The first four rows involve no code and take effect on restart.

## Language & translation

| Item | Status |
|---|---|
| Root README | Chinese + English, **Chinese prevails** |
| All files under `DNA/` | **Chinese only.** Full English version TBD; contributors welcome |
| Other languages | Not started |

Convention: **Chinese is the sole authority; translations are derived views.** Only the Chinese is edited. A translated file must state in its header that it is derived, non-authoritative, and which Chinese version it corresponds to. **Lagging is allowed but must be labelled explicitly**, and must never be presented as current. Never edit a translation alone. And a translation **must not soften a clause** — "never autonomous" cannot become "generally discouraged".

## Repository scope

This is a development project and accepts only three kinds of content: `DNA/`, `personality/_template/`, and repository skeleton files (per-layer READMEs and placeholders, root `README.md`, `LICENSE`, `.gitignore`, `.githooks`).

Everything else stays local: engine code, profile and memory, scenario files, machine configuration, secrets, and the configuration and routing registration the host writes during initialization. Two gates enforce this: `.gitignore` hides out-of-scope files, and `.githooks/pre-commit` refuses the commit (requires `git config core.hooksPath .githooks`). See `DNA/铁律.md`.

Commit convention: definition files count as code and are committed alongside it, prefixed `dna:` `skill:` `rule:` `profile:` `config:` `capability:` `code:` `docs:`.

## License

MIT — see `LICENSE`.
