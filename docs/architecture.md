# Claude Code Repository Architecture

This repository is primarily a **plugin catalog and automation workspace** for Claude Code. It combines:

- bundled plugins under `plugins/`
- shared slash commands under `.claude/commands/`
- GitHub automation scripts under `scripts/`
- GitHub Actions workflows under `.github/workflows/`
- example settings under `examples/settings/`

## Module map

| Module | Purpose | Key files |
| --- | --- | --- |
| Root documentation | Entry points and usage guidance | `README.md`, `SECURITY.md`, `CHANGELOG.md` |
| Plugin marketplace | Registry of bundled plugins and metadata | `.claude-plugin/marketplace.json` |
| Shared commands | Reusable Claude Code slash commands for repo workflows | `.claude/commands/*.md` |
| Plugins | Feature-specific commands, agents, hooks, and skills | `plugins/*` |
| GitHub automation scripts | Bun/TypeScript scripts for issue lifecycle and duplicate handling | `scripts/*.ts`, `scripts/*.sh` |
| GitHub workflows | Event-driven automation invoking Claude Code or scripts | `.github/workflows/*.yml` |
| Settings examples | Sample org/project Claude Code settings | `examples/settings/*` |

## Repository overview

```mermaid
flowchart TD
    A[README.md] --> B[Plugin marketplace<br/>.claude-plugin/marketplace.json]
    B --> C[plugins/*]
    A --> D[Shared commands<br/>.claude/commands/*]
    A --> E[Settings examples<br/>examples/settings/*]
    F[GitHub workflows<br/>.github/workflows/*] --> G[Claude Code GitHub Action]
    F --> H[Bun / shell automation scripts<br/>scripts/*]
    H --> I[GitHub Issues / PRs]
    D --> I
    C --> J[Commands / Agents / Hooks / Skills]
```

## Plugin architecture

Each bundled plugin follows the same Claude Code plugin shape.

```mermaid
flowchart TD
    A[plugins/plugin-name] --> B[.claude-plugin/plugin.json]
    A --> C[README.md]
    A --> D[commands/]
    A --> E[agents/]
    A --> F[skills/]
    A --> G[hooks/]
    A --> H[.mcp.json]

    B --> I[Plugin metadata]
    D --> J[Slash commands]
    E --> K[Specialized agents]
    F --> L[Reusable skills]
    G --> M[Lifecycle hooks]
    H --> N[External tool integration]
```

## Automation flow

The repository's runtime behavior is driven mostly by GitHub events.

```mermaid
sequenceDiagram
    participant User as GitHub user
    participant GH as GitHub event
    participant WF as GitHub workflow
    participant CC as Claude Code action
    participant Scripts as Bun/shell scripts
    participant API as GitHub API

    User->>GH: opens issue / comments / requests review
    GH->>WF: triggers .github/workflows/*.yml
    alt Claude-assisted workflow
        WF->>CC: run anthropics/claude-code-action
        CC->>API: read/update issues or PR context
    else Scripted automation
        WF->>Scripts: run bun scripts/*.ts or scripts/*.sh
        Scripts->>API: post comments, labels, close duplicates
    end
    API-->>User: updated issue or PR state
```

## Key architectural relationships

1. **Marketplace-first packaging**
   `.claude-plugin/marketplace.json` is the catalog entrypoint that points to the bundled plugins in `plugins/`.

2. **Plugin implementation is self-contained**
   Each plugin owns its commands, agents, skills, hooks, and documentation inside its own directory.

3. **Repository automation is workflow-driven**
   `.github/workflows/` provides the orchestration layer; the workflows either invoke Claude Code directly or run scripts from `scripts/`.

4. **Scripts act on GitHub state**
   The TypeScript and shell scripts are thin automation layers around GitHub issues, comments, labels, and pull requests.

5. **Examples document deployment patterns**
   `examples/settings/` shows how teams can configure Claude Code behavior across environments without changing plugin code.
