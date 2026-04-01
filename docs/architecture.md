# Claude Code Repository Architecture

This repository is organized as a documentation- and plugin-centric codebase for Claude Code. The main deliverables are bundled plugins, supporting examples, and GitHub automation used to maintain the repository itself.

## Top-level modules

| Module | Purpose |
| --- | --- |
| [`README.md`](../README.md) | Entry point for product-level documentation and plugin discovery. |
| [`plugins/`](../plugins/README.md) | Bundled Claude Code plugins, each with its own commands, agents, skills, hooks, and docs. |
| [`.claude/`](../.claude/) | Repository-local Claude commands and settings used while working in this repo. |
| [`.claude-plugin/`](../.claude-plugin/marketplace.json) | Marketplace metadata describing the bundled plugin catalog. |
| [`examples/`](../examples/) | Example hook scripts and settings for plugin authors. |
| [`scripts/`](../scripts/) | Maintenance utilities used by repository automation. |
| [`.github/workflows/`](../.github/workflows/) | GitHub Actions workflows for issue triage, dedupe, sweeping, and repo maintenance. |

## Architecture overview

```mermaid
flowchart TD
    A[README.md<br/>top-level documentation]
    B[plugins/<br/>bundled plugin modules]
    C[examples/<br/>reference configs & hooks]
    D[.claude/<br/>repo-local commands]
    E[.claude-plugin/marketplace.json<br/>plugin catalog metadata]
    F[scripts/<br/>maintenance utilities]
    G[.github/workflows/<br/>automation pipelines]

    A --> B
    A --> C
    D --> B
    E --> B
    G --> F
    G --> D
```

## Plugin system architecture

The `plugins/` directory is the functional center of the repository. Each plugin is a self-contained module that follows the same structure and can contribute different Claude Code capabilities.

```mermaid
flowchart LR
    M[.claude-plugin/marketplace.json] --> P[plugins/<br/>official plugin catalog]

    P --> P1[development plugins]
    P --> P2[productivity plugins]
    P --> P3[learning/style plugins]
    P --> P4[security plugins]

    P1 --> S[standard plugin layout]
    P2 --> S
    P3 --> S
    P4 --> S

    S --> C1[commands/<br/>slash commands]
    S --> A1[agents/<br/>specialized subagents]
    S --> K1[skills/<br/>reusable guidance]
    S --> H1[hooks/<br/>event-driven automation]
    S --> R1[README.md<br/>plugin documentation]
    S --> M1[.mcp.json / plugin.json<br/>integration metadata]
```

## Bundled plugin categories

```mermaid
flowchart TB
    subgraph Development
        DEV1[agent-sdk-dev]
        DEV2[feature-dev]
        DEV3[frontend-design]
        DEV4[plugin-dev]
        DEV5[ralph-wiggum]
        DEV6[claude-opus-4-5-migration]
    end

    subgraph Productivity
        PROD1[code-review]
        PROD2[commit-commands]
        PROD3[hookify]
        PROD4[pr-review-toolkit]
    end

    subgraph Learning_and_Output_Style
        LEARN1[explanatory-output-style]
        LEARN2[learning-output-style]
    end

    subgraph Security
        SEC1[security-guidance]
    end
```

## Repository automation architecture

The repository is also maintained through automation. Workflows in `.github/workflows/` orchestrate scripts in `scripts/` to triage issues, deduplicate reports, and keep repository hygiene in place.

```mermaid
flowchart LR
    I[GitHub issues / PR events] --> W[.github/workflows/*.yml]
    W --> S[scripts/*.ts / *.sh]
    W --> C[.claude/commands]
    S --> O[comments, labels, dedupe actions]
    C --> O
```

## How the modules fit together

1. The root documentation points users to bundled plugins and setup guidance.
2. Marketplace metadata enumerates the official plugins shipped in the repository.
3. Each plugin encapsulates its own commands, agents, skills, hooks, and optional MCP integration.
4. Example files show plugin authors how to configure settings and hooks.
5. GitHub Actions workflows and maintenance scripts keep issue management and repository operations running.
