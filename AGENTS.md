# Working with this skill

## Installing for a user

Start in the consuming project. The source is `DejavuMoe/general-project-audit-and-optimization` on GitHub, with the skill at the root of `master`. Follow the [project installation instructions](README.md#install-in-a-project). When a user specifies a branch or commit, resolve that ref and install its contents. For a manual install, copy the root `SKILL.md`, `references/` and `examples/` into `.agents/skills/general-project-audit-and-optimization/` in the consuming project.

Verify the installed frontmatter name and the relative guides and example it links to. Report the installed path, source revision when available, and the host's invocation for the skill followed by `audit`. For running an audit, use the [skill instructions](SKILL.md).

## Maintaining the repository

Update [README.md](README.md) (English) and [README.zh-CN.md](README.zh-CN.md) (Simplified Chinese) together in the same change. When skill behavior changes, update the affected guidance in both editions.

Before finishing a README change, compare both editions with the [skill](SKILL.md). Keep the project install path, chat commands, quick replies, examples, status meanings and constraints consistent. Show the client selecting `general-project-audit-and-optimization` before the command. This repository root is the skill's development directory. The project install path belongs to the consuming project. Keep `SKILL.md` and the reference guides in English and verify links in both editions.
