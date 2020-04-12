# Inputs

In the examples, we are going to go thru the more common inputs, and test their value after interacting with the inputs.

To demo inputs, we are going to use [bootstrap website](https://getbootstrap.com/docs/4.4/components/forms/).

## Content

1. Text and Password
2. Dropdown
3. Slider
4. Radio
5. Checkbox

## Text and Password

Text input is the most common when dealing with forms. To type into an input box simply use `.type` to let cypress knows you want to type into a text input. If cypress cannot type into the box, cypress will throw you an error. Once we type into an inbox, the property `value` will get updated. using `.should("have.value", <value you expect>)`. You can find the expected input.

```html
<form>
  <div class="form-group">
    <label for="exampleInputEmail1">Email address</label>
    <input
      type="email"
      class="form-control"
      id="exampleInputEmail1"
      aria-describedby="emailHelp"
    />
    <small id="emailHelp" class="form-text text-muted"
      >We'll never share your email with anyone else.</small
    >
  </div>
  <div class="form-group">
    <label for="exampleInputPassword1">Password</label>
    <input type="password" class="form-control" id="exampleInputPassword1" />
  </div>
</form>
```

```js
it("enter email and password", () => {
  cy.get("#exampleInputEmail1").type("email@gmail.com");
  cy.get("#exampleInputEmail1").should("have.value", "email@gmail.com");
});
```

Input with type `password` works like `text`. The only notable difference is that browser will hide the password field on the UI.

```js
it("enter password", () => {
  cy.get("#exampleInputPassword1").type("password");
  cy.get("#exampleInputPassword1").should("have.value", "password");
});
```

## Dropdown

Another popular input is the dropdown box. In Html, we can use the `select` element and input options for the user to select.
To invoke a selection, we can use the `select` command with the selection as the argument. Cypress will fail if the option is not available.

```html
<div class="form-group">
  <label for="exampleFormControlSelect1">Example select</label>
  <select class="form-control" id="exampleFormControlSelect1">
    <option>1</option>
    <option>2</option>
    <option>3</option>
    <option>4</option>
    <option>5</option>
  </select>
</div>
```

```js
it("select dropdown", () => {
  cy.get("#exampleFormControlSelect1").select("3");
  cy.get("#exampleFormControlSelect1").should("have.value", "3");
});
```

## Slider

The slider is the least common among the input we going thru, and cypress doesn't have a handy command for. Is possible to invoke cypress to drag the slider to a position or based on duration but it this is difficult and flaky. It will be much better to directly change the value. We can do that using the `invoke` command which differently gives access to the properties of an element. In the example below, we use `invoke` to change the value to `25` and trigger to cause a javascript event from happening. The `change` event gets triggers on

```html
<form>
  <div class="form-group">
    <label for="formControlRange">Example Range input</label>
    <input type="range" class="form-control-range" id="formControlRange" />
  </div>
</form>
```

```js
it("changing slider", () => {
  cy.get("#formControlRange").invoke("val", "25").trigger("change");
  cy.get("#formControlRange").should("have.value", "25");
});
```

## Radio and checkbox.

Radio and checkbox can use both `click` and `check`. You cannot uncheck a radio button by selecting it again but you can uncheck any checkboxes.

### Radio

There are 2 ways to select the radio button by `click` and by `check`.

```html
<div class="form-check">
  <input
    class="form-check-input"
    type="radio"
    name="exampleRadios"
    id="exampleRadios1"
    value="option1"
    checked
  />
  <label class="form-check-label" for="exampleRadios1">
    Default radio
  </label>
</div>
<div class="form-check">
  <input
    class="form-check-input"
    type="radio"
    name="exampleRadios"
    id="exampleRadios2"
    value="option2"
  />
  <label class="form-check-label" for="exampleRadios2">
    Second default radio
  </label>
</div>
```

using `click` to trigger the radio button.

```js
it("click on radio button", () => {
  cy.get("#exampleRadios1").should("be.checked");
  cy.get("#exampleRadios2").should("not.be.checked");

  cy.get("#exampleRadios2").click();

  cy.get("#exampleRadios1").should("not.be.checked");
  cy.get("#exampleRadios2").should("be.checked");
});
```

using `check` instead of `click` to trigger the radio button. The radio button cannot be unchecked.

```js
it("check radio button", () => {
  cy.get("#exampleRadios1").check();

  cy.get("#exampleRadios1").should("be.checked");
  cy.get("#exampleRadios2").should("not.be.checked");
});
```

### Checkbox

There are 2 ways to select the checkbox by `click` and by `check` and you can unselect them by `click` the checkbox again or by using `uncheck`.

```html
<div class="custom-control custom-checkbox">
  <input type="checkbox" class="custom-control-input" id="customCheck1" />
  <label class="custom-control-label" for="customCheck1"
    >Check this custom checkbox</label
  >
</div>
```

```js
it("click on checkbox", () => {
  cy.get("#customCheck1").should("not.be.checked");

  cy.get("#customCheck1").parent().find(".custom-control-label").click();
  cy.get("#customCheck1").should("be.checked");

  cy.get("#customCheck1").parent().find(".custom-control-label").click();
  cy.get("#customCheck1").should("not.be.checked");
});
```

Using `check` and `uncheck` on checkboxes. Sometimes, cypress may think that another element is blocking the checkbox. You can use the `{ force: true }` to force a click on a checkbox.

```js
it("checked and uncheck checkbox", () => {
  cy.get("#customCheck1").check({ force: true });
  cy.get("#customCheck1").should("be.checked");

  cy.get("#customCheck1").uncheck({ force: true });
  cy.get("#customCheck1").should("not.be.checked");
});
```
