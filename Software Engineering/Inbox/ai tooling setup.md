---
origin:
  - work
  - personal
---

# AI Tooling Setup

The AI tooling I run around Claude Code, and the commands to set it up again on another machine. Claude Code itself is listed among my development tools in [[tools & libraries]].

Commands are equivalents rebuilt from the configuration on my Windows machine, not a record of what I originally typed. Paths are placeholders. Nothing here needs a secret. If a command ever does, write a placeholder such as `<API_KEY>`, never the real value.

## Prerequisites

| Tool | Needed for |
| --- | --- |
| Node.js (`npx`) | MCP servers started with `npx` |
| Bun | opencode |
| Dart SDK (ships with Flutter) | `dart mcp-server`, `patrol_mcp` |
| Scoop | The supporting command-line tools |

## CLI tools

- **Claude Code** — native install, binary at `~/.local/bin/claude`.
- **opencode** — global Bun package `opencode-ai`.

```powershell
irm https://claude.ai/install.ps1 | iex
bun add -g opencode-ai
```

The first line is the documented Windows installer for Claude Code. It is not confirmed as the command I ran.

## MCP servers

Scope decides where a server loads: `user` in every project, `local` (the default) only in the project it was added from.

| Server | Scope | Purpose |
| --- | --- | --- |
| `filesystem` | user | File access limited to one folder, here the vault |
| `codebase-memory-mcp` | user | Code knowledge graph: symbol search and call tracing |
| `dart` | local, per Flutter project | Dart and Flutter tooling: analysis, hot reload, pub, widget inspector |
| `next-devtools` | local | Next.js documentation and dev-server tooling |
| `playwright` | local | Browser automation |
| `patrol` | local, one Flutter project | Patrol UI-test integration |

```bash
claude mcp add --scope user filesystem -- npx -y @modelcontextprotocol/server-filesystem "<vault-path>"
claude mcp add dart -- dart mcp-server
claude mcp add next-devtools -- npx next-devtools-mcp@latest
claude mcp add playwright -- npx @playwright/mcp@latest
claude mcp add patrol -e PROJECT_ROOT=<flutter-project> -e PATROL_FLAGS=<value> -e SHOW_TERMINAL=<value> -e PATROL_FLUTTER_COMMAND=<value> -- dart run patrol_mcp
```

`codebase-memory-mcp` is set up by its own installer instead:

```bash
codebase-memory-mcp install --dry-run
codebase-memory-mcp install
```

One run writes the MCP entry, three agents (`codebase-memory`, `codebase-memory-scout`, `codebase-memory-auditor`), and its session hooks (`cbm-*.cmd`). It also configures the other AI clients it detects on the machine. `--dry-run` previews all of that without writing. How the binary itself was obtained is not recorded.

## Plugins

All four are installed at user scope.

| Plugin | Marketplace source | Adds |
| --- | --- | --- |
| `caveman@caveman` | `JuliusBrussee/caveman` | Terse-output mode: 5 skills, 2 hooks |
| `dx@ykdojo` | `ykdojo/claude-code-tips` | 6 skills: conversation clone and handoff, GitHub Actions debugging, Reddit fetch, CLAUDE.md review |
| `flutterflow@flutterflow` | `https://github.com/FlutterFlow/flutterflow-claude.git` | 1 skill for building with the FlutterFlow CLI, 1 hook |
| `dart-flutter@dart-flutter` | `flutter/agent-plugins` | 22 Dart and Flutter skills, plus the Dart MCP server |

```bash
claude plugin marketplace add JuliusBrussee/caveman
claude plugin install caveman@caveman

claude plugin marketplace add ykdojo/claude-code-tips
claude plugin install dx@ykdojo

claude plugin marketplace add https://github.com/FlutterFlow/flutterflow-claude.git
claude plugin install flutterflow@flutterflow

claude plugin marketplace add flutter/agent-plugins
claude plugin install dart-flutter@dart-flutter
```

`claude plugin details <plugin>` lists what a plugin bundles and its token cost. `claude plugin list` shows scope and status.

The `kepano/obsidian-skills` marketplace is registered too, with no plugin installed from it.

## Skills and agents

There is no install command. A skill is a folder holding a `SKILL.md`, an agent is a single `.md` file. User-level ones live in `~/.claude/skills/` and `~/.claude/agents/`, project-level ones in the repository's `.claude/`.

User-level skills I wrote:

| Skill | Purpose |
| --- | --- |
| `flutter-page-scaffolder` | Scaffold a feature page following the Bloc structure |
| `flutter-page-localizer` | Move a page's hardcoded strings into the ARB files |
| `flutter-arb-sorter` | Sort ARB localization files and validate them |
| `flutter-constants-sorter` | Sort a Dart constants file |
| `review-l10n` | Review and standardize ARB keys across locales |
| `sort-alphabetically` | Sort the contents of a file |

User-level items I did not write:

- `codebase-memory` skill and the three `codebase-memory*` agents — belong to `codebase-memory-mcp`.
- `frontend-design` skill — third-party, ships its own license file. Source not recorded.

In this vault's `.claude/`: six Obsidian skills from `kepano/obsidian-skills` (`obsidian-markdown`, `obsidian-bases`, `obsidian-cli`, `json-canvas`, `defuddle`, `knap`), the `knowledge-assistant` agent, and the `organize-notes` command. How the Obsidian skills were copied in is not recorded.

## Provided by the claude.ai account

Nothing to install locally. These load once Claude Code is signed in.

- Synced plugins: `design`, `engineering`, `operations`, `cowork-plugin-management`.
- Synced skills: document formats (`docx`, `pdf`, `pptx`, `xlsx`), `deep-research`, `skill-creator`, and others.
- Connectors in use: ClickUp, Postman, Claude Docs. Google Drive and LogRocket are listed but not authenticated.

Browser control comes from the Claude in Chrome extension, which is installed in the browser, not through the CLI.

## Supporting command-line tools

Not AI tooling. General tools that sit beside the agents.

| Tool | What it is |
| --- | --- |
| `psmux` | Terminal multiplexer for Windows, for running several sessions side by side |
| `ast-grep` | Structural code search and rewrite |
| `difftastic` | Syntax-aware diff |
| `jq`, `yq` | JSON and YAML processors |
| `scc`, `tokei` | Code line counters |
| `shellcheck` | Shell script linter |
| `gh` | GitHub CLI, installed with its own installer |

```powershell
scoop install psmux ast-grep difftastic jq yq scc tokei shellcheck
```

## Observations

- The `dart` server is added at local scope in four projects, while the `dart-flutter` plugin at user scope already bundles the Dart MCP server. The per-project entries are likely redundant.
- `@playwright/mcp` is installed globally through npm, but the server entry runs `npx @playwright/mcp@latest`, so the global copy is likely not the one that runs.
- Three servers (`next-devtools`, `dart`, `playwright`) are registered at local scope for the drive root, so they load only when Claude Code starts from there.
- Not yet verified: that these commands reproduce the setup on a clean machine.
