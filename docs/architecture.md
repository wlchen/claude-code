# Claude Code Repository Architecture

This repository is organized as a Claude Code plugin marketplace: it bundles a small set of root-level commands, a marketplace manifest, reusable examples, and a collection of official plugins that add commands, agents, hooks, skills, and MCP integrations.

## Module overview

- **Root documentation**: high-level project overview and usage guidance in [`README.md`](../README.md)
- **Root commands**: shared commands in [`.claude/commands/`](../.claude/commands/)
- **Marketplace metadata**: bundled plugin registry in [`.claude-plugin/marketplace.json`](../.claude-plugin/marketplace.json)
- **Plugins**: feature-focused extensions in [`plugins/`](../plugins/README.md)
- **Examples**: reference hooks and settings in [`examples/`](../examples/)
- **Scripts**: repository automation in [`scripts/`](../scripts/)

## Repository architecture

```mermaid
flowchart TD
    User[User in Claude Code] --> Root[Repository]

    Root --> Docs[README.md<br/>Project overview]
    Root --> Commands[.claude/commands<br/>Root slash commands]
    Root --> Marketplace[.claude-plugin/marketplace.json<br/>Plugin registry]
    Root --> Plugins[plugins/<br/>Official bundled plugins]
    Root --> Examples[examples/<br/>Hooks and settings samples]
    Root --> Scripts[scripts/<br/>Repository automation]

    Marketplace --> Plugins
    Commands --> User
    Plugins --> User
```

## Plugin ecosystem architecture

The bundled plugins fall into a few broad categories defined by the marketplace manifest:

- **Development**: `agent-sdk-dev`, `claude-opus-4-5-migration`, `feature-dev`, `frontend-design`, `plugin-dev`, `ralph-wiggum`
- **Productivity**: `code-review`, `commit-commands`, `hookify`, `pr-review-toolkit`
- **Learning**: `explanatory-output-style`, `learning-output-style`
- **Security**: `security-guidance`

```mermaid
flowchart LR
    Marketplace[Marketplace manifest] --> Development
    Marketplace --> Productivity
    Marketplace --> Learning
    Marketplace --> Security

    Development --> DevPlugins[Feature-building and migration plugins]
    Productivity --> ProdPlugins[Git, review, and workflow plugins]
    Learning --> LearnPlugins[Session guidance plugins]
    Security --> SecPlugins[Safety reminder plugins]
```

## Standard plugin module structure

Most plugins follow the same internal layout described in [`plugins/README.md`](../plugins/README.md):

```text
plugin-name/
├── .claude-plugin/
│   └── plugin.json
├── commands/
├── agents/
├── skills/
├── hooks/
├── .mcp.json
└── README.md
```

```mermaid
flowchart TD
    Plugin[Plugin directory] --> Metadata[.claude-plugin/plugin.json]
    Plugin --> Readme[README.md]
    Plugin --> Commands[commands/]
    Plugin --> Agents[agents/]
    Plugin --> Skills[skills/]
    Plugin --> Hooks[hooks/]
    Plugin --> MCP[.mcp.json]

    Commands --> Runtime[Claude Code runtime]
    Agents --> Runtime
    Skills --> Runtime
    Hooks --> Runtime
    MCP --> Runtime
```

## Runtime interaction model

```mermaid
sequenceDiagram
    participant U as User
    participant C as Claude Code runtime
    participant M as Marketplace / plugin metadata
    participant P as Selected plugin module
    participant X as Command / agent / hook / skill

    U->>C: Invoke Claude Code or a slash command
    C->>M: Resolve installed/bundled capabilities
    M-->>C: Available plugins and metadata
    C->>P: Select matching plugin
    P->>X: Route to command, agent, hook, or skill
    X-->>C: Produce guidance, automation, or validation
    C-->>U: Return result in the session
```

## Key architectural characteristics

- The repository is **documentation- and plugin-centric**, not a traditional compiled application.
- The **marketplace manifest** is the index that describes the bundled plugin set.
- The **plugins directory** is the main extension surface; each plugin is self-contained and documented.
- **Root-level `.claude/commands`** provide repository-wide commands outside any single plugin.
- **Examples** and **scripts** support adoption and maintenance rather than runtime packaging.
