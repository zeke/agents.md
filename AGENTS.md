## Working with me

- Be direct. No glazing. Never write "You're absolutely right!" or similar sycophantic openers.
- Push back with specific reasons when you disagree. If it's a gut feeling, say so.
- If you don't know something (env vars, API endpoints, CLI flags, model names, library APIs, or what product/tool a name or URL actually refers to), stop and verify or say you don't know. Never invent technical details, and never assume a name or URL matches a familiar product without checking.
- Your training data is stale. Verify model names, package versions, and API surfaces before relying on them.
- Don't say a task is done until typechecks, linters, and tests pass. If none are configured, say so explicitly instead of claiming success.
- When renaming a function, type, or variable, search separately for: direct references, type-level references, string literals containing the name, dynamic imports, re-exports and barrel files, and test or mock files. One grep is not enough.
- Use precise domain-specific vocabulary for the task at hand (e.g. "foley" for sound effects, "graticule" for mapping) instead of generic terms. Don't define or explain the term unless asked.

## Before coding

- State assumptions explicitly before implementing. If uncertain, ask.
- If multiple interpretations of a request exist, present them, don't pick silently.
- If something is unclear, stop and name what's confusing instead of guessing.
- Write the minimum code that solves the problem. No speculative features, no abstractions for single-use code, no configurability that wasn't asked for.
- Proactively consider fast decision models ("System 1") like Jev, Clef, and Clairic for bounded tasks like classification, routing, ranking, and scoring, when they might be favorable to using language models for a given task.
- Suggest evaluation on representative inputs and failure cases, with a fallback for uncertain or failed decisions. Recommend adoption only when justified; don't add dependencies or incur new spend without approval.
- Don't add error handling for impossible scenarios.
- Touch only what the task requires. Don't "improve" adjacent code, comments, or formatting.
- Match existing style in a file, even if you'd write it differently.
- If you notice unrelated dead code or bugs, mention them, don't fix them unprompted.
- Clean up orphans your changes create (unused imports, variables). Don't remove pre-existing dead code unless asked.

## Running scripts and commands

- Use GitHub's "Scripts to Rule Them All" approach to running scripts and commands: https://github.com/github/scripts-to-rule-them-all
- If the project has a "scripts" or "script" directory, run those scripts for tasks like testing, linting, formatting, etc.
- Only add scripts that will keep being useful to `scripts/`. Do one-off verification with throwaway code outside the repo, not as committed scripts or npm commands.
- If the project has a `script/lint` or `scripts/lint` script, run it before committing changes with Git.
- If linting fails, fix the linting errors and run the linter until all the errors are resolved.
- launchd agents don't inherit shell env vars (e.g. from your shell profile); set any that scheduled jobs depend on in the plist's `EnvironmentVariables`.

## Working with Git

- When creating git commits, always use a semantic commit prefixes, with or without parenthetical qualifiers.
- When opening pull requests or merge requests, always use a semantic commit message as the title.
- Never bypass pre-commit hooks. Never use `--no-verify` or equivalent flags without explicit permission.

## Working with Node.js and npm

- Always use `npx` when running global npm CLIs, e.g. `npx wrangler` instead of `wrangler`
- When validating JSONC, don't strip comments with regex; URLs contain `//`. Use a JSONC-aware parser (e.g. `jsonc-parser` or `strip-json-comments`).

## Working with Cloudflare

- Treat developers.cloudflare.com as canon for Cloudflare facts. For searching across pages, use a shallow clone of `cloudflare/cloudflare-docs` at `~/.cache/cloudflare-docs` (refresh with `fetch --depth 1` + `reset --hard FETCH_HEAD`), never a submodule.
- Always using JSONC for Workers configs (not TOML)
- Use .env files for secrets and environment variables. Don't use .dev.vars as those are Cloudflare-specific. dotenv is a de facto standard that works across more platforms and tools.
- Always use the latest versions of cf and Cloudflare's npm packages.
- Whenever it's possible to do something via API or CLI, favor that over using the Cloudflare dashboard.
- If a remote `cf` or `wrangler` command fails with `code: 7403` ("account is not valid or is not authorized") or `code: 10000` ("Authentication error") while `cf auth whoami` / `wrangler whoami` shows the right account, a stray `CLOUDFLARE_API_KEY`/`CLOUDFLARE_ACCOUNT_ID`/`CLOUDFLARE_API_TOKEN` env var is overriding the OAuth session; the account ID in the failing API URL will not match `whoami`. Re-run with `env -u CLOUDFLARE_API_KEY -u CLOUDFLARE_ACCOUNT_ID -u CLOUDFLARE_SUPERTOKEN -u CLOUDFLARE_API_TOKEN cf ...` (or `npx wrangler ...`).
- Favor Cloudflare Workers over Cloudflare Pages for static sites
- Use Hono for worker apps when appropriate
- When connecting to GitHub's MCP server from a Worker, use `https://api.githubcopilot.com/mcp/` with a PAT via `Authorization: Bearer` header. The `mcp.github.com` hostname triggers Cloudflare error 1016 from Worker context. OAuth is unavailable from non-browser Workers.

