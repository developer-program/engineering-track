# Creating a cypress test

Create a new file
`cypress/integration/hello-cypress.js`

If you have cypress open, you should see the file in the left runnable window.

1. First, add a describe block like in jest

```js
describe("Hello Cypress", () => {});
```

2. We can ask cypress to visit the website we want to visit

```js
it("visit the who is next site", () => {
  cy.visit("http://localhost:3000");
});
```

3. our first test that simplies checks that cypress can find the test

```js
it("should render default Learn React text", () => {
  cy.contains("Learn React");
});
```

![test runner](_media/cypress-hello.png.png)

## Moving the URL out to the environment variable

There are various ways to add environment variables in cypress.
The easiest is thru package.json

Modify the `cy:open` script in package.json

```json
"cy:open": "cypress open --env host=http://localhost:3000"
```

To run cypress on test/prod environment, all we need to do is change the host.

If you have lots of other environment variables, you can also choose to set them in a `cypress.env.json` file. Remember to add this file in `.gitignore` and document in Readme.md to help new developers set up their machine.

In cypress.env.json

```JSON
{
  "host": "http://localhost:3000"
}
```

## Lab

1. Using your personal project, write a cypress test that visits localhost in the application and look for the title of the website.
