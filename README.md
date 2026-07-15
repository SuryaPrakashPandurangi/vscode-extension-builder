# VS Code Extension Builder

A Claude Code plugin that builds a working, installed VS Code extension
end-to-end from a single request. The user never runs `npm install`,
installs `yo`/`generator-code`, or invokes `vsce` themselves — the skill
does all of it: scaffolding, dependency install, TypeScript compilation,
packaging, and local installation into VS Code.

Supports three extension types:

- **command** — contributes one or more commands, runs TypeScript logic
  (webviews, menus, keybindings, status bar items, file watchers, etc.)
- **theme** — a color theme
- **snippets** — a snippet pack for one or more languages

## Install

```
/plugin marketplace add SuryaPrakashPandurangi/vscode-extension-builder
/plugin install vscode-extension-builder@vscode-extension-builder
```

## Usage

Ask Claude Code to build a VS Code extension, e.g.:

> "Create a VS Code extension that formats JSON on save"

The skill scaffolds the project, implements the requested feature, compiles
it, packages it as a `.vsix`, and installs it into your local VS Code. See
[skills/vscode-extension-builder/SKILL.md](skills/vscode-extension-builder/SKILL.md)
for the full workflow, and
[skills/vscode-extension-builder/references/](skills/vscode-extension-builder/references/)
for contribution-point wiring (webviews, menus, keybindings, etc.) and
Marketplace publishing steps.

## Repository layout

```
.claude-plugin/plugin.json                   — plugin manifest
.claude-plugin/marketplace.json              — marketplace catalog (lists this plugin for /plugin install)
skills/vscode-extension-builder/             — the skill itself
  SKILL.md                                   — workflow instructions
  scripts/create-extension.js                — deterministic scaffolder
  references/                                — contribution points & publishing docs
.claude/skills/vscode-extension-builder/      — dev-only harness (not part of the published plugin)
  SKILL.md, driver.mjs                       — smoke-tests the scaffolder end to end
```

## Development

To verify the scaffolder still works end to end (scaffold → `npm install` →
compile → `vsce package` → optional `code --install-extension`), run the dev
harness:

```bash
node ".claude/skills/vscode-extension-builder/driver.mjs" --type command
```

See [.claude/skills/vscode-extension-builder/SKILL.md](.claude/skills/vscode-extension-builder/SKILL.md)
for all driver flags.
