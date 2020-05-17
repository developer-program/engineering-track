# Design Patterns

In software engineering, a software design pattern is a general, reusable solution to common problems within a given context.

They are usually tested and proven patterns.

Many of these patterns came from a book named **Design Patterns: Elements of Reusable Object-Oriented Software**, first published in 1994. Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides is now widely known as “The Gang of Four”.

## Content

1. Creational patterns
2. Structural patterns
3. Behavioral patterns

## Creational patterns

Patterns for creating new instances

### Factory pattern

Provides a clear interface to create objects while abstracting away the logic or complexity involved in creating them. You will be able to read data and create the create correct objects.

Imagine that when you want to create a vehicle (instance of a `Car`, `SportsCar` or a `Van`), you will need to add fuel to its FuelTank. Also, you need to give it a `props` object that specifies some customisation like the color of the vehicle.

There will be duplicate code if you are required to do this everytime you want to create a vehicle instance. Thus it make sense to use the Factory pattern.

```js
// FuelTank.js
class FuelTank {
  constructor() {
    amountOfFuel = 0;
  }

  addFuel(fuelToAdd) {
    this.amountOfFuel += fuelToAdd;
  }
}

module.exports = FuelTank;
```

```js
// vehiclesFactory.js
const FuelTank = require("./FuelTank");
const vehiclesToCreate = ["car", "sports car", "van"];

const vehiclesFactory = (type) => {
  const fuelTank = new FuelTank();
  fuelTank.addFuel(100);
  const props = { color: "blue" };

  switch (type) {
    case "car":
      return new Car(fuelTank, props);
    case "van":
      return new Van(fuelTank, props);
    case "sports car":
      return new SportsCar(fuelTank, props);
    default:
      throw new Error("invalid vehicle type");
  }
};

const vehiclesCreated = vehiclesToCreate.map((type) => vehiclesFactory(type));

module.exports = vehiclesFactory;
```

A factory pattern can also be used to create private variables (encapsulation) through closure. The private variable's scope is kept within the function which is created by the factory.

```js
const createSayHi = (message) => {
  return () => console.log(`Hi ${message}`);
};

const sayHiToAlice = createSayHi("Alice");
const sayHiToBob = createSayHi("Bob");

sayHiToAlice(); // Alice
sayHiToBob(); // Bob
```

### Builder pattern

The builder pattern can help us to construct complex objects. It helps separate object construction from its representation, which will help us reuse this to create different representations. Normally, we will construct a complex object step by step and the final step will return the object.

For example,

```js
class Character {
  constructor(name, gender, weight, height) {
    this.name = name;
    this.gender = gender;
    this.weight = weight; // in kg
    this.height = height; // in cm
  }
}

pikachu = new Character("pikachu", "female", 20, 70);
```

It is possible that we will easily mix up weight and height during instantiation of the character, since they are both numbers. We also might want to limit the value of `gender` to only `female` and `male`. How can we use the builder pattern to help us contruct a character instance step by step?

```js
const Character = require("./Character");

class CharacterBuilder {
  constructor(name) {
    this.name = name;
  }

  setGender(gender) {
    if (!["female", "male"].includes(gender)) {
      throw new Error("Invalid gender");
    }
    this.gender = gender;
    return this;
  }

  setWeight(weight) {
    this.weight = weight;
    return this;
  }

  setHeight(height) {
    this.height = height;
    return this;
  }

  build() {
    if (!("gender" in this)) {
      throw new Error("Gender is missing");
    }
    if (!("weight" in this)) {
      throw new Error("Weight is missing");
    }
    if (!("height" in this)) {
      throw new Error("Height is missing");
    }
    return new Character(this.name, this.gender, this.weight, this.height);
  }
}

const leon = new CharacterBuilder("Leon")
  .setGender("male")
  .setWeight(20)
  .setHeight(70)
  .build();

console.log(leon);
```

