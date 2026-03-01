# Env Variables Standard

All monorepo projects must use **`dotenv-cli`** injected via package scripts to load a single workspace-root `.env.local` file.

Do not use framework-specific programmatic loaders (e.g., Next.js `@next/env` `loadEnvConfig()`) or custom raw `dotenv` utility wrappers.

## Why?

- **Framework Agnostic**: Works identically for Next.js, Express, Hardhat, React Native, etc.
- **Bulletproof Execution**: Variables populate before Node.js executes, avoiding middleware or singleton bugs.
- **Zero Utils**: No custom env wrappers required.
- **Cloud Safe**: Vercel/CI variables override naturally. `dotenv-cli` v11+ silently ignores missing local files.

## Implementation

1. **Install at workspace root:**

   ```bash
   pnpm add -w -D dotenv-cli
   ```

2. **Single Monorepo file:**
   Keep one `.env.local` and one `.env.example` at the repository root. Ensure `.env` and `.env.local` are gitignored. Delete all per-app `.env` files.

3. **Wrap `package.json` scripts:**
   Prefix app execution scripts to point to the root using `-e`:

   ```json
   "scripts": {
     "dev": "dotenv -e ../../.env.local -- next dev --port 3000",
     "build": "dotenv -e ../../.env.local -- next build",
     "start": "dotenv -e ../../.env.local -- next start --port 3000"
   }
   ```

   For non-Next.js apps (Hardhat, Express, etc.), the same pattern applies:

   ```json
   "dev": "dotenv -e ../../.env.local -- npx hardhat node"
   ```

   _(Note: The `--` passes the rest to the wrapped command)._