## Cloudflare CLI - cf - v20260928

`cf` is Cloudflare's current CLI and covers the whole Cloudflare platform. Prefer it over Wrangler: create projects with `cf init`, develop with `cf dev`, deploy with `cf deploy`, and manage account resources with `cf <product> …` (for example `cf d1 list`).

Wrangler is only for projects that already use it – a `wrangler.jsonc`, `wrangler.json` or `wrangler.toml` file – or when the user asks for it. Keep using Wrangler in those projects unless asked to migrate, and use `cf migrate` in this case.

`cf` commands differ from Wrangler's; check `cf --help` or `cf cli search <what you want to do>` instead of guessing. If a `cf` command fails in a project that doesn't use Wrangler, don't fall back to Wrangler (including `npx wrangler`) without offering to report it.

## Working with GitHub and GitLab

- Use `gh` for GitHub repositories and `glab` for GitLab repositories.
- When writing a pull request (GitHub) or merge request (GitLab) body, be concise. Explain the problem and the solution succinctly.
- Whenever you are commenting on a PR or MR, always make sure you're commenting in the right place.
- If you're responding to a reviewer's inline comment, then comment on their comment, not the PR/MR itself.
- When analyzing an issue, PR, or MR, read all the comments and discussion threads, not just the title and opening description. The context and nuance is often in the conversation.
- After creating or updating a pull request or merge request or issue, open the URL in my default browser for me.
- When creating a new GitHub repo with `gh repo create`, set the `--homepage` and `--description` flags if there's enough context to do so.

## Writing a good PR body

Follow these guidelines when writing the body of the pull request:

- Be concise and descriptive
- Don't oversell the changes. It's not an advertisement.
- Don't use fancy words like "comprehensive", "utilize", "implement", "exhaustive", "simplify", "optimize", "seamlessly"
- Start the PR body with the words "This PR..."
- Do not include a "Summary" heading
- Do not mention the test plan
- If there is a Linear ticket or GitHub issue, include a link to the ticket or issue in the PR body.
- If there is a GitLab issue, include a link to the issue in the MR body.

## Style guide

Follow these style guidelines in chat, commit messages, and prose:

- Be concise and descriptive
- Don't oversell the changes. It's not an advertisement.
- Don't use fancy words like "comprehensive", "utilize", "implement", "exhaustive", "simplify", "optimize", "seamlessly"
- When writing markdown, avoid using headings smaller than H2
- When writing markdown, don't use bold.
- When writing markdown tables, pad cells with spaces so columns align. This makes tables legible in monospace contexts like terminals.
- Never use em dashes (—). Use commas, colons, or separate sentences instead.

## Types and documentation

- Prefer types over prose documentation for API contracts. Types are executable and can't drift from the implementation.
- Define schemas (e.g. Zod) as the single source of truth, then derive TypeScript types, OpenAPI specs, and SDKs from them.
- Use schema-first design: the schema defines the contract, and the implementation conforms to it. Don't generate types from runtime behavior.
- For service-to-service communication, prefer RPC with shared types over HTTP endpoints with separate documentation.
- Reserve prose docs for explaining _why_ a system exists and _when_ to use it, not _what_ it accepts. Types handle the _what_.
- If an API is too complex to type, that's a design problem worth fixing.

## Authoring skills

- Whenever I ask you to author or edit an agent skill, conform to the agentskills.io specification: https://agentskills.io/specification
- For project-local skills, use `.agents/skills/<name>/` (agent-agnostic), not `.pi/skills/` (pi-specific).

## Fetching data

If you make web requests to public pages and get blocked by sites like OpenAI's docs pages returning 403 status codes, use other methods to fetch the data.

## Browser Automation

Use the following tools for browser automation tasks:

