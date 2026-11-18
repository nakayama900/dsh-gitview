# Contributing

## Build and test

The browser half is written in `src/client/` and built into `lib/client.js`. The built file is committed because the DSH Host serves it as is.

```sh
npm ci
npm run build   # src/client → lib/client.js
npm run check   # types, a stale-build check, syntax
npm test
```

Tests need `js-yaml@4.3.2` available through `NODE_PATH`.

## Package verification

```sh
npm run test:package
```

This needs pnpm and a DSH version listed under `engines.dsh` (`DSH_BIN` selects the binary). It builds a `.tgz`, checks its contents, and tests installation, configuration composition and removal in a temporary DSH profile. It does not start a server or call a model. The verified package is saved in `dist/`.

## Supported DSH versions

`engines.dsh` and both `peerDependencies` in `package.json` list the DSH versions this plugin is tested against. CI runs the checks above once per listed version.

To see whether npm has published a DSH version the list does not cover yet:

```sh
npm run test:track
```

The same check runs weekly in GitHub Actions. To add a version:

1. Install it.
2. Run `DSH_BIN=<path> npm run test:package`.
3. Add the version to `engines.dsh` and both `peerDependencies`, then run `npm install --package-lock-only` so the lockfile matches.

## Releases

After a push to `main` passes every listed DSH version, CI publishes the `package.json` version to npm through trusted publishing and creates the `v<version>` GitHub release. Nothing is published when npm already has that version, so a release is a version bump merged to `main`.
