# 宿主：TraeCode CLI

TraeCode CLI 支持非交互运行，可以直接被脚本调用，因此它能充当无人值守时的推理段。桌面端与网页端不在此列——它们需要人打开才能工作，对应的是拉取模式。

| 项 | 值 |
|---|---|
| 名称 | trae |
| 形态 | TraeCode CLI，非交互模式 |
| 唤醒方式 | 命令行 |
| 唤醒命令 | `traecli -p "/decision-loop {id}" --allowed-tool Bash,Edit,Write --query-timeout 5m` |
| 在线判定 | 命令存在，且 `TRAECLI_PERSONAL_ACCESS_TOKEN` 在实例 `long-term/secrets.env` 里已配置 |
| 工具通路 | 通过 Bash 调用 `tools/` 暴露的命令行入口 |
| 人格加载 | 仓库根 `AGENTS.md`，以及项目级 `.trae/skills/lifeos-persona/` |
| 接线位置 | 仓库根 `AGENTS.md`、项目级 `.trae/skills/` 与 `.trae/commands/` |
| 领取超时 | 10 分钟 |

## 占位符

`{id}` 是待办 id，由触发器填充。触发器不需要知道 `traecli` 是什么，它只按模板拼命令。

## 为什么用命令行，而不是它的定时任务

TraeCode 自带定时任务，但那是 cron 式，粒度以分钟计，且依赖应用处于运行状态。时机判断留在触发器，宿主只负责推理，各管一段。

## 密钥

`TRAECLI_PERSONAL_ACCESS_TOKEN` 属于密钥，写实例的 `long-term/secrets.env`，不入库。企业自建域名另配 `TRAECLI_HOST`。

## 放开哪些工具

`--allowed-tool` 只放行推理确实需要的几个。工具名以 `traecli --help` 的当期输出为准。

不要用 `-y`。它跳过全部权限检查，等于把一个能被提示词影响的模型直接接到你的机器上。

## 拉取模式

人在 TraeCode 里打开本项目时，说一句「看队列」，或直接键入 `/decision-loop <id>`，即可处理积压。这条路径与无人值守共用同一份队列与同一份对话流，只是换了触发者。

## 相关文档

- 命令行参数与使用场景：https://docs.trae.cn/cli/use-cases
- 登录令牌：https://docs.trae.cn/cli/login-token
- 技能：https://docs.trae.cn/ide/skills
- 命令：https://docs.trae.cn/ide/slash-commands
- MCP：https://docs.trae.cn/cli/model-context-protocol
