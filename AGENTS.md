# Arco Design - Agent Instructions

## Cursor Cloud specific instructions

### Node.js Version
This project requires **Node.js 16** (v16.20.2). The default system Node is too new. Use `nvm use 16` to activate the correct version. Node 16 is set as the nvm default.

### Timezone
Tests involving date/time components (DatePicker, TimePicker, Statistic, ConfigProvider) require `TZ=Asia/Shanghai`. Always prefix test commands with this:
```
TZ='Asia/Shanghai' yarn test:client
```

### Key Commands
Standard commands are in `package.json` scripts and `CONTRIBUTING.md`. Non-obvious notes:

- **Full init**: `yarn run init` (installs site deps, root deps, builds icons, builds components)
- **Dev site**: `yarn dev:site` (port 9000) — requires prior `yarn icon` and builds (`yarn build:es`, `yarn build:cjs`, `yarn build:css`)
- **Tests**: `TZ='Asia/Shanghai' yarn test` runs `build:cjs` then `test:client` + `test:node`. For faster iteration, run `TZ='Asia/Shanghai' yarn test:client` directly (requires CJS build already done).
- **Lint**: `yarn eslint` and `yarn stylelint`
- **Icon generation**: `yarn icon` must run before builds; regenerates React icon components from SVGs in `icon/` directory.

### Build Order
Before running the dev site or tests, the following builds are needed:
1. `yarn icon` — generate icon components
2. `yarn build:cjs` — CJS module build (needed for tests)
3. `yarn build:es` — ES module build (needed for ConfigProvider demo tests and dev site)
4. `yarn build:css` — CSS build (needed for dev site)

Or simply `yarn build` to do all component builds at once (but this is slower).

### No External Services
This is a frontend-only React component library. No databases, Docker, or external APIs are required.
