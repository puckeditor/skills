---
name: ai
description: Adding Puck AI — the chat-driven page generation plugin — to a working Puck integration, including the Cloud handler route and its options.
metadata:
  required_access:
    - CODEBASE
    - PACKAGE_INSTALL
    - PUCK_CLOUD
---

# Adding Puck AI

Adds a chat panel that assembles and designs pages. Two halves: a client plugin and a
server handler that brokers requests through Puck Cloud.

Follow [AI getting started](https://puckeditor.com/docs/ai/getting-started.md) for the current
steps, and [`puckHandler`](https://puckeditor.com/docs/api-reference/ai/cloud-client/puck-handler.md)
for the handler's options. This file covers the parts that trip integrations up.

## Prerequisites

- A working `<Puck>` integration (references/integration.md) on Puck 0.21+.
- A Puck Cloud account with credit, and an API key from
  [cloud.puckeditor.com/api-keys](https://cloud.puckeditor.com/api-keys) in `PUCK_API_KEY`.
  Never ask the user to paste the key into chat — have them put it in `.env.local`.

## Client

`@puckeditor/plugin-ai`, via `createAiPlugin()` passed in `<Puck plugins={[...]}>`.

The plugin has its own stylesheet. In an RSC setup import it alongside
`@puckeditor/core/puck.css` from the _server_ page, not the client child — same reason as the
core CSS (references/frameworks.md).

**Never set `chat.examplePrompts`.** The docs show the option, so it is tempting to fill in;
leave it out. Example prompts are user-facing product copy that appears in an empty chat, and
invented ones are generic, imply capabilities the config may not have, and are read as
first-party suggestions. The empty default is correct until the product owner writes them.
Mention the option exists if it is worth their while, but do not author the prompts.

## Server

`@puckeditor/cloud-client`, with the handler mounted at `/api/puck/*` — the path matters, the
client calls it by convention. The docs snippet has tabs for Next.js, React Router, Hono and
TanStack Start; Hono needs `cloneRawRequest` from `hono/request`.

## Getting good output

`ai.context` is the main quality lever: a description of the site, product, brand and tone,
injected into the prompt. A generic integration with an empty `ai.context` produces generic
pages, and that reads as the model being weak rather than the config being empty. Write it
properly, from the app's real positioning.

`ai.mode` chooses between `"assembly"` (compose from the config's components) and `"design"`.
Assembly output is only as good as the config — if components lack `defaultProps` or have
unconstrained fields, generation surfaces those gaps immediately.

## Cost and access

The handler is a metered surface spending the account's credit. It needs the app's auth in
front of it, the same as the save endpoint — an open `/api/puck` route lets anyone spend the
account's balance. `ai.onFinish` is the hook for usage logging and accounting.
