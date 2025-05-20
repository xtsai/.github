# Front UI
> vue3 + TS + router integrante

## Common libs

1. router pinia axios
2. common development libs

```bash
pnpm add vue-router axios pinia @vueuse/core

pnpm add -D @types/node @pinia/testing @iconify/json @vue/compiler-sfc @vitejs/plugin-vue-jsx
```

### Vite engineering enhancement

1. initialize build dir

```bash
mkdir -p build/vite build/utils
```

2. some development dependencies

```bash
pnpm add -D chalk dotenv cross-env fs-extra
```

3. some enhancement utils files
   utils/logger.ts ed.

4. vite plugins

```bash
# install plugins dependencies
pnpm add -D vite-plugin-compression
```

5. addition SASS support

```bash
# install plugins dependencies
pnpm add -D sass
```

```bash
mkdir -p src/styles/dark
touch src/styles/index.scss src/styles/variables.scss src/styles/reset.scss

```

```ts
//vite.config.js
```

---

## Eslint9.x + prettier

1. install dependencies

```bash
pnpm add -D @eslint/js @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint-plugin-vue eslint globals typescript-eslint

pnpm add -D eslint-config-prettier eslint-plugin-prettier prettier
```

2. eslint configuration

```bash
touch eslint.config.js
```

```json
{
  // package.json scripts
  "lint": "eslint \"{src,scripts,bin}/**/*.{ts,tsx,jsx,vue,json,less,scss,css}\" --fix"
}
```

```ts
// eslint.config.js
import pluginJs from '@eslint/js';
import globals from 'globals';
import pluginVue from 'eslint-plugin-vue';
import tseslint from 'typescript-eslint';
import { globalIgnores } from 'eslint/config';

import configPrettier from 'eslint-config-prettier'; // 禁用与 Prettier 冲突的规则

/** @type {import('eslint').Linter.Config[]} */
export default [
  globalIgnores([
    '!node_modules', //unignore `node_modules/` directory
    'node_modules/*',
    'src/typings/vite-env.d.ts'
  ]),
  { files: ['**/*.{ts,js,mjs,cjs,vue,tsx,jsx}'] },
  // 全局配置
  {
    languageOptions: {
      globals: {
        ...globals.browser,
        __APP_INFO__: 'readonly'
      }
    }
  },
  pluginJs.configs.recommended,
  ...tseslint.configs.recommended,
  ...pluginVue.configs['flat/recommended'],
  {
    files: ['**/*.vue'],
    languageOptions: {
      parserOptions: {
        parser: tseslint.parser,
        ecmaVersion: 'latest',
        sourceType: 'module',
        ecmaFeatures: {
          jsx: true // resolve jsx ,tsx Parsing error '>' expected
        }
      }
    }
  },
  // custom rules
  {
    rules: {
      ...configPrettier.rules,
      'vue/multi-word-component-names': 'off',
      '@typescript-eslint/no-unused-expressions': [
        'error',
        {
          allowShortCircuit: true,
          allowTernary: true
        }
      ],
      '@typescript-eslint/no-explicit-any': [
        'warn',
        {
          fixToUnknown: true,
          ignoreRestArgs: true
        }
      ],
      '@typescript-eslint/no-unused-vars': [
        'error',
        {
          args: 'all',
          argsIgnorePattern: '^_',
          caughtErrors: 'all',
          caughtErrorsIgnorePattern: '^_',
          destructuredArrayIgnorePattern: '^_',
          varsIgnorePattern: '^_',
          ignoreRestSiblings: true
        }
      ]
    }
  }
];
```

3. prettier configuration

```bash
touch .prettierrc.yml .prettierignore
```

```yaml
# .prettierrc

semi: true
singleQuote: true

tabWidth: 2
useTabs: false
trailingComma: 'none'
```

```json
// package.json scripts
  "format": "prettier \"{src,build,scripts}/**/*.{ts,js,tsx,jsx,json,vue,scss,less}\" --write",
```

## Integrated unocss

> [docs](https://unocss.dev/integrations/vite)

1. install dependencies

```bash
pnpm add -D unocss

pnpm add -D @unocss/preset-uno
```

2. configuration file

```ts
// plugins vite : build/vite/plugins
```

```bash
touch uno.config.ts
```

3. import unocss

```ts
// main.ts
import 'virtual:uno.css';
```

4. some useful dependencies

[iconify](https://iconify.design/docs/usage/css/unocss/)

@iconify/json,

```bash
pnpm add -D @unocss/preset-icons

# @iconify-json/<icon-collections>

pnpm add -D @iconify-json/tabler  @iconify-json/carbon @iconify-json/fluent
```

```vue
<span class="i-carbon-logo-github"></span>
<span class="i-mdi-light-home"></span>
```

----
## 集成 Husky + Commitlint Lint-staged

1. add dependency libraries

```bash
pnpm add @commitlint/{config-conventional,cli} @commitlint/types husky lint-staged -D
pnpm add @commitlint/types -D

# initialize husky
npx husky init

touch commitlint.config.ts
```

2. commitlint.config.ts

```ts
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      2,
      'always',
      [
        'feat',
        'fix',
        'docs',
        'chore',
        'style',
        'refactor',
        'ci',
        'test',
        'revert',
        'perf',
        'vercel'
      ]
    ]
  }
};
```

# check commitlint

npx commitlint --from HEAD~1 --to HEAD --verbose

3. edit husky commit-msg

```bash
touch .husky/commit-msg

#!/bin/sh
npx --no -- commitlint --edit "$1"

```

4. lint-staged

---

## Integranted Stylelint

1. install library

```bash
pnpm add -D stylelint stylelint-config-html stylelint-config-prettier stylelint-config-recommended-scss stylelint-config-standard stylelint-config-recommended-vue stylelint-config-standard-scss
```

2. config files

```bash
touch stylelint.config.js
# NPM scripts
"lint:css": "stylelint --fix \"src/**/*.{vue,less,css,scss,postcss}\" --cache --cache-location node_modules/.cache/stylelint",
```

```ts
/** @type {import('stylelint').Config} */
export default {
  extends: [
    'stylelint-config-html/vue', //  配置 vue 中 template 样式格式化
    'stylelint-config-recommended-scss',
    'stylelint-config-recommended-vue/scss' // 配置 vue 中 scss 样式格式化
  ],
  ignoreFiles: [
    '**/*.ts',
    '**/*.tsx',
    '**/*.json',
    '**/*.md',
    '**/*.js',
    'dist/*',
    'public/*'
  ],
  rules: {
    'keyframes-name-pattern': null,
    'function-url-quotes': 'always', // URL 的引号 "always(必须加上引号)"|"never(没有引号)"
    'property-no-unknown': null, // 禁止未知的属性
    'no-empty-source': null,
    'selector-pseudo-class-no-unknown': [
      true,
      {
        ignorePseudoClasses: ['global', 'export', 'v-deep', 'deep']
      }
    ]
  }
};
```

-----
