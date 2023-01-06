
## React (Vite, Typescript) + eslint, prettier, husky

1. Init [vite](https://vitejs.dev/) with react-ts

```bash
yarn create vite app-name --template react-ts
```

2. Add [prettier](https://prettier.io/)

```bash
yarn add -D prettier
```

After it add file with eslint configuration in the root of the project (.prettierrc.json)

```json
{
  "printWidth": 120,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "bracketSpacing":true,
  "jsxBracketSameLine": false,
  "semi":true
}
```

3. Add [eslint](https://eslint.org/)

```bash
npx eslint --init
```

Points to choise

* How would you like to use ESLint? - To check syntax, find problems, and enforce code style
* What type of modules does your project use? - JavaScript modules (import/export)
* Which framework does your project use? - React
* Does your project use TypeScript? - Yes
* Where does your code run? - Browser
* How would you like to define a style for your project? - Use a popular style guide
* Which style guide do you want to follow? - standard-with-typescript
* What format do you want your config file to be in? - JSON
* The config that you've selected requires the following dependencies - Yes

Additional rules for eslint

```bash
yarn add -D eslint-plugin-react-hooks eslint-plugin-simple-import-sort
```

Configuration file (.eslintrc.json)

```json
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "plugin:react/recommended",
        "plugin:@typescript-eslint/recommended",
        "standard-with-typescript",
        "prettier",
        "plugin:react-hooks/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "overrides": [
    ],
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": "latest",
        "sourceType": "module",
        "project": "./tsconfig.json"
    },
    "plugins": ["react", "@typescript-eslint", "simple-import-sort"],
    "rules": {
        "simple-import-sort/imports": "error",
        "@typescript-eslint/no-empty-function": "off",
        "react/display-name": "off",
        "@typescript-eslint/ban-types": "off",
        "@typescript-eslint/no-empty-interface": "off",
        "react/prop-types": "off"
    }
}

```

4. Configure [husky](https://typicode.github.io/husky/#/) and  [Lintstaged](https://github.com/okonet/lint-staged#readme) (precommit)

```bash
yarn add -D husky lint-staged
npx husky install
npm set-script prepare "husky install"
npx husky add .husky/pre-commit "npx lint-staged"
```

If you don't want to see a lot of messages in console when you commit add this line (.husky/precommit)

*exec >/dev/tty 2>&1*

```
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

exec >/dev/tty 2>&1

npx lint-staged
```

After it add file with precommit (.lintstagedrc.json or into package.json)

```json
{
  "src/**/*.{ts,tsx}": [
    "eslint --cache --max-warnings 0",
    "prettier --write"
  ],
  "*.json": [
    "prettier --write"
  ],
}
```

5. The last one file with configuration for IDE (.editorconfig)

```
root = true

[*]
indent_style = tab
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
max_line_length = 120

[*.{json,md,eslintrc,prettierc,lintstagedrc}]
indent_style = space
indent_size = 2

```
