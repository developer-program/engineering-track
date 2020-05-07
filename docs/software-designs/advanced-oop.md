# Advanced OOP

## Polymorphism

Polymorphism means "One name, with many forms."

There are two types of polymorphism. The first type of polymorphism involves a child class overriding a parent class method, by having a method or property that is of the same name as the one in the parent class.

The second polymorphism involves method overloading whereby the class has methods of the same name but with different parameters (Methods with the same name but different parameters have a different _signature_).

However, JavaScript does not support method overloading natively. If there are two methods with the same name, JavaScript will only consider the last defined function and override the first one. See [this website for more information](https://www.codeproject.com/Articles/797997/JavaScript-Does-NOT-Support-Method-Overloading-Tha).

### Overriding

Overriding is the first type of polymorphism.

By default, all methods that are not specified in the child class are taken directly “as is” from the parent class.

But if we specify a method in the child class with the same name, this will override the method that was inherited from the parent class.

```js
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }

  stop() {
    this.speed = 0;
  }
}

class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }

  // this overrides the method inherited from the Animal class
  stop() {
    this.hide();
  }
}
```

You also can override properties / fields:

```js
class Seat {
  constructor(seatNumber) {
    this.seatNumber = seatNumber;
    this.status = SEAT_AVAILABLE;
    this.capacity = 1;
  }
}

class CoupleSeat extends Seat {
  constructor(seatNumber) {
    super(seatNumber);
    this.capacity = 2;
  }
}
```

### The "super" keyword

Previously, we used `super()` to call the constructor of the parent class in the child class. In the child class, you can also call methods of the parent class with the `super` keyword. These methods might have been overriden in the child class, but `super` makes it possible to call the parent's methods too.

```js
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }

  stop() {
    this.speed = 0;
  }
}

class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }

  stop() {
    // call parent's stop method which was overriden
    super.stop();
    this.hide();
  }
}
```

## Why is polymorphism and inheritance useful?

Let's say we have three animals, Dog, Cat and Lion. We want them to be able to woof, meow or roar respectively.

```js
class Dog {
  woof() {
    console.log("i'm a dog, hear me woof!");
  }
}

class Cat {
  meow() {
    console.log("i'm a cat, hear me meow!");
  }
}

class Lion {
  roar() {
    console.log("i'm a lion, hear me roar!");
  }
}
```

We did not design the above three classes with polymorphism in mind. Thus we are unable to generalise the following code that will make all three classes woof, meow or roar.

```js
var dog = new Dog();
var cat = new Cat();
var lion = new Lion();
var animals = [dog, cat, lion];

// this for-loop shows how messy things can get when we don't design
// our classes with polymorphism in mind.
for (let i = 0; i < animals.length; i++) {
  var currentAnimal = animals[i];
  if (currentAnimal.constructor === Dog) {
    currentAnimal.woof();
  } else if (currentAnimal.constructor === Cat) {
    currentAnimal.meow();
  } else if (currentAnimal.constructor === Lion) {
    currentAnimal.roar();
  }
}
```

What happens if a new animal is required?

However, if we designed the three classes with polymorphism in mind, we are able to produce cleaner and easily extensible code.

Steps to achieving polymorphism:

- Make Dog, Cat and Lion extend Animal
- Make Dog, Cat and Lion implement the same method: makeSound()

```js
class Animal {
  makeSound() {
    // do nothing, because we can't actually implement this method.
    // think about it. what sound does Animal make?
  }
}
class Dog extends Animal {
  makeSound() {
    console.log("i'm a dog, hear me woof!");
  }
}

class Cat extends Animal {
  makeSound() {
    console.log("i'm a cat, hear me meow!");
  }
}

class Lion extends Animal {
  makeSound() {
    console.log("i'm a lion, hear me roar!");
  }
}

// Wow! polymorphism just made our life so much simpler!
// Now we can scale nicely to a circus with many more types of animals with ease!
for (let i = 0; i < this.animals.length; i++) {
  this.animals[i].makeSound();
}
```

## Composition

Each object is a building block, and we can build more complex classes with other classes/objects.
Imagine that our animals now need a trainer. To create an instance of a trainer, we need to implement a Trainer class. Our implementation can be simplified by using composition.
As before, let's look at an implementation without composition, before we look at a better implementation with composition.

```js
class Trainer {
  woof() {
    console.log("i'm a dog, hear me woof!");
  }

  meow() {
    console.log("i'm a cat, hear me meow!");
  }

  roar() {
    console.log("i'm a lion, hear me roar!");
  }

  makeAnimalSound() {
    this.woof();
    this.meow();
    this.roar();
  }
}
```

There are several problems with this implementation:

1. Duplication.
   We had to rewrite the logic for woof(), meow() and roar() in the Trainer class when they already exist in the Dog, Cat and Lion classes.

2. Incorrect modelling.
   Trainer should not know how to meow/woof/roar
   What if the method was eat()? Trainer now needs to eat raw meat like a lion! Yikes!

Object composition to the rescue!

Steps to achieving composition:
Pass in the animal objects into the constructor of the Trainer class (that's why it's called composition. We are composing trainers using other objects)

Rely on the animal object's methods (e.g. dog.makeSound())!

```js
class Trainer {
  constructor(animals) {
    this.animals = animals;
  }

  makeAnimalSound() {
    for (let i = 0; i < this.animals.length; i++) {
      const animal = this.animals[i];
      animal.makeSound();
    }
  }
}
```

### Composition vs inheritance

When you need to reuse some logic, there are at least two choices:
 - via Inheritance. You can define the logic in a parent class and all children class can inherit it and reuse it.
 - via Composition. You can define the logic in a helper class and then whoever need that function can keep a reference to that helper and reuse it.


In many cases, reusing logic via composition is the preferred approach because it's more flexible. You may sometimes hear an advice like "favoring composition over inheritance".

However, that does not mean we should always avoid Inheritance. That's still a valid technique in OOP.

Here is a rule of thumb to help you decide when to use Inheritance and when to use Composition.

You should declare a class B inherits from class A only when there is a true **is-a relationship** between the two classes.This means, don't let a class inherit another parent class just because you need to reuse some logic in parent class. Code reuse via composition could be a better choice here.

You should compose class B with class A (i.e. keeping a reference to class A in class B) when there is a **has-a relationship** between the two classes.

## Problems with OOP

### Mutable state

OOP has mutable state, means that given some time, after the project starts evolving, there will be many side-effects created by the code to manipulate state. Often, the state becomes difficult to maintain.

Read more about how people think that [OOP could be a disaster](https://medium.com/better-programming/object-oriented-programming-the-trillion-dollar-disaster-92a4b666c7c7). This has caused some to delve into functional programming instead, which avoids this mutable state problem. 

However, it also depends on the nature of the problem whether it is suitable for OOP or not.


## Exercises

1. Create a **Figure** parent class
2. Create **Circle**, **Rectangle**, **Square** child classes
3. `calculateSurfaceArea()`: Number
4. `calculatePerimeter()`: Number
