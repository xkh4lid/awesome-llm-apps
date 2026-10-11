# AI Shadcn Component Generator — tablet trial & initial security review

**Experimental branch:** `test1`. This document does not change `main`.

## Verified architecture
- `apps/ui`: React + Vite frontend (port 5173).
- `apps/runtime`: Hono + CopilotKit runtime (port 4000).
- `apps/agent`: Python 3.12+ FastAPI + LangGraph agent (port 8123).
- Current README expects `OPENAI_API_KEY` for runtime and agent, and `TAVILY_API_KEY` for agent.

## Initial risk checklist (static configuration review only)
1. **Costs:** `render.yaml` declares paid Render Starter services for runtime and agent. Do not deploy it blindly.
2. **Remote code/dependency changes:** `.mcp.json` uses `npx shadcn@latest mcp`; pin and audit the package before executing.
3. **Reproducibility:** local `.gitignore` excludes `pnpm-lock.yaml`; review dependency locking before production.
4. **Secrets:** never commit `.env`, API keys, or tokens. Restrict keys and set provider budgets.
5. **Public exposure:** review auth, CORS, rate limits, request validation and code export before making any backend public.
6. **Supply chain:** run dependency audit in an isolated environment before installing or deploying. No vulnerability scan or execution has yet been performed.

## Cheapest staged tablet trial
**Phase 1: UI only, no API key, no paid deployment.**
- Open this branch in a temporary development workspace (or tablet Linux environment).
- Use Node.js 20+ and pnpm. From `generative_ui_agents/ai-shadcn-component-generator`, review packages before installation.
- After review, run:
  ```sh
  corepack enable
  pnpm install
  pnpm --filter @shadify/ui dev --host 0.0.0.0
  ```
- Open the workspace's forwarded port 5173. The frontend may render, but AI generation and backend-connected features will not function without the other two services.

**Phase 2: Agent test only after confirming costs.**
- Install Python >=3.12 and `uv`; configure runtime and agent in private environment settings.
- Provide `OPENAI_API_KEY` and `TAVILY_API_KEY` only if approved, with hard spending limits.
- Start all three services only after an audit and test in a private workspace.

**Phase 3: Security gate before deployment.**
- Audit direct/transitive dependencies, remove unnecessary tools, test frontend build and backend endpoints.
- Check authentication, request validation, rate limiting, CORS, API-key handling, and abuse/cost controls.
- Do not publish until tests pass.

## Status
- [x] Read primary README, package manifests, `.mcp.json`, `.gitignore`, and Render configuration.
- [x] Plan a no-cost UI-only smoke test.
- [ ] Run dependency scanner and test builds.
- [ ] Run backend security review and integration tests.
- [ ] Approve provider and hosting costs.