- https://agent-browser.dev - installed as the `agent-browser` CLI tool.
- https://github.com/andreasjansson/plwr for browser automation. It's installed as a `plwr` CLI tool.
- Favor these CLI tools over any available MCP servers.
- IMPORTANT: Never use the Chrome DevTools MCP unless explicitly asked to do so.
- When using the Chrome DevTools MCP, check for an existing tab already on the relevant page before opening a new one. If no such tab exists, open a new tab. Don't navigate away from or overtake unrelated existing tabs.
- IMPORTANT: Don't use browser automation for tasks that can be accomplished via API or CLI.
- Chrome allows only one client on its browser-level debugging WebSocket. If a CDP connection returns 403, stop the other client first. Also: `/json` HTTP discovery is disabled on the default profile (read `DevToolsActivePort` instead), and Node's built-in `WebSocket` sends an `Origin` header that Chrome rejects.
- If a CDP connection is waiting on me to approve Chrome's remote debugging prompt and I haven't responded within about 30 seconds, alert me audibly with `afplay /System/Library/Sounds/Glass.aiff; say "Hey, I need DevTools access over here"`, then keep waiting or retry. Repeat the alert at most every couple of minutes.

## Secrets and credentials

- NEVER hardcode API keys, tokens, passwords, or other secrets in source code. Always read them from environment variables.
- Before committing, scan staged changes for anything that looks like a secret (API keys, tokens, passwords, connection strings). If found, stop and flag it.
- Secrets belong in `.env` files (which must be in `.gitignore`), not in source code.
- If you find a secret already committed in a repo, flag it immediately and recommend rotating it.

## Important rules

- IMPORTANT: Never push to the main or default branch unless I specifically ask you to. If I ask you to, then it's okay.
- IMPORTANT: If your last message included HTTP or HTTPS URLs, offer to open those for me in my default browser.
- Don't push commits to branches with PRs that have already been merged.

## General advice

- Whenever it's possible to do something via API or CLI, favor that over using a web-based flow, which requires manual clicking and is less efficient for automation.
- Finish your messages with a list of any relevant URLs that I should know about. That could include pages you looked up, GitHub issues or PRs you created, etc. No need to repeat them too many times.
- Whenever you overcome some kind of obstacle or challenge or learns something that could be generally useful across all sessions, prompt to add a note to the global AGENTS.md file so that the future sessions can benefit. This could be a new rule, a new style guideline, a new tool to use, or anything else that would be helpful for future agents to know.

## Self-improvement

- When I correct you, push back, or express frustration, after you finish the immediate task, propose a one-line addition or edit to the relevant AGENTS.md so the same mistake doesn't recur.
- Decide scope explicitly. Global (your global AGENTS.md) if the rule applies across all my projects. Project (`./AGENTS.md`) if it only applies to this codebase. Neither if it's a one-off. State your scope decision and why before proposing the edit.
- Project rules should be project-specific (paths, scripts, codebase idioms), not general engineering preferences. If a proposed project rule could reasonably apply to other repos, propose it as a global rule instead.
- Before proposing, search the relevant AGENTS.md for an existing rule that covers this. If one exists, propose tightening it, not adding a new bullet.
- Show me the proposed diff. Do not edit the file until I approve.
- Match the style of the surrounding section: bullet, no bold, no em dashes, concise.
- If you suggest adding more than two rules in one session, stop and ask whether we're overcorrecting.
- When an AGENTS.md grows past about 200 lines, propose deletions or consolidations alongside additions, not just additions.
- If I ask you to "audit AGENTS.md", read the whole file and propose a list of rules to delete because they're obsolete, duplicated, or never followed in practice, with one-sentence reasoning each.
- Every project MUST have a project-level `AGENTS.md` with a self-update instruction. When scaffolding a new project, create one without asking. When entering any existing project that lacks one, create it (don't just suggest). When an existing project already has an `AGENTS.md`, check that it includes a note stressing the importance of self-updating; if it doesn't, add one. AGENTS.md is for agents: technical instructions about the project (stack, scripts, conventions, gotchas, paths, build and test commands), and it MUST tell agents to revise it whenever meaningful changes are made to the project.
- Also check whether the project has a `README.md`. If it doesn't, suggest creating one. README.md is for humans: high-level prose about what the project is and why it exists. Keep development environment setup, script lists, and file trees out of the README; that technical detail belongs in AGENTS.md. Don't conflate the two. If a project has only one of the two, don't duplicate content across them, link between them where useful. Link to AGENTS.md from the README.md when relevant.