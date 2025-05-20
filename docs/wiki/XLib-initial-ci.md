# Library

# Project CI workflow


## TS library project initialize

> TS + Eslint + prettier + changeset + commitlint + huskey

1. add tsc config

```bash
pnpm add typescript -D
npx tsc --init
```
2. 集成Jest

```bash
pnpm add jest ts-node ts-jest @types/jest @jest/globals -D
npx ts-jest config:init
```
-----

3. 集成 eslint

> eslint 9.x

```bash
pnpm add eslint globals typescript-eslint @eslint/js eslint-plugin-import -D
pnpm add prettier eslint-plugin-prettier eslint-config-prettier -D
```

> eslint.config.mjs 9.x

```mjs
import js from '@eslint/js';
import globals from 'globals';

import tseslint from 'typescript-eslint';
import eslintConfigPrettier from 'eslint-config-prettier';

export default tseslint.config({
  extends: [
    js.configs.recommended,
    ...tseslint.configs.recommended,
    eslintConfigPrettier,
  ],
  files: ['**/*.{ts,js,json}'],
  ignores: [
    'build/*',
    '.changeset/*',
    '.vscode/*',
    '.husky/*',
    'dist',
    'node_modules',
    '.vscode',
    'tsconfig.json',
    'package.json',
    '.*',
    'commitlint.config.mjs',
  ],
  languageOptions: {
    ecmaVersion: 2020,
    globals: {
      ...globals.browser,
      ...globals.node,
    },
  },
  plugins: {},
  rules: {
    'arrow-body-style': 'warn',
    '@typescript-eslint/no-explicit-any': 'off',
  },
});
```
> config prettier

```ts
// .pretterignore
// package.json
"format": "prettier \"**/**/*.{ts,js,json,tsx,mjs}\" --ignore-path ./.prettierignore --write",
```

4. 集成 Husky + Commitlint Lint-staged

```bash
pnpm add @commitlint/{config-conventional,cli} @commitlint/types husky lint-staged -D

npx husky init
pnpm add @commitlint/types -D
touch commitlint.config.ts

# check commitlint
npx commitlint --from HEAD~1 --to HEAD --verbose
```

5. 集成changeset

```bash
pnpm add @changesets/cli @changesets/changelog-github -D
npx changeset init
```

## Rollup build

```bash
pnpm add rollup @rollup/plugin-typescript @rollup/plugin-terser rollup-plugin-dts -D

touch rollup.config.js

```
