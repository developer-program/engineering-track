# Page Object Pattern

[PageObject](https://martinfowler.com/bliki/PageObject.html ) - Martin Fowler

In Summary:
- PageObject is the interface to the web element
- you should not directly modify HTML
- Interface for writing test to interact with the Web application
- One PageObject Per WebPage
  - with micro frontend becoming more popular it can potentially be per container within a page
- Martin Fowler prefers to have no assertions in page objects

## Page Object in Cypress

We typically create a Page Object in `cypress/support` folder.
If your application going to be big, you can create more folder to categories the Page Object. 

### Create Page Object
In cypress/support/cypressLabPage.js
```javascript
class CypressLabPage {
  enterName(name) {
    cy.contains("What is your name?")
      .parents("[role='listitem']")
      .find("input")
      .type(name);
  }
}

export default new CypressLabPage();
```

### Consuming Page Object
import PageObject

```javascript
import cypressLabPage from "../support/cypressLabPage";

describe("Cypress Lab", () => {
  it("should enter name", () => {
    cypressLabPage.enterName("John");
  });
  ...
}
```

### Lab
1. Create a PageObject for the google form cypress lab(from Inputs)
2. Replace all methods with page object. 
The Spec page should become shorter and more readable.