Notice that the `this` is returned so that `setWeight` and `setHeight` are chainable.

### Singleton

Create a instance that can be used in multiple places.
It can use as enum, or data stores like in redux reducer.

```js
// directions.js
const DIRECTIONS = Object.freeze({
  NORTH: "North",
  SOUTH: "South",
  EAST: "East",
  WEST: "West",
});
```

Another way of implementing Singleton:

```js
class DataStore {
  static instance = null;

  constructor() {
    if (!DataStore.instance) {
      DataStore.instance = this;
    } else {
      throw new Error("should only have one DataStore");
    }
  }

  static getInstance() {
    if (!DataStore.instance) {
      DataStore.instance = new DataStore();
    }
    return DataStore.instance;
  }
}

const instance1 = DataStore.getInstance();
const instance2 = DataStore.getInstance();

console.log("Same instance? " + (instance1 === instance2));
```

## Structural patterns

Structural patterns help to bridge or connect different objects to work together.

### Proxy

In common English, a proxy is defined as the authority to represent someone else.

As a design pattern, it involves an object acting as a subsitute or placeholder for another object. The proxy allows us to control access or perform something before / after the request is made to the original object, preventing objects from directly talking to each other.

The proxy should have the exact interface as the original object, since it is can directly subsitute the original.

![Substitute pokemon](https://cdn.bulbagarden.net/upload/0/0a/Substitute_VIII.png)
Picture from Bulbagarden.

(Substitute pokemon)

```javascript
class Person {
  constructor(name) {
    this._name = name;
  }

  set name(newName) {
    // substitute for _name property
    this._name = newName.toLowerCase(); // controls access
  }

  get name() {
    return `my name is ${this._name}`;
  }
}
```

Another example is the Google Maps Geocoding service. In geocording, you can provide a location and it will return its latitude/longitude. We can implement a Proxy object to speed up the service.

the proxy can help us to:

1. caches frequently requested location
2. accumulate queries and reduce number of requests to original service

### Adaptor

Creates a middle layer to transform / translate one component to be compatible with another

```js
class Shop {
    collect(usd) {
        this.register.addUSD(usd);
    }
}

const JapaneseWallet {
    constructor(yen) {
        this.yen;
    }

    pay(amountInYen) {
        if(this.yen >= amountInYen) {
            this.yen -= amountInYen;
            return this.amountInYen;
        }
    }
}

class JapaneseWalletAdapter {
    yenPerUsd = 107.09;
    attachJapaneseWallet(wallet) {
        this.wallet = wallet;
    }

    pay(amountInUsd) {
        return this.wallet.pay(amountInUsd * yenPerUsd)
    }
}

const japaneseWallet = new JapaneseWallet(2000);
const japaneseWalletAdapter = JapaneseWalletAdapter();
japaneseWalletAdapter.attach(japaneseWallet);

const shop = new Shop();
shop.collect(japaneseWalletAdapter.pay(5));
```

Another example is that if you have an app that works with data in XML format, but after development, you need to use a library that can only work with JSON. You can write an Adapter that translate JSON data to XML format and vice versa.

### Facade

In common English, facade is one exterior side of a building, usually the front. It is a loan word from French.

![Facade](https://media.geeksforgeeks.org/wp-content/uploads/facadeA.png)

Picture from GeeksForGeeks.

As a design pattern, Facade pattern helps us to create a new, simplfied and "beautiful" interface for complex modules.

```javascript
class OrderService() {
    takeOrder(item) {
        //send order to DB
    }
}

class PaymentService() {
    handlePayment(paymentType) {
        //connect with bank etc
    };
}

class McDonaldKiosk() { //facade
    addItem(item) {
        orderService.takeOrder(item);
    }

    checkout() {
        const paymentType = await waitForPaymentType();
        paymentService.handlePayment(paymentType)
    }
}
```

### Composite

Combine objects of similar signature to a parent object for representation

```javascript
class Song {
    constructor(title, duration, songBinary) {
        this.title = title;
        this.duration: duration;
        this.songBinary = songBinary;
    }
}

class Playlist { // composite for songs
    constructor() {
        this.songList = [];
    }

    addSong(song) {
        this.songList.push(song);
    }

    playSongs() {
        this.player.play(this.songList);
    }
}
```

Front-end libraries like React (and Vue) also use this composite pattern to build reuseable components. Multiple components together can be combined to create a new component. These components with the `render()` method can then be rendered out by a composite that combine these components to form a web page.

### Comparisons

Adapter provides a different interface to the wrapped object, Proxy provides it with the same interface.

## Behavioral pattern

Identify common communication patterns among objects and realize these patterns.
By doing so, these patterns increase flexibility in carrying out this communication.

### Command

Create an interface that on input, invoke certain methods or functions relating to it.

```js
class Television {
  setChannel(channel) {
    this.channel = channel;
  }

  setVolume(volume) {
    this.volume = volume;
  }

  getVolume() {
    return this.volume;
  }
}

class Remote {
  addCommand(commandName, command) {
    commandMap.set(commandName, command);
  }

  execute(commandName) {
    commandMap.get(commandName).execute();
  }
}

const tv = new Television();
const remote = new Remote();

remote.addCommand("button 1", { execute: () => tv.setChannel(1) });
remote.addCommand("button 2", { execute: () => tv.setChannel(2) });
remote.addCommand("add volume", {
  execute: () => tv.setVolume(tv.getVolume() + 1),
});
remote.addCommand("decrease volume", {
  execute: () => tv.setVolume(tv.getVolume() - 1),
});

remote.execute("add volume");
```

### Observer

![observer pattern](https://gblobscdn.gitbook.com/assets%2F-LBJBL3Fj_tcfkvqLj9P%2F-LCW3Peyo9sS-PfvMbi8%2F-LCW3Upla259bzKfYGzk%2Fobserver_pattern_diagram.png?alt=media)

Picture from https://refactoring.guru/design-patterns/observer

It provides a way to react to events happening in other objects without coupling to their classes.

The observer pattern defines a one-to-many dependency between objects so that when the subject (a.k.a. publisher / observerable) object changes state, all its observers (a.k.a. subscribers) are notified and updated automatically.

Subjects (Publishers) and Observers (Subscribers) know nothing about each, other than each others' interface (i.e. the methods that they have).
You can push or pull data from the Observable when using the pattern (pull is considered more “correct”).

This is common in GUI programming, where we often have to subscribe to click events etc.

### Strategy

This pattern is used to handle similar actions applied to different data, where every data type needs special handling.

The original class, called `context`, must have a field for storing a reference to a strategy. The `context` delegates the work to a strategy object instead of executing it on its own.

```javascript
class ShapeCalculations {
    constructor(shape, strategy) {
        this.shape = shape;
        this.strategy = strategy;
    }

    calculateArea() {
        this.strategy.calculateArea(this.shape);
    }

    calculatePerimeter() {
        this.strategy.calculatePerimeter(this.shape);
    }
}

const circleStrategy {
    calculateArea: function (circle) {
        return Math.PI * Math.pow(circle.radius, 2)
    }

    calculatePerimeter: function (circle) {
        eturn Math.PI * 2 * circle.radius;
    }
}

const circle = new Circle(5);
const circleCalculate = new ShapeCalculations(circle, circleStrategy);

circleCalculate.calculateArea();
```

## Exercises

### Vending Machine with ContainerFactory

Use the factory pattern in vending machine for deciding whether to make a Cup or Can. We can make a class called `ContainerFactory`.

### Vending Machine with commands

Freshie the startup wants to have a command system for the vending machine. 
Using the Command pattern, implement a command system for the Vending Machine.

Suggested commands:
1. Dispense item and change
2. Change price of an item
3. Return / refund money (without buying any item)

