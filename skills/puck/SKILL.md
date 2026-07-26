---
name: puck
description: Integrate and extend Puck, the visual editor for React. Use when needing to (1) add Puck, a page builder, or a visual/drag-and-drop editor to an existing application, (2) author or extend a Puck config so the app's own components become editable, (3) debug a Puck integration — unstyled previews, RSC errors, iframe or persistence problems, or (4) look up how any Puck API, field, or plugin works.
allowed-tools:
  - WebFetch(domain:puckeditor.com)
  - Bash(npm view @puckeditor/* *)
  - Bash(npm ls @puckeditor/*)
---

# Puck

Puck is a visual editor for React. A `config` declares which of the app's components are
editable; the editor produces a JSON `Data` payload; the app stores that payload and renders
it back with `<Render>`. The app owns the data — there is no hosted content store.

## Core principles

1. **Docs first.** NEVER write Puck code from memory. Fetch the relevant
   [docs](https://puckeditor.com/docs) page before implementing — the API moves, and getting
   a field or prop name wrong costs more than the fetch. Package scope changed in 0.21
   (`@measured/puck` → `@puckeditor/core`); check `npm ls @puckeditor/core` for the version
   in play before advising anything version-sensitive.
2. **Expose the app's real components.** The value of an integration is the config. Do not
   invent generic Hero/Text/Button blocks when the app already has components.
3. **Don't reshape the app to fit Puck.** Adapt inside the component's `render`, not by
   editing the app's components.
4. **Confirm scope before editing.** Which components become editable, which routes get
   added, and where `Data` is stored are the user's decisions, and the expensive ones to
   get wrong.
5. **Verify in a browser, not a typecheck.** A Puck integration that compiles and renders
   every component unstyled is the normal first-run outcome, not success.

## Shape of an integration

Four pieces, in this order. [Getting started](https://puckeditor.com/docs/getting-started)
has the minimal version.

1. **Config** — maps app components to Puck components:
   [component](https://puckeditor.com/docs/integrating-puck/component-configuration) and
   [root](https://puckeditor.com/docs/integrating-puck/root-configuration) configuration.
2. **Editor route** — renders [`<Puck>`](https://puckeditor.com/docs/api-reference/components/puck),
   client-side only.
3. **Persistence** — `onPublish` writes the
   [`Data`](https://puckeditor.com/docs/api-reference/data-model/data) payload to wherever
   the app already stores things.
4. **Render route** — renders [`<Render>`](https://puckeditor.com/docs/api-reference/components/render)
   for end users.

Scaffolding a _new_ app is out of scope — use `npx create-puck-app`.

## Use case specific references

- adding Puck to an existing application, end to end: references/integration.md
- choosing which existing components to expose and designing their fields: references/config-authoring.md
- wiring the editor and render routes for a specific framework or router: references/frameworks.md
- diagnosing a broken integration — unstyled previews, FOUC, RSC errors, hydration, drag-and-drop: references/troubleshooting.md
- adding Puck AI, the chat-driven page generation plugin: references/ai.md
- submitting feedback about this skill: references/skill-feedback.md

## Facts worth knowing before reading anything

- `<Puck>` is client-only. `<Render>` and
  [`resolveAllData`](https://puckeditor.com/docs/api-reference/functions/resolve-all-data)
  are the only RSC-safe APIs.
- `data` cannot change after `<Puck>` mounts. Load it before rendering.
- Puck renders the canvas in an iframe and syncs host stylesheets into it. Most "Puck is
  broken" reports are the app's styles not reaching that iframe — see
  references/troubleshooting.md.
- Use the [`slot` field](https://puckeditor.com/docs/api-reference/fields/slot) for nested
  drag-and-drop areas. The legacy `DropZone` component has RSC problems.
- The editor route and the save endpoint are content-write surfaces. They need the app's
  auth before anything ships.
