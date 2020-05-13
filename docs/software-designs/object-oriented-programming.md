# Object-oriented programming

Object-oriented programming (OOP) is one of the programming paradigms that we commonly use. It is widely popular in large-scale software engineering.

In object-orientated modelling, we use objects to model the real world things that we want to represent inside our programs. OOP envisions software as a collection of cooperating objects rather than a collection of functions or simply a list of commands (as is the traditional / procedural view).
Today, many popular programming languages (such as Java, JavaScript, C#, C++, Python, PHP, Ruby and Objective-C) support OOP.

## Why is OOP useful?

It's a technique to manage a large-scale codebase. It helps to answer the question: given a piece of code/functionality, where should we put it?

OOP strongly emphasizes modularity, thus it promotes greater flexibility and maintainability in programming. Object-oriented code is simpler to develop, easier to understand and extend later on.

## What is involved in OOP?

### Class

- A named concept that represent a group of things with same characteristics
- In ES6 for JavaScript, classes were introduced as a new syntax for defining objects
- This new class syntax is just syntactic sugar. Under the hood it is still using the inheritance method with prototypes which is specific to JavaScript.

```js
//defining an Animal class
class Animal {}
```

### Instance

- When you use the `new` operator to create a new object out of the given class, that object is an instance of the class.
- Declare your classes first before making an instance of it

```js
// creating an instance of the class
const animal1 = new Animal();
```

### Field / Property

- Data that is part of a class
- These properties can be any value (i.e. string, number, function, or even other objects)

### Constructor

- A method that will run immediately on the object after using the `new` operator to create a new object.
- The constructor method is a special method for creating and initializing an object created with a class.
- There can only be one special method with the name "constructor" in a class. A SyntaxError will be thrown if the class contains more than one occurrence of a constructor method.

```js
class Movie {
  constructor(title, price, duration) {
    this.title = title;
    this.price = price;
    this.duration = duration;
  }
}
```

### Method

- A named function or procedure, with or without parameters, that implements some behavior for a class.
- Method in a class should be aligned with the purpose/role/responsibility of the class.

```js
class Theater {
  constructor(name, seats) {
    this.name = name;
    this.seats = seats;
  }

  getAvailableSeats() {
    // return a list of available seats
  }

  markSeatAsBlocked(seatNumber) {
    // block the given seat
  }

  markSeatAsAvailable(seatNumber) {
    // cancel the blocking on the seat
  }

  markSeatAsSold(seatNumber) {
    // mark the seat as sold
  }
}
```

## Abstraction

Abstraction is to create a simple model of a more complex thing, which represents its most important aspects in a way that is easy to work with for our program's purposes.

- Derive generalised concepts from concrete examples
- Ignore/drop irrelevant details
- Give it a name

For example, if we abstract a television console, it will be abstracted to the screen and the box. We ignore the fact that the screen is made up of individual pixels and that the box contains various electronic parts. We also don't know how the color is decided for each pixel or what data is manipulated.

## Encapsulation

Encapsulation is to put data and the operations using/manipulating the data into the same class.

- Some of the implementation details are declared as 'private' and hidden from public interface. (Some languages like Java makes it very easy to declare private fields, but JavaScript did not have straightforward support of it. It is now an [experimental feature in Node 12](https://github.com/tc39/proposal-class-fields).)
- the functionality offered by an instance is only accessed through the public interface.

## Exercise 1

Create a **Car** class which has methods and properties for the following instances:

Car objects:

| car1     | car2        | car3   |
| -------- | ----------- | ------ |
| Green    | Red         | Blue   |
| Mercedes | Toyota      | Proton |
| Gasoline | Electricity | Diesel |
| faster   | fast        | normal |

What kind of methods should the cars have?

## Static methods and fields

Static methods are methods that are callable on the class itself - not on its instances. Static fields are similar, fields that are only callable on the class itself.

```js
class Theatre {
  static SEAT_TYPE = {
    GOLD_CLASS: "GOLD_CLASS",
    NORMAL: "NORMAL",
  };

  static isValidSeat(seat) {
    const seatTypes = Object.values(Theatre.SEAT_TYPE);
    return seatTypes.includes(seat);
  }
}
```

Adding public static fields this way is [an experimental feature (stage 3)](https://github.com/tc39/proposal-class-fields) which is possible in Node 12 and newer versions.

Static methods are used typically to implement behavior that does not pertain to a particular instance. For example, we could design a `Vehicle` class so that it tracks every vehicle it creates. We could then write static methods that return how many vehicles have been created, search for vehicles by their make, etc.

What static methods would you add to your **Car** class?

## Inheritance

A child class may inherit the fields and methods of its parent class.

- An instance of child class is also an instance of the parent class. This is called is-a relationship.
- Sometimes the child class is called a subclass and the parent class is called a super class.
- Inheritance is _transitive_, so a class may inherit from another class which inherits from another class, and so on, up to a base class (typically an Object class or possibly implicit/absent).

### The "extends" keyword

The `extends` keyword causes the inheritance to happen. It will look something like this:

```js
class ChildClass extends ParentClass
```

### Constructor of the child class

If the child class has no constructor then the following "empty" constructor will be generated:

```js
class ChildClass extends ParentClass {
  // generated for extending classes without child constructor
  constructor(...args) {
    super(...args);
  }
}
```

A constructor of the child class can use `super()` to call the constructor of the parent class.

Here is a concrete example on how you can define a child constructor:

```js
class Seat {
  constructor(seatNumber) {
    this.seatNumber = seatNumber;
    this.capacity = 1;
  }


class CoupleSeat extends Seat {
  constructor(seatNumber) {
    super(seatNumber);
    this.capacity = 2;
  }
}
```

See [this javascript.info lesson on class inheritance](https://javascript.info/class-inheritance) for more information.

### Class-based Inheritance vs Prototype-based Inheritance

Class-based Inheritance: a child class inherits methods from its parent/ancestor class.
Prototype-based Inheritance: one object inherits methods from its prototype object.

Some languages (like Java, C#) adopts Class-based Inheritance.

JavaScript actually follows Prototype-based Inheritance, although the class syntax gives us an illusion of class-based inheritance.

In ES5, we would have created a class like this:

```js
function Person(name) {
  this.name = name;
}

var bob = new Person("Bob");
console.log(typeof bob);
console.log(bob.name);
```

The function named Person is a constructor function. This works because in JavaScript, remember that functions are first-class objects. As first-class objects, you can add members to the function.

In ES6, we have syntactic sugar of a "class".

```js
class Person {
  constructor(name) {
    this.name = name;
  }
}

const bob = new Person("Bob");
console.log(bob instanceof Person);
console.log(bob.name);
```

Compare this with object literals in JavaScript:

```js
const bob = {
  name: "Bob",
};
```

Note that these object literals actually has a `__proto__` property which links to the prototype of Object, which means that it was as if you did `const bob = new Object()`. This allows the object literals to inherit methods from the Object prototype.

JavaScript prototypes is a complicated subject. For this coure, we will just use the syntactic sugar syntax of classes. If you would like to know more about JavaScript prototypes, you could read watch videos like [A beginner guide to JavaScript's Prototype](https://www.youtube.com/watch?v=XskMWBXNbp0) and [JavaScript inheritance and the Prototype Chain](https://www.youtube.com/watch?v=MiKdRJc4ooE)

## Exercise 2

Create a parent **Vehicle** class which has child classes **Car** and **Motorcycle**.
The classes should have fields such as `Manufacturer`.
**Motorcycle** class should have an extra field for `gear` because it has a 6th gear.
What kind of fields and methods should be in the parent class?

Read more about this here [in this gist](https://gist.github.com/jim-clark/5d66a759b3842b4a06659d1d73da25b6).

## Vending Machine Problem part 1

Credit: Elson Lim

Freshie is an F&B startup, and the company's want to design an Orange juice vending machine.

The vending machine can take in coins and notes.
It will dispense orange juice when the accumulated sum hits 200 cents.
The vending machine will dispense the orange juice in a plastic cup together with the change.

The vending machine can take in the following currency (in cents).

Coins: 10, 20, 50, 100
Notes: 200, 500

Input: ([], [200])
Output: ["OJ"]

Explanation: \$2 note returns a cup of orange juice with no change

Input: ([50, 100], [200, 200, 200])
Output: ["OJ", 500, 50]

Explanation: Input of 50 and 100 cents
and three \$2 notes (200 used, 550 as change) returns a cup of orange juice and change

Input: ([10, 20, 50, 100], [500])
Output: ["OJ", 200, 200, 50, 20, 10]

Explanation: Input of 680 (200 used, 480 as change) returns a cup of orange juice and change

1. Create vending machine as a class named `VendingMachine`.
2. Decide what functions should we have. You are free to design your own functions.

Possible tests: (You should have more tests than this!)

Try out doing this question with TDD.

```js
beforeEach(() => {
  vendingMachine = new VendingMachine([10, 20, 50, 100, 200, 500]);
});

it("should return change if money is not enough to buy drink", () => {
  vendingMachine.insertMoney([[100], []]);
  expect(vendingMachine.dispenseDrinkAndChange().toEqual([100]);
});

it("should return drink if there is enough money to buy drink", () => {
  vendingMachine.insertMoney([[], [200]]);
  expect(vendingMachine.dispenseDrinkAndChange().toEqual(["OJ"]);
});
```
