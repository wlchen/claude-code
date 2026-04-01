# Claude Code Repository Architecture

This repository is organized as a **plugin marketplace for Claude Code**. Most of the implementation lives in self-contained plugins, while the root of the repository provides marketplace metadata, documentation, automation scripts, and GitHub workflows.

## Primary modules

| Path | Role |
| --- | --- |
| `/README.md` | Top-level product and repository overview |
| `/.claude-plugin/marketplace.json` | Registry of bundled plugins and their categories |
| `/plugins/` | 13 bundled plugins, each with its own commands, agents, skills, hooks, and README |
| `/scripts/` | TypeScript and shell automation used by repository workflows |
| `/.github/workflows/` | CI and issue/PR automation workflows |

## Repository architecture

```mermaid
flowchart TD
    Repo["claude-code repository"] --> RootDocs["README.md"]
    Repo --> Registry[".claude-plugin/marketplace.json"]
    Repo --> Plugins["plugins/"]
    Repo --> Scripts["scripts/"]
    Repo --> Workflows[".github/workflows/"]

    Registry --> PluginEntries["Plugin entries + categories"]

    Plugins --> PluginA["Plugin A"]
    Plugins --> PluginB["Plugin B"]
    Plugins --> PluginN["... 13 bundled plugins"]

    Workflows --> Scripts
    Workflows --> Registry
    RootDocs --> Plugins
```

## Plugin anatomy

All bundled plugins follow the same high-level structure documented in `plugins/README.md`.

```mermaid
flowchart TD
    Plugin["plugins/<plugin-name>/"] --> Manifest[".claude-plugin/plugin.json"]
    Plugin --> Readme["README.md"]
    Plugin --> Commands["commands/"]
    Plugin --> Agents["agents/"]
    Plugin --> Skills["skills/"]
    Plugin --> Hooks["hooks/"]
    Plugin --> MCP[".mcp.json (optional)"]
    Plugin --> Support["core/ utils/ hooks-handlers/ (plugin-specific support code)"]
```

## Runtime interaction model

Claude Code composes plugin features from a few reusable building blocks:

- **Commands** provide user-facing workflows.
- **Agents** perform specialized analysis or implementation in parallel.
- **Skills** inject reusable knowledge or instructions.
- **Hooks** run on lifecycle events such as `SessionStart`, `PreToolUse`, `Stop`, or similar events.

```mermaid
flowchart LR
    User["User invokes Claude Code"] --> Command["Plugin command or built-in workflow"]
    Command --> Agent["Specialized agents"]
    Command --> Skill["Relevant skills"]
    Command --> Hook["Lifecycle hooks"]
    Agent --> Output["Analysis / implementation guidance"]
    Skill --> Output
    Hook --> Output
```

## Representative plugin groups

| Group | Examples | Responsibility |
| --- | --- | --- |
| Development workflows | `feature-dev`, `plugin-dev`, `agent-sdk-dev`, `ralph-wiggum` | Guide feature delivery, plugin authoring, SDK work, and iterative task loops |
| Review and quality | `code-review`, `pr-review-toolkit`, `security-guidance` | Review pull requests, analyze risk, and warn about unsafe changes |
| Productivity | `commit-commands`, `hookify` | Automate git flows and configurable safety/productivity rules |
| Learning and UX | `frontend-design`, `explanatory-output-style`, `learning-output-style`, `claude-opus-4-5-migration` | Add design guidance, educational context, learning-oriented behavior, and migration help |

## Example workflow: command-driven feature delivery

The `feature-dev` plugin is a good example of how the parts fit together.

```mermaid
sequenceDiagram
    participant User
    participant Command as /feature-dev command
    participant Explorers as code-explorer agents
    participant Architects as code-architect agents
    participant Reviewers as code-reviewer agents

    User->>Command: Request a new feature
    Command->>Explorers: Explore related code and patterns
    Explorers-->>Command: Findings and key files
    Command->>User: Clarifying questions
    User-->>Command: Answers / constraints
    Command->>Architects: Propose implementation approaches
    Architects-->>Command: Architecture options
    Command->>User: Recommendation + approval request
    User-->>Command: Approve chosen approach
    Command->>Reviewers: Review completed implementation
    Reviewers-->>Command: Quality findings
    Command-->>User: Final summary and next steps
```

## Key architectural takeaways

1. The repository is **plugin-first**: functionality is grouped into independently documented plugins under `/plugins/`.
2. The marketplace file is the **catalog and discovery layer** for those plugins.
3. GitHub workflows and repository scripts provide **operational automation** around issues, triage, and maintenance.
4. The core abstraction used across plugins is a combination of **commands, agents, skills, and hooks**.
