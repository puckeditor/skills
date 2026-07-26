# Puck Skills

[Agent Skills](https://github.com/anthropics/skills) that teach AI coding assistants (Claude
Code, Cursor, Codex, and others) how to work with [Puck](https://puckeditor.com) — the visual
editor for React.

## Skills

| Skill                 | Description                                                                                                                                                          |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [puck](./skills/puck) | Integrate Puck into an existing application, author a config from the app's own components, wire up editor and render routes, debug an integration, and add Puck AI. |

## Installation

### skills CLI

```bash
npx skills add puckeditor/skills --skill "puck"
```

Add `-g` to install globally rather than into the current project.

### Claude Code plugin

```
/plugin marketplace add puckeditor/skills
/plugin install puck
```

### Manual symlink

```bash
git clone https://github.com/puckeditor/skills.git /path/to/puck-skills
ln -s /path/to/puck-skills/skills/puck /path/to/skills-directory/puck
```

## Usage

Once installed, the agent picks the skill up when relevant — for example:

- "Add a page builder to this app"
- "Make our marketing components editable in Puck"
- "Puck components render unstyled in the editor"
- "Set up Puck AI"
- "Move this config off `DropZone`"

## Prerequisites

Only [`@puckeditor/core`](https://www.npmjs.com/package/@puckeditor/core) and a React 18+
application. The skill installs it if it isn't present.

Puck AI additionally needs a [Puck Cloud](https://cloud.puckeditor.com) account with credit
and a `PUCK_API_KEY`.

Starting a project from scratch? Use `npx create-puck-app` — this skill is for existing
applications.

## Contributing

Authoring conventions are in [AGENTS.md](./AGENTS.md); they're also the review criteria.
Bugs and use-case requests: [issues](https://github.com/puckeditor/skills/issues).

## License

MIT
