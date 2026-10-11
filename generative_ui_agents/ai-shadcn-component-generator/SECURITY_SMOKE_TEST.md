# Shadify — Security and tablet smoke-test checklist

Scope: `generative_ui_agents/ai-shadcn-component-generator` on branch `test1`.
Status: static source review only; **no dependencies installed or tests executed**.

## Confirmed observations
- `apps/runtime/server.ts` calls `app.use("/*", cors())` with default permissive CORS and does not implement authentication in this file. **Do not expose publicly** until access control and allowed origins are designed and tested.
- `apps/agent/main.py` hard-codes `model="openai:gpt-5.5"`. An API provider and possible charges are required for full AI generation.
- `apps/ui/vite.config.ts` proxies `/api/copilotkit` and `/realtime/session` to localhost:4000. UI-only tests may produce network errors for AI features.
- `render.yaml` defines two Render Starter services. Do not deploy without confirming pricing.
- `.mcp.json` invokes `npx shadcn@latest mcp`. Avoid executing an unpinned version.
- Project `.gitignore` excludes `pnpm-lock.yaml`; lockfile policy should be corrected after an audited install.

## Safe no-cost first test (isolated workspace)
1. Check available runtime: `node --version`, `corepack --version`, `python3 --version`.
2. Use a disposable private development environment; do not provide API keys.
3. From the project directory, review dependencies before running `corepack enable` and `pnpm install`. Installation executes third-party package scripts, so only do this in an isolated environment.
4. Run `pnpm --filter @shadify/ui dev --host 127.0.0.1` for a local-only trial; use workspace port forwarding rather than a public bind.
5. Test page load, navigation, responsiveness, and code export if available. AI chat will not work without runtime and agent.
6. Stop the process and inspect browser/terminal errors.

## Before backend activation
- Add authentication and per-user limits.
- Restrict CORS to the actual frontend origin and verify behavior.
- Set model spending limits and secret management.
- Audit package lockfiles, dependencies and install scripts.
- Add tests for validation, abuse cases, and failures.
- Verify deployment pricing and keep environments private.

## Test log
- Static inspection: completed.
- Dependency vulnerability scan: pending.
- UI build and browser smoke test: pending.
- Runtime/agent integration: pending.
