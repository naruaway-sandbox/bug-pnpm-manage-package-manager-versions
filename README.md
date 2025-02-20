# Bug repro: pnpm "manage-package-manager-versions" does not work well with "engines" field and workspace

When I reproduced this bug, my environment was the following:

- macOS 15.2 (MacBook Air, M2 2022)
- Node.js v23.6.1
- npm v10.9.2

## How to reproduce the bug

1. As a preparation, inside `pnpm-installation/` directory in this repo, run `npm ci` to install `pnpm` package to be used for this bug reproduction.

- This preparation is to make sure we do not use other things like Corepack as `pnpm` since the bug cannot be reproduced when using `pnpm` from Corepack

2. Run `rm -rf ~/.local/share/pnpm/.tools` to clean up previously downloaded "tools" by pnpm
3. Inside `repro/` directory in this repo, run `../pnpm-installation/node_modules/.bin/pnpm --version`

After running these steps, you should see the "Unsupported engine" error like the following:

```
../pnpm-installation/node_modules/.bin/pnpm --version
 ERROR  Unsupported engine for /Users/naru/repos/github.com/naruaway-sandbox/bug-pnpm-manage-package-manager-versions/r/repro: wanted: {"pnpm":"10.3.0"} (current: {"node":"v23.6.1","pnpm":"10.4.1"})
For help, run: pnpm help
```

### What is causing this behavior

The bug happens only when we have pnpm workspace **and** the "engines" field in package.json to specify `pnpm` version along with the "packageManager" field.
If I remove either of them, the bug does not happen.
