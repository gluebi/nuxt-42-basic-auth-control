# Nuxt 4.0.3 — basic-auth-in-URL control case

Companion to [`../nuxt-44-basic-auth-repro/`](../nuxt-44-basic-auth-repro/). Same minimal Nuxt app, same nginx basic-auth harness — but pinned to **Nuxt 4.0.3** (which naturally resolves `vue-router@4.6.4` rather than `5.0.7`).

## What this shows

Visiting `http://test:test@localhost:8082/` in a fresh Chromium profile **does not** trigger the `SecurityError` cascade that fires in the 4.4 repo. Console is clean, hydration completes, and `<NuxtLink>` performs client-side navigation. See `console-clean-40.log` (zero entries at error level) and `console-clean-40.png`.

This control isolates the regression to the vue-router 4 → 5 jump that happened across Nuxt 4.x.

## Run

```bash
docker compose up --build
# wait for "Listening on http://0.0.0.0:3000"
```

Then open `http://test:test@localhost:8082/` in a fresh Chrome profile (`--user-data-dir=/tmp/repro`).

## Versions

- `nuxt@4.0.3`
- transitively pulled `vue-router@4.6.4`
- nginx, htpasswd, ports `:8081` (nuxt direct, dev only) / `:8082` (nginx + basic-auth)

Everything else (pages, app.vue, nuxt.config.ts) is byte-identical to the broken repro.
