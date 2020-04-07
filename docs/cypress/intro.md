# Cypress

## E2E test

## What is Cypress

Cypress is a very popular end to end automation test runner. Cypress takes prides in being easy to setup, write and run your test in a way much more stable than Selenium. Cypress give you the flexibility of selecting, click, dragging, wait for elements. All these capabilities make you it easy to test your application in great details.

Checkout Cypress intro video [here](https://www.cypress.io/)

## Cypress Vs Selenium

### Cypress

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
