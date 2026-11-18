# general-project-audit-and-optimization

[English](README.md) | 简体中文

审计现有工程，用一条回复选择要修的问题，再按约定的条件验证结果。

## 开始审计

选中 **`general-project-audit-and-optimization`** 技能，再输入子命令 `audit`。在 Codex 中，审计当前项目的完整输入如下。

```text
$general-project-audit-and-optimization audit
```

在 Claude Code 中输入 `/general-project-audit-and-optimization audit`。有技能选择器的客户端，选中同名技能后输入 `audit` 即可。在命令末尾加本地路径或 GitHub 地址可指定其他项目。例如，`$general-project-audit-and-optimization audit reliability,ci ../service` 会检查同级项目的可靠性和 CI 耗时。Agent 会一次给出按优先级排列的问题、证据、选项和验收条件，等你选定后再修改。

## 安装到项目

在**目标项目的 Git 根目录**运行下面这条命令。需要支持 `gh skill install` 的 GitHub CLI，安装位置是该项目内的 `.agents/skills/general-project-audit-and-optimization/`。

```sh
gh skill install DejavuMoe/general-project-audit-and-optimization general-project-audit-and-optimization --agent universal --scope project
```

GitHub CLI 优先选用最新的带标签发布版本，其次取默认分支 `master`。需要固定版本时，在命令末尾加 `--pin <提交 SHA>`。手动安装可下载并解压 `master` 压缩包，将根目录的 `SKILL.md`、`references/` 和 `examples/` 复制到上述项目目录。已有本地源码时，可从技能开发目录复制同样的文件。[GitHub CLI 安装选项](https://cli.github.com/manual/gh_skill_install) | [下载 master 压缩包](https://github.com/DejavuMoe/general-project-audit-and-optimization/archive/refs/heads/master.zip)

**让 Agent 代为安装**时，可以在目标项目的对话里粘贴这句话。

```text
请从 https://github.com/DejavuMoe/general-project-audit-and-optimization 的 master 分支安装这个技能到本项目的 .agents/skills/，保留原技能名。安装后核对 SKILL.md、配套指南和示例，并告诉我在当前客户端如何启动审计。
```

Codex 和 Antigravity 都能从项目的 `.agents/skills/` 发现技能。使用 Claude Code 时，可把项目内的 `.claude/skills/general-project-audit-and-optimization` 链接到这份已安装的目录。有文件读取能力的其他客户端可以直接打开安装后的 `SKILL.md`。[Codex](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills) | [Antigravity](https://antigravity.google/docs/skills) | [Claude Code](https://code.claude.com/docs/en/skills)

## 审计后怎么选

报告里的每一行都有编号、固定的问题 ID、建议动作、成本和验证方法。报告末尾会列出 `A` 对应的具体操作。在同一对话里回复一次即可。

| 回复 | 会做什么 |
| --- | --- |
| `A` | 执行报告列出的推荐组合。 |
| `1 3` | 只执行第 1、3 项各自列出的建议。 |
| `1 3 T2 X4` | 执行第 1、3 项的建议，第 2 项只做试验，拒绝第 4 项。改用 `D4` 可暂缓第 4 项。 |
| `D` | 暂缓所有待选项。 |

没选的项保持待定，有歧义时 Agent 会集中澄清一次。到最终验收时，回复 `Y` 只签收列出的通过项及其已测版本。需要退回某项时，输入 `return F3 <原因>`。

## 命令

下表只列**跟在技能名后面**的部分。例如，在 Codex 中输入 `$general-project-audit-and-optimization ci` 就会开始 CI 专项审计。命令末尾可加目标，多个方向用逗号组合。同一对话里回答已显示的菜单时，可以直接回复字母或编号。也可以使用自然语言。

| 技能名后输入 | 作用 |
| --- | --- |
| `audit` | 从用户流程入手，审计工程应保持的行为、现有检查和交付流程。 |
| `tests` / `security` / `perf` / `reliability` | 分别检查测试覆盖和疑似废弃代码、安全、实测性能与资源占用，或故障恢复。 |
| `stack` | 对照现有工具链，评估受支持的升级、替代工具和 mise。[工具链选型](references/toolchains.md) |
| `data` / `rewrite rust` | 评估查询、驱动、ORM 和数据库，或评估使用指定语言重写的可行性。[数据与重写](references/data-and-rewrites.md) |
| `ui` | 审查隐式状态、文案、视觉层级与无障碍，说明对用户、开发者和 Agent 的影响。[界面审查](references/interface.md) |
| `ci` / `agents` | 评估流水线耗时与选测策略，或比较可自托管的 Agent 方案。[持续集成](references/ci.md) |
| `fix F1` / `trial F2` | 实施已选修复，或在约定范围内试验。 |
| `verify F3` / `review F3 <SHA>` | 复跑功能验收，或让独立执行者在指定版本上复核。[功能验收](references/feature-acceptance.md) |
| `accept F3` / `return F3 <原因>` | 签收指定结果，或退回该项，在修订后沿用原 ID 重验。 |
| `split` / `handoff` / `help` | 在改动能独立交付时拆分 PR，按需交接，或查看可用命令。 |

检查状态用 `PASS` 表示达标，`FAIL` 表示已执行但未达标，`NOT_RUN` 表示未执行，`BLOCKED` 表示缺少前置条件。Sani 实例记录了同一项功能如何从真实浏览器复现失败，到修复后通过独立复核。[查看案例](examples/sani-search.md)

新增实现和工具排除 PHP、Java、Next.js、Deno，以及绑定 Vercel 或 Cloudflare 的依赖。项目已有这些技术时，会查清依赖方和迁移成本。技能正文与指南使用英文，Agent 使用你的提问语言交付结果。[技能正文](SKILL.md)

维护时在同一次改动中同步更新两份 README。[项目维护约定](AGENTS.md)
