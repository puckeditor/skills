# Agent Instructions

## Adding or improving a use case

FOLLOW THESE RELIGIOUSLY. After every edit, come back and judge critically whether you
adhered to them. Fix the edit if not.

- **Only add a use case if it beats the docs.** If an agent could already serve the user by
  fetching [puckeditor.com/docs](https://puckeditor.com/docs), add nothing. Reserve new
  content for where the docs fall short. _Every addition is maintenance surface and dilutes
  the skill._

- **Never commit Puck code.** Link to the docs page so the agent fetches current, correct
  code; use pseudo-code only for logic that is genuinely specific to the workflow. _Committed
  code goes stale, and a skill is copied into the user's repo at install time, so stale code
  there outlives any fix here._

- **You should almost never touch the frontmatter `description` in `SKILL.md`.** It only
  controls whether the skill is invoked, and anyone asking about a use case already mentions
  Puck, a page builder, or a visual editor. In-skill routing handles the rest.

- **Put "when to use" guidance in exactly two places:** one one-line entry per reference in
  the `## Use case specific references` list in `SKILL.md`, and the `description` in that
  reference's frontmatter. Nowhere else — no prose routing section, no "when to use" heading
  in a reference body. _A reference body is only read after the agent chose to open it._

- **Every reference file's frontmatter must declare `metadata.required_access`** — what an
  agent needs in order to execute it. Use only these tokens, consistently:
  - `CODEBASE` — reads or edits the user's source code
  - `PACKAGE_INSTALL` — installs npm dependencies
  - `DEV_SERVER` — runs the app or a browser to observe behaviour
  - `PUCK_CLOUD` — needs a Puck Cloud account and API key

- **Every line must earn its place.** Cut filler, restatements, self-explanatory steps, and
  anything the agent already has from the docs or the task context. Keep references at 100
  lines or fewer including frontmatter. Exceeding that usually means you are producing slop.

- **Prefer fixing the docs.** Generally useful product guidance belongs on
  [puckeditor.com/docs](https://puckeditor.com/docs), with the skill linking to it. Only
  judgment that would be out of place in reference documentation — which components to
  expose, what to check first when a preview is unstyled — belongs here.

- **Be cautious with `allowed-tools`.** A tool absent from the list still works; the user just
  grants permission the first time. Only auto-allow what a user would find a no-brainer —
  today that means read-only, Puck-specific commands. _A risky auto-allow makes people
  hesitate to install at all._

## Plugin version bumps

The repo ships to two marketplaces, each with its own manifest:

- `.claude-plugin/plugin.json` (Claude Code)
- `.cursor-plugin/plugin.json` (Cursor)

Both have a `version` field and **must stay in lockstep** — bump them together, to the same
value, in the same PR as the change.

Semver:

- **Patch** — content corrections, clarified instructions, fixed links.
- **Minor** — a new skill or reference, meaningful new capability, non-breaking changes.
- **Major** — removing or renaming a skill in a way that breaks `/skill-name` invocation.

Do not bump for typos, formatting, or changes outside the published skills (this file, the
README, CI).

## Reviewing pull requests

The principles above are the review criteria, not just authoring advice. Don't restate them;
call out where the diff violates them.

- **Docs checked, and the addition beats them.** Fetch every docs page the changed content
  links, and search the docs for the use case even when the diff links nothing. Flag anything
  an agent could have got from the docs.
- **No committed Puck code.** Flag snippets that should be a docs link.
- **Concise.** Flag filler and anything over 100 lines without specific justification.
- **`metadata.required_access` present and using only the allowed tokens.**
- **Routing lives in exactly two places**, one entry per reference.
- **Version bumps in lockstep** when published behaviour changed; none for docs/CI-only.

If the diff is clean against all of the above, say so plainly.

## Verifying a change

This repo has no test suite — the skill is prose, and its correctness is whether an agent
following it reaches a working integration. Before shipping a substantive change, run the
affected path against a real application (a fresh Next.js app and a fresh Vite app are the
two that matter) and confirm the agent reaches a styled, publishing editor unattended.
