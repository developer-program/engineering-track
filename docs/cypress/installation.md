# Installation

##

## clone the repo

We typically maintain our cypress test in our UI repo or create a new app

### Prerequisite

Install [Node](https://nodejs.org/en/)

### Cloning

```sh
git clone repo
```

### Create new project

```javascript
mkdir <dir>
cd <dir>
npm init
```

or React Project

```javascript
npx create-react-app <project>
```

## Install Cypress

The easiest way to install cypress is treating it as a developer dependencies

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

## Ignore example

create a cypress.json in the root folder, if cypress didn't already create one for you.

```bash
touch cypress.json
```

Cypress.json

```JSON
{
  "ignoreTestFiles": "**/examples/*.js",
}
```

Rerun cypress

```sh
npm run cy:open
```

Now all examples files should be ignored

## Lab

1. Create a new project folder, install and run Cypress example files
2. Configure Cypress to ignore test in the examples
