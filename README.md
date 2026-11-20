# Claude Code Skill Summary

A collection of useful Claude Code skills and plugins, included as git submodules:

| Folder | Source | What you get |
|---|---|---|
| `agent-skills/` | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | 25 engineering workflow skills (TDD, debugging, code review, specs, ...) + 4 subagents |
| `andrej-karpathy-skills/` | [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) | `karpathy-guidelines` skill (avoid common LLM coding mistakes) |
| `ponytail/` | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) | `ponytail` "smallest change that works" skill + audit/review/debt helpers |
| `claude-mem/` | [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | Persistent memory plugin: hooks capture each session, context is injected into the next one |
| `context-mode/` | [mksglu/context-mode](https://github.com/mksglu/context-mode) | Plugin that keeps large tool output out of the context window (local sandbox + SQLite index, MCP tools) |
| `vercel-labs/skills/` | [vercel-labs/skills](https://github.com/vercel-labs/skills) | Source of the `npx skills` CLI used below + `find-skills` skill |
| `claude-howto/` | [luongnv89/claude-howto](https://github.com/luongnv89/claude-howto) | Visual guide with copy-paste templates for commands, memory, skills, subagents, MCP, hooks, plugins (reference, nothing to install) |

## Install

### Quick install with npx (skills only)

Requires [Node.js](https://nodejs.org). The [skills CLI](https://github.com/vercel-labs/skills) (submodule `vercel-labs/skills/`) downloads the skills straight from GitHub, no clone needed:

```bash
npx skills add addyosmani/agent-skills --agent claude-code --global
npx skills add multica-ai/andrej-karpathy-skills --agent claude-code --global
npx skills add DietrichGebert/ponytail --agent claude-code --global --skill '*'
npx skills add vercel-labs/skills --agent claude-code --global --skill find-skills
```

- `--global` installs for your user (all projects); leave it out to install into the current project only.
- Add `--list` to see the skills of a repo first, or `--skill <name>` to install a single one.
- This installs **skills only**, not the subagents. For those, use the manual install below (step 2, last command).
- The CLI sends anonymous usage data; set `DISABLE_TELEMETRY=1` to turn it off.

### claude-mem (plugin)

claude-mem needs hooks and a background worker, so copying files is not enough. Install it with its own installer:

```bash
npx claude-mem install
```

or inside Claude Code:

```
/plugin marketplace add thedotmack/claude-mem
/plugin marketplace add mksglu/context-mode
/plugin install claude-mem
```

Memory is stored locally in `~/.claude-mem`. Anonymous usage analytics are on by default; turn them off with `npx claude-mem telemetry disable`. See the [claude-mem README](claude-mem/README.md) for providers and the optional sign-in.

### context-mode (plugin)

context-mode also brings hooks and an MCP server. Install it as a plugin (needs Node 24.15+; on Windows also Git for Windows):

```bash
claude plugin marketplace add mksglu/context-mode
claude plugin install context-mode@context-mode
```

Its index lives locally in `~/.context-mode`. See the [context-mode README](context-mode/README.md) for other agents and the update/remove commands.

### claude-howto (guide)

Nothing to install. Start with the [claude-howto README](claude-howto/README.md) and copy the templates you want from its numbered folders (e.g. `01-slash-commands/`) into your project's `.claude/`.

### Manual install

#### 1. Clone with submodules

```bash
git clone --recurse-submodules <this-repo-url>
cd CaudeCodeSkillSummary
# already cloned? run:
git submodule update --init --recursive
```

#### 2. Copy skills and agents into Claude Code

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

### Verify

Restart Claude Code, then type `/` — the skills (e.g. `/ponytail`, `/karpathy-guidelines`, `/test-driven-development`) should be listed. Run `/agents` to see the subagents.

### Alternative: install as plugins

Each repo also ships a Claude Code plugin marketplace, which brings hooks and commands too:

```
/plugin marketplace add addyosmani/agent-skills
/plugin marketplace add multica-ai/andrej-karpathy-skills
/plugin marketplace add DietrichGebert/ponytail
/plugin marketplace add thedotmack/claude-mem
```

Then pick the plugins in `/plugin`. Use either plugins **or** the copied files, not both, to avoid duplicate skills.

## Update

```bash
git submodule update --remote --merge
```

Then repeat step 2 (or re-run the `npx skills add` commands).

## License

The files in this repository are licensed under [MIT](LICENSE).
The submodules are separate projects under their own licenses (MIT, except claude-mem under Apache 2.0 and context-mode under Elastic License 2.0, at the time of writing):
[agent-skills](agent-skills/LICENSE) © Addy Osmani,
[ponytail](ponytail/LICENSE) © DietrichGebert,
[andrej-karpathy-skills](andrej-karpathy-skills/README.md#license) © multica-ai,
[claude-mem](claude-mem/LICENSE) © Alex Newman,
[context-mode](context-mode/LICENSE) © Mert Koseoglu,
[skills](vercel-labs/skills/LICENSE) © Vercel, Inc.,
[claude-howto](claude-howto/LICENSE) © luongnv89.
