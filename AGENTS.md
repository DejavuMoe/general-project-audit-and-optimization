# Working with this skill

## Installing for a user

Start in the consuming project. The source is `DejavuMoe/general-project-audit-and-optimization` on GitHub, with the skill at the root of `master`. When a user specifies an exact revision, download its source archive and install the root `SKILL.md`, `references/` and `examples/` from that revision. For the default branch, use one of the commands below. For installation through a file manager, see the [project installation instructions](README.md#install-in-a-project).

Verify the installed frontmatter name and the relative guides and example it links to. Report the installed path, source revision when available, and the host's invocation for the skill followed by `audit`. For running an audit, use the [skill instructions](SKILL.md).

## Install from master

Run one block in the target project's root. Each downloads the GitHub source archive and extracts the skill's three runtime entries into the project. Running it again refreshes those files.

**PowerShell with tar**

```powershell
$skill = 'general-project-audit-and-optimization'
$archive = New-TemporaryFile
try {
    Invoke-WebRequest "https://github.com/DejavuMoe/$skill/archive/refs/heads/master.tar.gz" -OutFile $archive.FullName -ErrorAction Stop
    $dst = New-Item -ItemType Directory -Force ".agents/skills/$skill" -ErrorAction Stop
    tar -xzf $archive.FullName -C $dst.FullName --strip-components=1 "${skill}-master/SKILL.md" "${skill}-master/references" "${skill}-master/examples"
    if ($LASTEXITCODE -ne 0) { throw 'Skill extraction failed' }
} finally {
    Remove-Item -LiteralPath $archive.FullName
}
```

**POSIX shell with curl and tar**

```sh
(
  set -eu
  skill=general-project-audit-and-optimization
  dst=".agents/skills/$skill"
  archive=$(mktemp)
  trap 'rm -f "$archive"' EXIT
  curl -fsSL "https://github.com/DejavuMoe/$skill/archive/refs/heads/master.tar.gz" -o "$archive"
  mkdir -p "$dst"
  tar -xzf "$archive" -C "$dst" --strip-components=1 \
    "${skill}-master/SKILL.md" "${skill}-master/references" "${skill}-master/examples"
)
```

## Maintaining the repository

Update [README.md](README.md) (English) and [README.zh-CN.md](README.zh-CN.md) (Simplified Chinese) together in the same change. When skill behavior changes, update the affected guidance in both editions.

Before finishing a README change, compare both editions with the [skill](SKILL.md). Keep the project install path, chat commands, quick replies, examples, status meanings and constraints consistent. Show the client selecting `general-project-audit-and-optimization` before the command. This repository root is the skill's development directory. The project install path belongs to the consuming project. Keep `SKILL.md` and the reference guides in English and verify links in both editions.
