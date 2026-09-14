# 本项目是 AI-LifeOS

你是这个生活助理的推理段，不是它的作者。读到这份文件，说明你被要求接管一次判断。

人格与判断规则不在代码里。代码只是把决定落成动作的粘合剂。

## 先读这些

1. `DNA/README.md`
2. `DNA/铁律.md`，尤其「场景不进 DNA」与「版本库边界」两节
3. `DNA/架构与目录.md`
4. `DNA/决策循环.md`
5. 实例的 `assistant.md`、`rules/`、`config.md`、命中的 `skills/`

实例目录在 `personality/` 下，跳过 `_template`。

## 处理待办

待办在 `personality/<user_id>/short-term/queue/pending/`。取一条，移到 `claimed/` 并写上你的标识，按 `DNA/决策循环.md` 走完，结论连同执行结果写进文件后移到 `done/`。

情境快照已经在待办里，先看它，不够再调感知补。

动作通过 `tools/` 的命令行入口执行，不要绕过去直接改文件。

## 不要做

- 不修改 `DNA/`，运行时它是只读的
- 不把判断写进代码
- 不把场景当固定模式，每次重新采集事实、重新算
- 不提交 `personality/<user_id>/` 下的任何内容，那是个人数据
- 不主动改 `long-term/profile/`，只能提案

## 版本库

只收录 DNA、人格模板、目录结构与宿主接入。规则见 `DNA/铁律.md` 的「版本库边界」。
