# Claude Code Skill Summary

A collection of useful Claude Code skills, included as git submodules:

| Folder | Source | What you get |
|---|---|---|
| `agent-skills/` | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | 25 engineering workflow skills (TDD, debugging, code review, specs, ...) + 4 subagents |
| `andrej-karpathy-skills/` | [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) | `karpathy-guidelines` skill (avoid common LLM coding mistakes) |
| `ponytail/` | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) | `ponytail` "smallest change that works" skill + audit/review/debt helpers |

## Install

### 1. Clone with submodules

```bash
git clone --recurse-submodules <this-repo-url>
cd CaudeCodeSkillSummary
# already cloned? run:
git submodule update --init --recursive
```

### 2. Copy skills and agents into Claude Code

Skills go to `~/.claude/skills/`, subagents to `~/.claude/agents/`.
They are then available in every project.

**macOS / Linux / Git Bash:**

```bash
mkdir -p ~/.claude/skills ~/.claude/agents
cp -r agent-skills/skills/* andrej-karpathy-skills/skills/* ~/.claude/skills/
cp -r ponytail/skills/*/ ~/.claude/skills/
cp agent-skills/agents/*.md ~/.claude/agents/
```

**Windows (PowerShell):**

```powershell
New-Item -ItemType Directory -Force "$HOME\.claude\skills", "$HOME\.claude\agents" | Out-Null
Copy-Item -Recurse -Force agent-skills\skills\*, andrej-karpathy-skills\skills\* "$HOME\.claude\skills\"
Get-ChildItem ponytail\skills -Directory | Copy-Item -Recurse -Force -Destination "$HOME\.claude\skills\"
Copy-Item -Force agent-skills\agents\*.md "$HOME\.claude\agents\"
```

> Per-project instead of global? Copy into `<your-project>/.claude/skills/` and `.claude/agents/`.

### 3. Verify

Restart Claude Code, then type `/` — the skills (e.g. `/ponytail`, `/karpathy-guidelines`, `/test-driven-development`) should be listed. Run `/agents` to see the subagents.

### Alternative: install as plugins

Each repo also ships a Claude Code plugin marketplace, which brings hooks and commands too:

```
/plugin marketplace add addyosmani/agent-skills
/plugin marketplace add multica-ai/andrej-karpathy-skills
/plugin marketplace add DietrichGebert/ponytail
```

Then pick the plugins in `/plugin`. Use either plugins **or** the copied files, not both, to avoid duplicate skills.

## Update

```bash
git submodule update --remote --merge
```

Then repeat step 2.

## License

The files in this repository are licensed under [MIT](LICENSE).
The submodules are separate projects under their own licenses (all MIT at the time of writing):
[agent-skills](agent-skills/LICENSE) © Addy Osmani,
[ponytail](ponytail/LICENSE) © DietrichGebert,
[andrej-karpathy-skills](andrej-karpathy-skills/README.md#license) © multica-ai.
