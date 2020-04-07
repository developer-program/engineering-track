# Installation

## clone repo

We typically maintain our cypress test in our UI repo.

```sh
git clone repo
```

## Install Cypress

The easiest way to isnstall cypress is treating it as a developer dependencies

```sh
npm i -D cypress
```

## Add script in package.json

```json
  "scripts": {
    "cy:open": "cypress open"
  },
```

## Start cypress

```sh
npm run cy:open
```

## Configurations

create a cypress.json in the root folder

```
touch cypress.json
```

Cypress.json

```JSON
{
	"ignoreTestFiles": "examples/*.js",
}
```

Rerun cypress

```sh
npm run cy:open
```

Now all examples files should be ignored

## ESLINT

We will need to let eslint know we are using cypress and support the syntax.

First install eslint-plugin-cypress

```sh
npm install eslint-plugin-cypress --save-dev
```

Append the following config in `eslint.json`

```JSON
{
  "env": {
    "cypress/globals": true
  },
  "extends": [
    "plugin:cypress/recommended"
  ],
  "plugins": ["cypress"],
}
```
