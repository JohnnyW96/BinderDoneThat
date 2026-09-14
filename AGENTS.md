# AGENTS.md

## Project Context

This is a Base44 app repository. Treat it as user-owned application code, keep changes focused on the user's request, and preserve existing project conventions.

Start with `README.md` for local setup, environment variables, and publish workflow.

## Base44 References

- CLI overview: https://docs.base44.com/developers/references/cli/get-started/overview.md
- Agent skills: https://docs.base44.com/developers/backend/overview/skills.md

If your agent supports Agent Skills, install or update Base44 skills before Base44-specific work:

```bash
npx skills add base44/skills
```

## Key Files

- `src/`: frontend application source.
- `src/api/base44Client.js`: frontend Base44 SDK client.
- `vite.config.js`: Vite config and Base44 Vite plugin setup.
- `.env.local`: local-only environment values; never commit secrets.

## Base44 Sandbox Setup (docker-compose.base44.yml)

The app runs as a Vite + React frontend using `@base44/vite-plugin`. The compose file runs `npm run dev` (Vite dev server) on port 5173, mapped to host port 3000.

- The `@base44/vite-plugin` auto-detects sandbox mode via `MODAL_SANDBOX_ID` and configures `host: 0.0.0.0`, `port: 5173`, `allowedHosts: true`, and `cors: true`.
- `__VITE_ADDITIONAL_SERVER_ALLOWED_HOSTS` is passed through for Vite's host allowlist.
- The plugin enables an `/api` proxy to the Base44 backend only when `VITE_BASE44_APP_BASE_URL` is set. Without it, API calls fail and the app shows the login page (Google OAuth and access requests won't work without the backend).
- Full backend requires the Base44 CLI + Deno + `base44 login` + `base44 link` + `base44 dev`. The app must be published at least once for the local backend to serve app settings.
- No external secrets are needed for the frontend to boot.

## Working Notes

- Use `base44 dev` as the default local development command when you need the local Base44 backend. It can run the backend and frontend together.
- When docs or code mention the frontend being started automatically, that usually means the Base44 project config includes `site.serveCommand`, for example `"serveCommand": "npm run dev"` in `base44/config.jsonc`.
- Use `npm run dev` only for frontend-only work against the hosted Base44 backend.
- Prefer the existing Base44 CLI workflow over adding new npm scripts for Base44-specific tasks.
- Reuse the existing SDK client and Vite plugin patterns before adding new Base44 integration paths.
- Run the relevant checks from `package.json` before finishing code changes.
