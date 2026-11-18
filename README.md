# general-project-audit-and-optimization

English | [简体中文](README.zh-CN.md)

Audit an existing project, choose fixes in one reply, and verify the selected work against its acceptance criteria.

## Start an audit

Select the **`general-project-audit-and-optimization`** skill, then enter the `audit` command. In Codex, send this message to audit the current project.

```text
$general-project-audit-and-optimization audit
```

In Claude Code, use `/general-project-audit-and-optimization audit`. With a skill picker, select the same name and enter `audit`. A local path or GitHub URL at the end selects a different target. For example, `$general-project-audit-and-optimization audit reliability,ci ../service` audits reliability and CI time in a sibling project. The agent gives you one prioritized report with evidence, options and acceptance criteria, then waits for your selection before editing.

## Install in a project

1. Download and extract the source ZIP from the default branch. [Download master ZIP](https://github.com/DejavuMoe/general-project-audit-and-optimization/archive/refs/heads/master.zip)
2. In the **target project**, create `.agents/skills/general-project-audit-and-optimization/`. Copy `SKILL.md`, `references/` and `examples/` from the extracted folder directly into that directory. A local source checkout has the same layout.
3. Open the target project in your agent and select the skill by its full name. Its installed entry point is `.agents/skills/general-project-audit-and-optimization/SKILL.md`.

For a terminal install, run the commands for your shell from the target project's root. To pin a version, take the same three entries from a specific commit's source archive. [PowerShell and POSIX shell commands](AGENTS.md#install-from-master)

**To delegate installation to an agent**, paste this into the target project's chat.

```text
Install https://github.com/DejavuMoe/general-project-audit-and-optimization from master in this project's .agents/skills/ directory. Keep the original skill name, verify SKILL.md and its bundled guides and examples, and show me how to start an audit in this client.
```

Codex and Antigravity discover skills in the project's `.agents/skills/`. For Claude Code, link `.claude/skills/general-project-audit-and-optimization` in that project to the installed skill folder. Other clients with file access can open the installed `SKILL.md` directly. [Codex](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills) | [Antigravity](https://antigravity.google/docs/skills) | [Claude Code](https://code.claude.com/docs/en/skills)

## Reply to the audit

Each numbered row has a stable finding ID, a recommended action, its cost and a way to verify the result. The report shows exactly which actions `A` will authorize. Reply once in the same chat.

| Reply | What happens |
| --- | --- |
| `A` | Run the displayed recommended bundle. |
| `1 3` | Run the recommendations for rows 1 and 3 only. |
| `1 3 T2 X4` | Follow rows 1 and 3, run row 2 as a bounded trial and reject row 4. `D4` would defer row 4. |
| `D` | Defer all open choices. |

Unmentioned items stay pending. The agent asks once if a choice is ambiguous. At final acceptance, `Y` signs off only the listed passing results and their tested revisions. Use `return F3 <reason>` to reopen one.

## Commands

The table lists what to type **after the skill name**. For example, `$general-project-audit-and-optimization ci` starts a CI audit in Codex. Topics accept an optional target and can be combined with a comma. In the same chat, replies to a displayed menu need no prefix. Natural-language requests work too.

| After the skill name | Action |
| --- | --- |
| `audit` | Audit the project's user flow, supported behavior, baseline checks and delivery. |
| `tests` / `security` / `perf` / `reliability` | Inspect coverage and suspected legacy code, security, measured speed and resource use, or failure recovery. |
| `stack` | Compare supported upgrades, replacement tools and mise against the current stack. [Toolchains](references/toolchains.md) |
| `data` / `rewrite rust` | Evaluate queries, drivers, ORMs and databases, or assess a rewrite in the named language. [Data and rewrites](references/data-and-rewrites.md) |
| `ui` | Review implicit states, copy, visual hierarchy and accessibility for users, developers and agents. [Interface review](references/interface.md) |
| `ci` / `agents` | Evaluate pipeline time and test selection, or compare self-hosted agent options. [Continuous integration](references/ci.md) |
| `fix F1` / `trial F2` | Implement a selected fix or run a bounded experiment. |
| `verify F3` / `review F3 <SHA>` | Replay a feature or request an independent run on a specific revision. [Feature acceptance](references/feature-acceptance.md) |
| `accept F3` / `return F3 <reason>` | Sign off a specific result or return it and recheck the same finding after revision. |
| `split` / `handoff` / `help` | Split selected work when it can ship separately, hand it to another executor, or display the available commands. |

Checks report `PASS` when the criterion is met, `FAIL` when an executed check misses it, `NOT_RUN` for an unexecuted check and `BLOCKED` for a missing prerequisite. The Sani example follows one finding from a real browser failure through repair and independent review. [Worked example](examples/sani-search.md)

New implementations and tools exclude PHP, Java, Next.js, Deno and dependencies tied to Vercel or Cloudflare. The audit identifies existing consumers and the cost of a portable migration. The skill and guides are in English. The agent responds in your language. [Skill instructions](SKILL.md)

Maintain both README editions in the same change. [Project instructions](AGENTS.md)
