This is a minimal reproduction for `next-env.d.ts` churn between `next typegen` and `next dev`.

The repository intentionally tracks `next-env.d.ts` even though Next.js projects normally should ignore it. Tracking the file makes the generated import-path churn visible as a Git diff.

## Reproduction

```bash
npm run typecheck
sed -n '1,6p' next-env.d.ts
npm run dev
# Stop the dev server after it prints Ready.
sed -n '1,6p' next-env.d.ts
git diff -- next-env.d.ts
npm run typecheck
sed -n '1,6p' next-env.d.ts
```

Observed behavior:

- `next typegen` writes `import "./.next/types/routes.d.ts";`
- `next dev` writes `import "./.next/dev/types/routes.d.ts";`

Expected behavior:

The generated root `next-env.d.ts` output stays stable across local commands, or the docs/tooling make the intended ignore-and-typegen workflow clearer.
