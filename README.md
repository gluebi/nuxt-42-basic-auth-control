# Nuxt 4.2.2 — basic-auth-in-URL control case

Companion to [`gluebi/nuxt-44-basic-auth-repro`](https://github.com/gluebi/nuxt-44-basic-auth-repro). Same minimal Nuxt app, same nginx basic-auth harness — but pinned to **Nuxt 4.2.2**, which transitively resolves `vue-router@4.6.4` rather than `5.0.7`.

## What this shows

Visiting `http://test:test@localhost:8082/` in a fresh Chromium profile **does not** trigger the `SecurityError` cascade that fires in the 4.4 repro. Console is clean, hydration completes, and `<NuxtLink>` performs client-side navigation. See `console-clean-42.log` (zero entries at error level) and `console-clean-42.png`.

This isolates the regression to **Nuxt 4.4** specifically — the version where Nuxt bumped its vue-router peer requirement from `^4.6.x` to `^5.0.x`:

| Nuxt    | declared peer    | natural install |
|---------|------------------|-----------------|
| 4.2.x   | `vue-router ^4.6.3`  | 4.6.4 |
| 4.3.x   | `vue-router ^4.6.4`  | 4.6.4 |
| **4.4.x**   | **`vue-router ^5.0.3`** | **5.0.7** |

## Run

```bash
docker compose up --build
# wait for "Listening on http://0.0.0.0:3000"
```

Then open `http://test:test@localhost:8082/` in a fresh Chrome profile (`--user-data-dir=/tmp/repro`).

## Versions

- `nuxt@4.2.2`
- transitively pulled `vue-router@4.6.4`
- nginx alpine + htpasswd, ports `:8081` (nuxt direct, dev only) / `:8082` (nginx + basic-auth)

Everything else (pages, app.vue, nuxt.config.ts) is byte-identical to the broken repro.
