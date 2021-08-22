
# 開発環境構築ステップ(zshの場合)
## Homebrew & Anyenv & Nodenv

```
brew install anyenv
echo 'eval "$(anyenv init - zsh)"' >> ~/.zshrc
exec $SHELL -l
anyenv install nodenv
source .zshrc
nodenv install -l
nodenv install 12.13.1
nodenv install 10.17.0
nodenv global 12.13.1
npm install -g yarn
source .zshrc
npm install -g typescript
```

## create-react-app

```
npx create-react-app {project_name} --template typescript
cd {project_name}
```

# Lint & Prettier & Typesyncのインストール

```
yarn add -D stylelint prettier
yarn add -D eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-import eslint-plugin-jest
yarn add -D eslint-plugin-prefer-arrow eslint-plugin-jsx-a11y eslint-plugin-prettier eslint-config-prettier
yarn add -D eslint-config-airbnb
yarn add -D @typescript-eslint/parser @typescript-eslint/eslint-plugin
yarn add -D stylelint-config-prettier stylelint-config-standard stylelint-order
yarn add -D stylelint-config-styled-components stylelint-processor-styled-components
yarn add -D prettier-stylelint
npm install -g typesync
source ~/.zshrc
typesync
yarn
yarn add -D husky lint-staged
```

# Firebaseインストール

```
npm install -g firebase-tools
source ~/.zshrc
cd {project_name}
firebase login
```

# 各種設定ファイル

## package.jsonに追記
```
"scripts" : {
   "lint": "eslint 'src/**/*.{js,jsx,ts,tsx}'; stylelint 'src/**/*.{css,jsx,tsx}'; cd functions/ && eslint 'src/**/*.{js,ts}'",
   "precommit": "lint-staged"
},
"husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
},
 "lint-staged": {
    "src/**/*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "git add"
    ],
    "src/**/*.{css,jsx,tsx}": [
      "stylelint --fix",
      "git add"
    ],
    "functions/src/**/*.{js,ts}":[
      "cd functions/ && eslint --fix",
      "git add"
    ]
 }
```

## tsconfig.json

```
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react",
    "baseUrl": "src",
    "incremental": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "build", "scripts", "functions"]
}
```

### .eslintignore
```
node_modules/
*.config.js
```

### .eslintrc.js

