# Node express setup with typescript

Let’s initialize the project, open a terminal and write this command

```jsx
npm init
```

Install development dependencies

```jsx
npm i -D typescript ts-node-dev @types/node rimraf
```

Setup typescript configuration

```jsx
npx tsc -init
```

Update `tsconfig.json` file with these configurations:

```jsx
{
  "exclude": ["node_modules", "dist", "src/**/*.test.ts"],
  "include": ["src/**/*"],
  "compilerOptions": {
    "types": ["node", "express"],
    "target": "ESNext",
    "module": "CommonJS",
    "rootDir": "./src",
    "moduleResolution": "Node",
    "typeRoots": ["./node_modules/@types"],
    "sourceMap": true,
    "outDir": "dist/",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "strictNullChecks": true,
    "skipLibCheck": true,
    "baseUrl": "./src",
    "paths": {
      "@core/*": ["core/*"],
    }
  }
}
```

We can use a specific version of node for our server, create a new `.nvmrc` file in the root of your project, or you can use this command to generate it.

```jsx
node -v > .nvmrc
```

To run the application, we need to set some commands in `package.json`.

```jsx
{
  ...
 "scripts": {
  "dev": "ts-node-dev --respawn --clear --transpile-only --ignore-watch node_modules ./src/app.ts",
  "build": "rimraf ./dist && tsc",
  "start": "node dist/app.js",
 },
  ...
}
```

`ts-node-dev`: This is a tool that allows you to run TypeScript files directly in Node.js without the need to compile the code beforehand. It provides additional functionalities for a convenient development environment.

- `-respawn`: This flag instructs `ts-node-dev` to restart the application when changes in source files (like `.ts` files) are detected. It's useful for keeping the application running during development and automatically applying changes made.
- `-clear`: This flag clears the terminal screen before each restart of the application. It provides a cleaner development experience by removing previous terminal output.
- `-transpile-only`: This flag tells `ts-node-dev` to only transpile TypeScript code to JavaScript without performing any type checking. This speeds up the transpilation process by skipping type checking during development, which can be useful in large projects where type checking can be a costly operation.
- `-ignore-watch node_modules`: This flag tells `ts-node-dev` to ignore changes made in the `node_modules` folder. Since changes in this folder typically don't require restarting the application during development, this helps to avoid unnecessary restarts and improves performance.

**Environment variables**

```jsx
npm i dotenv env-var
```

Let’s create two files `.env` and `.env.template` in the root of the project, in the `.env` file we can define these variables

```
PORT=3000
DEFAULT_API_PREFIX=/api/v1
NODE_ENV=development
```

**ESLint**

```jsx
npm i -D eslint
```

Initialize ESLint config with this command

```jsx
npx eslint --init
```

Update `.eslintrc.json` file with these configurations:

```jsx
{
 "env": {
  "es2021": true,
  "node": true
 },
 "extends": [
  "standard-with-typescript",
  "plugin:prettier/recommended",
  "eslint:recommended",
  "plugin:@typescript-eslint/recommended"
 ],
 "plugins": ["@typescript-eslint", "import", "prettier"],
 "parserOptions": {
  "ecmaVersion": "latest",
  "sourceType": "module",
  "project": "./tsconfig.json"
 },
 "ignorePatterns": ["src/**/*.test.ts"],
 "rules": {
  "prettier/prettier": "error",
  "camelcase": "error",
  "spaced-comment": "error",
  "quotes": ["error", "single"],
  "no-duplicate-imports": "error",
  "no-unused-vars": "off",
  "no-magic-numbers": "off",
  "@typescript-eslint/no-unused-vars": "error",
  "@typescript-eslint/explicit-function-return-type": "error",
  "@typescript-eslint/strict-boolean-expressions": "off",
  "@typescript-eslint/no-extraneous-class": "off",
  "@typescript-eslint/no-magic-numbers": "error"
 }
}
```

Create `.eslintignore` file with this content

```jsx
node_modules/
dist/
jest.config.ts
```

**Prettier**

```jsx
npm i -D prettier eslint-plugin-prettier eslint-config-prettier
```

Create `.prettierrc` file with this content

```jsx
{
  "arrowParens": "always",
  "bracketSpacing": true,
  "insertPragma": false,
  "printWidth": 120,
  "proseWrap": "preserve",
  "quoteProps": "as-needed",
  "requirePragma": false,
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "none",
  "useTabs": true,
  "endOfLine": "auto",
  "overrides": [
    {
      "files": ".prettierrc",
      "options": { "parser": "typescript" }
    }
  ]
}
```

Create `.prettierignore` file with this content

```jsx
.yarn
dist
node_modules
.prettierrc
```

**Path aliases with TypeScript in Node.js**

Update tsconfig.json

```jsx
{
  "baseUrl": "./src",
  "paths": {
    "@modules/*": ["rest/modules/*"],
    "@services/*": ["services/*"]
  }
}
```

Install module-alias package

```jsx
npm i --save module-alias
```

This module registers the path aliases in the compiled JS files. Therefor we need to make some changes to our package.json

```jsx
"_moduleAliases": {
    "@modules": "dist/rest/modules",
    "@services": "dist/services"
}
```

Add the following line at the top of your startup file

```jsx
import "module-alias/register"
```