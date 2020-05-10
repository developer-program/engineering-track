# Cypress

## E2E test

Testing of workflows of an application, that involves all the components working together in an environment similar to the environment the user used.

- using real servers
- do actual API calls
- using database on backend
- login like a user if required

## What is Cypress

Cypress is a very popular end to end automation test runner. Cypress takes prides in being easy to setup, write and run your test in a way much more stable than Selenium. Cypress give you the flexibility of selecting, click, dragging, wait for elements. All these capabilities make you it easy to test your application in great details.

Checkout Cypress intro video [here](https://www.cypress.io/)

## Cypress Vs Selenium

### The often comparision between Cypress and Selenium

Pros

- Cypress is standalone(only require node)
- Very fast
- Stable thanks to electron and preinstalled setup that is already tested.
- Low learning curve
- Ability to store mock response and reply using the saved response

Cons

- Only Javascript(but is the web language)
- Only support Chrome, Electron, Firefox, Edge.
- No mobile support yet
- bad with iframes

## Selenium

Pros

- Can use any language(need configure properly)
- Can configure to test mobile using Appium. (IMHO the only reason why you want to use selenium other than you already know it) - iOS: XCUITest - Android: UIAutomator, Selendroid, Espresso
  Cons

- require additional setup(lanugague, test framework, web drivers)
- slower than cypress
- Can be more challenging to learn

- also bad with iframes

## Conclusion

For web application, Cypress is easier to use and learn for web application and tends to run faster and more also more stable.
For mobile application, you might need to looks elsewhere such as Detox for React Native or Appnium.
