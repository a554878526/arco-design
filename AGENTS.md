# AGENTS.md

## Cursor Cloud specific instructions

### Overview

Arco Design (`@arco-design/web-react`) is a React UI component library by ByteDance with 60+ components. It is a single-package repo (not a monorepo) using **Yarn Classic (v1)** as the package manager. There are no external services, databases, or Docker containers required.

### Critical: OpenSSL legacy provider

Node.js 22 requires `NODE_OPTIONS=--openssl-legacy-provider` for all webpack-based commands (build, dev, dev:site, test, demo). This is already set in `~/.bashrc` on the VM. If it's missing, commands like `yarn build` and `yarn dev:site` will fail with `ERR_OSSL_EVP_UNSUPPORTED`.

### Common commands

See `package.json` scripts and `CONTRIBUTING.md` for full details. Key commands:

| Task | Command | Notes |
|------|---------|-------|
| Full init | `yarn run init` | Runs `cd site && yarn && cd .. && yarn && yarn icon && yarn build` |
| Build | `yarn build` | Builds ES/CJS/dist bundles; required before tests |
| Dev (component) | `yarn dev` | Hot-reload dev server for component work |
| Dev (doc site) | `yarn dev:site` | Documentation site on port 9000 |
| Lint (JS/TS) | `yarn eslint` | ESLint with `--fix --cache --quiet` |
| Lint (CSS/Less) | `yarn stylelint` | Stylelint for Less files |
| Test (full) | `yarn test` | Runs `build:cjs` then `test:client` and `test:node` |
| Test (client only) | `yarn test:client` | Jest client tests; use `--testPathPattern` to scope |
| Test (watch) | `yarn test:watch ComponentName` | Watch mode for a specific component |
| Icons | `yarn icon` | Regenerate icon components from SVGs |
| Docgen | `yarn docgen` | Regenerate component README files from `interface.ts` |
| Storybook | `yarn demo` | Port 6006; needs `yarn build` first, no hot reload |

### Testing notes

- `yarn test:node` has 5 pre-existing snapshot failures in date/time-sensitive components (DatePicker, TimePicker, Statistic) due to snapshots containing dates that change over time. These are not caused by code changes.
- To run tests for a specific component: `yarn test:client --testPathPattern="components/Button/__test__"`
- The full `yarn test` suite is CPU-intensive. For quick validation, scope tests with `--testPathPattern`.

### Project structure

- `components/` — all 73 UI components, each with `__test__/`, `__demo__/`, `style/`, `interface.ts`
- `icon/` — icon sub-package (private); icons generated via `yarn icon`
- `site/` — documentation website (separate `package.json` and `yarn.lock`; needs its own `yarn install`)
- `stories/` — Storybook stories
- `.config/` — build configs (webpack, jest, docgen, style)
