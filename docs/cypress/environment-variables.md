# Cypress Environment Variables

Environment variables is useful to change where we want to run the code and store information such as username and password. 

## Access environment variable
```javascript
Cypress.env(‘USERNAME’);
Cypress.env(‘PASSWORD’);
```

## Store in configuration files

I do not recommend this as we want to check in the configurations file that contains our `ignoreTestFiles` and viewport settings but we do not want to commit environment specific variables.

```json
{
...,
“env”: {
“USERNAME”: “johnsmith”,
“PASSWORD”: “password”
}
}
```

## Storing in cypress.env.json

Create a `cypress.env.json` in root directory
```json
{
“USERNAME”: “johnsmith”,
“PASSWORD”: “password”
}
```

## Add to Machine’s environment variable

- Great when running on remote server
- Require prefixing with `CYPRESS_`

### On Mac, Linux, Window’s using Git Bash
```sh
export CYPRESS_USERNAME=johnsmith
export CYPRESS_PASSWORD=password 
```

### Retriving using machines environment variable

Ignore the `CYPRESS_`

```javascript
Cypress.env(‘USERNAME’);
Cypress.env(‘PASSWORD’);
```

### On Powersehll

```
set USERNAME “USERNAME”
```

## As an argument

### Setting env with `--env` and comma seperated `,`
` “cy:open”: “cypress open --env host=http://localhost:3000,value=\”Learn React\""`

### Code
```javascript
it(“visit the create-react-app default page”, () => {
cy.visit(Cypress.env(“host”));
cy.contains(Cypress.env(“value”));
});
```

## Setting BaseUrl

The above examples use `cy.visit(“http://localhost:3000”);`
This will allow cypress to prefix the baseUrl when we use `cy.visit()` or `cy.request()`.

In `Cypress.json`
```json
{
“baseUrl”: “http://localhost:3000”,
“ignoreTestFiles”: “**/examples/*.js”
}
```

```javascript
it(“visit the create-react-app default page”, () => {
cy.visit(“”);
cy.contains(“Learn React”);
});
```

## Order of priority

Where the same environment variable is defined in multiple places
How you define it will determine the priority.

Top to bottom(Lowest)
- `cypress open --env USERNAME=John`
- `export USERNAME=John && cypress open`
- In cypress.env.json
- In cypress.json under `"env"`

Override with `export CYPRESS_BASE_URL=xxx && cypress open`

## Lab

Console.log different environment variables created using different methods
1. Create an environment variable with name `FROM_CONFIG` with value “value comes from cypress.json”
2. Create an environment variable with name `FROM_ENV_FILE` with value “value comes from cypress.env.json”
3. Create an environment variable with name `CYPRESS_FROM_EXPORT` with value “value comes from exort”
4. Create an environment variable with name `FROM_INIT` with value “value comes from --init”
5. Print out the environment variables using `console.log(Cypress.env(<name of variable>))`
6. Add baseUrl of `http://locahost:3000` in `cypress.json`
7. Start another instance of express, it should be running on `http://localhost:3001`
8. Add an environment variable `CYPRESS_BASE_URL=http://localhost:3001` and run `npm run cy:open`. The test should now be running on `localhost:3001`