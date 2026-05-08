# digit-wrap repro

Local Vite + Svelte app that consumes **`@number-flow/svelte`** from this monorepo (`workspace:*`), so it picks up the patched `number-flow` package.

## Run locally

From the **repository root**:

```bash
pnpm install
pnpm --filter number-flow build
pnpm --filter @number-flow/svelte build
pnpm --filter digit-wrap-repro dev
```

Then open the printed URL (port **5173**).

## CodeSandbox (GitHub → Devbox)

1. [Create a Devbox](https://codesandbox.io/dashboard/recent) → **Import from GitHub**.
2. Repository: **`mike-albrecht/number-flow`** (or your fork), branch **`fix-negative-digit-wrap`**.
3. The root `.codesandbox/tasks.json` runs `pnpm install`, builds `number-flow` + `@number-flow/svelte`, then starts **`pnpm --filter digit-wrap-repro dev`** with preview on **5173**.

If setup fails (pnpm missing), run the same shell commands from the integrated terminal at the repo root.