```
module.exports = {
  env: {
    browser: true,
    es6: true,
    node: true,
    'jest/globals': true,
  },
  extends: [
    'airbnb',
    'eslint:recommended',
    'plugin:@typescript-eslint/eslint-recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:import/errors',
    'plugin:import/warnings',
    'plugin:import/typescript',
    'plugin:jest/recommended',
    'plugin:jsx-a11y/recommended',
    'plugin:prettier/recommended',
    'plugin:react/recommended',
    'prettier',
    'prettier/@typescript-eslint',
    'prettier/react',
    'prettier/standard',
  ],
  globals: {
    Atomics: 'readonly',
    cy: 'readonly',
    Cypress: 'readonly',
    SharedArrayBuffer: 'readonly',
    __DEV__: true,
  },
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 2018,
    project: './tsconfig.json',
    sourceType: 'module',
  },
  plugins: [
    '@typescript-eslint',
    'import',
    'jest',
    'jsx-a11y',
    'prefer-arrow',
    'prettier',
    'react',
    'react-hooks',
  ],
  root: true,
  rules: {
    // eslint official
    'linebreak-style': ['error', 'unix'],
    'newline-before-return': 'error',
    'no-console': 'warn',
    'no-continue': 'off',
    quotes: ['error', 'single', { avoidEscape: true }],
    'require-yield': 'error',
    semi: ['error', 'always'],
    // for react-app-env.d.ts (https://github.com/facebook/create-react-app/issues/6560)
    'spaced-comment': [
      'error',
      'always',
      {
        markers: ['/'],
      },
    ],

    // @typescript-eslint
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/explicit-member-accessibility': 'off',
    indent: 'off',
    '@typescript-eslint/indent': 'off',
    '@typescript-eslint/no-unnecessary-type-assertion': 'error',
    '@typescript-eslint/no-unused-vars': 'error',
    '@typescript-eslint/prefer-interface': 'off',

    // airbnb
    'no-restricted-syntax': [
      'error',
      {
        selector: 'ForInStatement',
        message:
          'for..in loops iterate over the entire prototype chain, which is virtually never what you want. Use Object.{keys,values,entries}, and iterate over the resulting array.',
      },
      {
        selector: 'LabeledStatement',
        message:
          'Labels are a form of GOTO; using them makes code confusing and hard to maintain and understand.',
      },
      {
        selector: 'WithStatement',
        message:
          '`with` is disallowed in strict mode because it makes code impossible to predict and optimize.',
      },
    ],
    // prefer-arrow
    'prefer-arrow/prefer-arrow-functions': [
      'error',
      {
        disallowPrototype: true,
        singleReturnOnly: true,
        classPropertiesAllowed: false,
      },
    ],

    // react
    'react/jsx-filename-extension': [
      'error',
      {
        extensions: ['jsx', 'tsx'],
      },
    ],
    'react/jsx-props-no-spreading': [
      'warn',
      {
        custom: 'ignore',
      },
    ],
    'react/prop-types': 'off',

    // react hooks
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'error',

    // import
    'import/extensions': [
      'error',
      'always',
      {
        js: 'never',
        jsx: 'never',
        ts: 'never',
        tsx: 'never',
      },
    ],
    'import/no-extraneous-dependencies': [
      'error',
      {
        devDependencies: [
          '.storybook/**',
          'stories/**',
          '**/*/*.story.*',
          '**/*/*.stories.*',
          '**/__specs__/**',
          '**/*/*.spec.*',
          '**/__tests__/**',
          '**/*/*.test.*',
          'src/setupTests.*',
        ],
      },
    ],
    'import/prefer-default-export': 'off',
  },
  settings: {
    'import/parsers': {
      '@typescript-eslint/parser': ['.ts', '.tsx'],
    },
    'import/resolver': {
      node: {
        extensions: ['.js', 'jsx', '.ts', '.tsx'],
        paths: ['src'],
      },
    },
    react: {
      version: 'detect',
    },
  },
};
```

### .prettierrc
```
{
  "bracketSpacing": true,
  "printWidth": 80,
  "semi": true,
  "singleQuote": true,
  "trailingComma": 'all',
  "useTabs": false,
}
```

### stylelint.config.js
```
const argv = require('yargs').argv;
const glob = argv['_'] && argv['_'][0];
const isJsxFile = glob && /.{jsx,tsx}/.test(glob);

if (isJsxFile) {
  module.exports = {
    extends: [
      'stylelint-config-standard',
      'stylelint-config-styled-components',
      './node_modules/prettier-stylelint/config.js',
    ],
    plugins: ['stylelint-order'],
    processors: ['stylelint-processor-styled-components'],
    rules: {
      'declaration-empty-line-before': 'never',
      'indentation': 2,
      'no-missing-end-of-source-newline': null,
      'string-quotes': 'single',
      'order/properties-alphabetical-order': true
    }
  };
}

module.exports = {
  extends: [
		'stylelint-config-standard',
		'./node_modules/prettier-stylelint/config.js'
	],
  ignoreFiles: [
    '**/node_modules/**',
    'src/styles/**'
  ],
  plugins: ['stylelint-order'],
  rules: {
    'declaration-empty-line-before': 'never',
    'indentation': 2,
    'no-missing-end-of-source-newline': null,
    'string-quotes': 'single',
    'order/properties-alphabetical-order': true
  },
};
```

## .gitignore
```
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

pids
*.pid
*.seed
*.pid.lock

node_modules/
jspm_packages/
/.pnp
.pnp.js

/coverage

/build

typings/

.npm

.firebase

.eslintcache

.node_repl_history

*.tgz

.yarn-integrity

.idea/
.*.swp
.vscode/chrome

.env
#.env.*
.firebaserc

.DS_Store
npm-debug.log*
yarn-debug.log*
yarn-error.log*
```
