# Eslint

Eslint makes code easy to read, help with code consistency and find problems before you run any code.

## Eslint react

Create an eslint configuration file. This will show you some console options which you can select default behaviors such as choosing `react`, `typescript` and file type for the configuration such as `javascript` and `JSON`. We will assume you choose `JSON` but converting to other format is intuitive.

```bash
npx eslint --init
```

### Update react version

1. Try running React App. 
2. If an error occurs, likely is because the current version of React from `create-react-app` expects `eslint@6.6.0`
3. Update eslint version using `npm i -D eslint@6.6.0`

### setup environment

Open `.eslintrc.json`

Under `env`, add, `"node": true` and `"jest": true`.
This will remove the complaining of `describe`, `it`, `test` and `process.env` not being created before used

```json
  "env": {
    ...
    "jest": true,
    "node": true
  },
```

### Updating react version

Detect will let eslint find the version from `package.json`

```json
  "settings": {
    "react": {
      "version": "detect"
    }
  }
```

## Prettier

Prettier is a set of default styling rules that are pretty well configured. Eslint doesn't have an opinion on how you style your code, is just a platform. Prettier is an opinion.
There are others configurations available such as [Airbnb eslint conifg](https://www.npmjs.com/package/eslint-config-airbnb) and more friendly version [Google eslint config](https://www.npmjs.com/package/eslint-config-google).

### Installation

```sh
npm install --save-dev prettier eslint-config-prettier eslint-plugin-prettier
```

### extend

```json
  "extends": [
    ...
    "plugin:prettier/recommended",
]
```

### Overwrite rules

Create `.prettierrc` in root directory
```json
{
  "singleQuote": true
}
```

## Cypress

### Installation

```sh
npm install eslint-plugin-cypress --save-dev
```

### Allow Cypress env variables

```json
 "env": {
    ...,
    "cypress/globals": true
  },
```

### Add plugin

```
  "plugins": [..., "cypress"],

```

### Add recommended rules

```
  "extends": [
    ...,
    "plugin:cypress/recommended"
  ],
```

## Scripts to run cypress

```package.json
 "scripts": {
    ...
    "lint": "eslint src/",
    "cy:lint": "eslint cypress/",
    "lint": "npm run app:lint && npm run cy:lint",
  },
```

## Ignore file

Running the lint command and you will see eslint throwing us lots of error on the default cypress configurations. We can ignore the example files given by cypress and fix the rest by commenting away, changing to quotes and adding semi.

Create a new file in the root directory `.eslintignore`

```.eslintignore
cypress/integration/examples
```

## Lab

1. configure eslint
2. configure eslint for react
3. configure eslint for cypress
4. fix all eslint errors
