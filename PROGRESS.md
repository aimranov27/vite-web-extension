# Progress

## Changelog

### 2026-10-14 — Fork, move to Bun, upgrade every dependency

Forked from JohnBra/vite-web-extension (last upstream push 2025-11-10) so we can
keep our own template current.

- Bun replaces Yarn. `bun.lock` replaces `yarn.lock`; `packageManager` pins Bun.
  Vite still runs on Node (24, see `.nvmrc`); Bun installs packages and runs scripts.
- Upgraded to the latest majors: Vite 8 (Rolldown), TypeScript 7 (native),
  @crxjs/vite-plugin 3, @vitejs/plugin-react 6, React 19.3, Tailwind 4.3,
  @types/chrome 0.3, @types/node 26.
- Oxlint replaces ESLint. typescript-eslint only supports TypeScript below 6.1,
  and eslint-plugin-react does not support ESLint 10, so ESLint would have pinned
  us to old versions. The old `.eslintrc` did not load under ESLint 9 anyway.
- Removed unused packages: fs-extra, ts-node, eslint-plugin-import,
  eslint-plugin-jsx-a11y, eslint-config-prettier. Removed vite-tsconfig-paths
  in favour of Vite 8's `resolve.tsconfigPaths`.
- Removed `src/global.d.ts`: its SVG type promised a `ReactComponent` export
  that nothing provides, and `vite/client` already types asset imports.
- Vite configs now load under Vite's native config loader (`import.meta.dirname`,
  `.ts` import extensions, JSON import attributes, type-only imports).
- New scripts: `typecheck`, `lint`, and `check` (typecheck + lint + both builds).
- CI runs `bun run check` on pushes to main and on pull requests and uploads
  Chrome and Firefox builds. Dependabot watches Bun packages and GitHub Actions.

Checked: built Chrome output loads in Chromium with the service worker, content
script, popup, options and new tab pages rendering and no errors, same as the
old template. Firefox builds but was not loaded in Firefox.

Known: dev builds print a Rolldown `SOURCEMAP_BROKEN` warning from
@tailwindcss/vite. It only affects dev sourcemaps for CSS.
