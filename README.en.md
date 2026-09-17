# AI-LifeOS

[简体中文](README.md) ｜ **English** ｜ Other languages (TBD)

> **Derived translation.** `README.md` (Simplified Chinese) is canonical. This file is a derived view and carries no authority; where the two differ, the Chinese prevails. See *Language & translation* below.

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

| File | Language |
|---|---|
| `README.md` | Simplified Chinese (**default, canonical**) |
| `README.en.md` | English, derived translation |
| Other languages | Not started; contributors welcome |

Convention: **Chinese is the sole authority; translations are derived views.** Only the Chinese is edited. A translated file must state in its header that it is derived, non-authoritative, and which Chinese version it corresponds to. **Lagging is allowed but must be labelled explicitly**, and must never be presented as current. Never edit a translation alone. And a translation **must not soften a clause** — "never autonomous" cannot become "generally discouraged".

**Adding a language:** add a sibling file named `README.<lang-code>.md` and add its link to the switcher line at the top of **every** language version; the same convention applies. All files under `DNA/` are currently Chinese only.

## Repository scope

This is a development project and accepts only three kinds of content: `DNA/`, `personality/_template/`, and repository skeleton files (per-layer READMEs and placeholders, root `README*.md`, `LICENSE`, `.gitignore`, `.githooks`).

Everything else stays local: engine code, profile and memory, scenario files, machine configuration, secrets, and the configuration and routing registration the host writes during initialization. Two gates enforce this: `.gitignore` hides out-of-scope files, and `.githooks/pre-commit` refuses the commit (requires `git config core.hooksPath .githooks`). See `DNA/铁律.md`.

Commit convention: definition files count as code and are committed alongside it, prefixed `dna:` `skill:` `rule:` `profile:` `config:` `capability:` `code:` `docs:`.

## License

MIT — see `LICENSE`.
