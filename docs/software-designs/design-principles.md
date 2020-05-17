# Design Principles

If you don't eat, you will get hungry. If you don't sleep for two days, you won't be able to focus on tasks.

How did we know this? How can we prove this is true? There might be a scientific explanation behind it but even without one, I know it is true based on anecdotal evidence, based on my own experience.

Design principles are common ways of organising our class, functions and methods. Many people believe these principles will result in code that is easier to read, understand, maintain and is still adaptable to changes. They come from long debates and experience of many veterans before us.

> "Any fool can write code that a computer can understand. Good programmers write code that human can understand" - Martin Fowler.

## Content

1. Don't Repeat Yourself(DRY)
2. Law of Demeter
3. S.O.L.I.D

## Don't Repeat Yourself(DRY)

Adapted from "The Pragmatic Programmer" by Andy Hunt and David Thomas, published in 1999.

The **DRY** principle is the solution to "The Evil of Duplication". When code is duplicated, it becomes hard to maintain. If the same piece of logic appears in the code multiple times, when a requirement changes which then changes the logic, the software developer will need to change these duplicated pieces of logic at all the different places.

Do you remember what you ate for breakfast one month ago? If you can't, then you won't be able to remember where all these duplicate pieces of logic are in the code. Even if you can remember, it will be tedious and error-prone to change all the duplicated code.

Duplications of code make maintenance of code hard for multiple reasons. Take these consequences into consideration before you "copy and paste" code.

1. One developer needs to know what multiple developers did in the history of the project.
2. Even doing the same code multiple times can prone to error. Higher chance of introducing bugs. The variables' name might be slightly different too.
3. More difficult to debug. If the program has a bug in method A but method B has the same code then you might try to find the bug at method B instead and end up wasting time.
4. Adding a new feature to duplicate code will result in more duplication. The extra duplication might introduce bugs.

"Every piece of **knowledge** must have a single, unambiguous authoritative representation within a system".

Andy Hunt and David Thomas explained that there are four causes of duplication.

- Imposed duplication: Architecture decision
- Inadvertent duplication: Developers didn't know about the duplication
- Impatient duplication: Duplicating the code was easier to do during development
- Interdeveloper duplication: Multiple people on a team duplicated the same piece of information

Example of Duplication in React:

Does this code look like it has been copied and pasted over?

```js
const SavedModal = ({ showSavedModal, closeSave }) => {
  return (
    <Modal open={showSavedModal}>
      <Header content="Saved" />
      <Modal.Content>
        <p>Successfully saved!</p>
      </Modal.Content>
      <Modal.Actions>
        <Button
          aria-label="close save message"
          color="green"
          onClick={closeSave}
        >
          <Icon name="checkmark" /> Ok
        </Button>
      </Modal.Actions>
    </Modal>
  );
};

const PublishModal = ({ showPublishedModal, closePublish }) => {
  return (
    <Modal open={showPublishedModal}>
      <Header content="Published" />
      <Modal.Content>
        <p>Successfully published!</p>
      </Modal.Content>
      <Modal.Actions>
        <Button
          aria-label="close publish message"
          color="green"
          onClick={closePublish}
        >
          <Icon name="checkmark" /> Ok
        </Button>
      </Modal.Actions>
    </Modal>
  );
};
```

Is there any way to remove the duplicate code?

```javascript
const confimationModalFactory = (message) => { showSavedModal, closeSave }) => {
  return (
    <Modal open={showSavedModal}>
      <Header content="Saved" />
      <Modal.Content>
        <p>{message}</p>
      </Modal.Content>
      <Modal.Actions>
        <Button
          aria-label="close save message"
          color="green"
          onClick={closeSave}
        >
          <Icon name="checkmark" /> Ok
        </Button>
      </Modal.Actions>
    </Modal>
  );
};

const SavedModal = confimationModalFactory("Successfully saved!");
const PublishModal = confimationModalFactory("Successfully published!");
```

Other forms of duplication:

- Documentation in code. When we write a comment and then start coding, the comments written is another form of duplication of knowledge.
- language / framework issue

An example of a language issue can be the wrapAsync is a logic that calls `next(Error)`. That prevented some logic from duplicating. If we look at the code, every handler still has `wrapAsync`. It would be nice if we can have the try-catch mechanism as a global setting.

```javascript
const wrapAsync = (fn) => async (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch((err) => next(err));
};

router.get(
  "/:articleId",
  wrapAsync(async (req, res, next) => {
    const singleArticle = await Publish.find({
      id: req.params.articleId,
    });
    res.status(200).send(singleArticle);
  })
);
router.patch(
  "/update/:articleId",
  wrapAsync(async (req, res, next) => {
    const publishingArticle = req.body;
    const updatePublishedArticle = await Publish.findOneAndUpdate(
      {
        id: req.params.articleId,
      },
      publishingArticle,
      { new: true }
    );
    res.status(200).send(updatePublishedArticle);
  })
);
```

### Law of Demeter

Adapted from Ian Holland at Northeastern University in 1987.

Also known as the principle of least knowledge. Each object should only directly access properties and methods.

- Each unit should only talk to its friends; don't talk to strangers.
- Only talk to your immediate friends.

When you buy chicken rice, do you give the seller your whole wallet or the actual money?

```javascript
const Wallet {
    amount = 5;

    pay(amount) {
        if (this.amount >= amount) {
            this.amount -= amount;
        } else {
            throw new Error("Wallet does not have enough money to pay this amount");
        }
    }

    topUp(amount) {
        this.amount += amount;
    }
}

class Consumer {
    const wallet = new Wallet();

    pay(amount) {
        this.wallet.pay(amount);
    }
}

const alice = new Consumer();


class Shop {
    transactionBad(consumer, price) {
        consumer.wallet.pay(price);  // breaks Law of Demeter, Shop should not know about the Alice's wallet
    }

    transactionBetter(consumer, price) {
        // the Shop doesn't know what the internal object of the consumer, all Shop know is that the consumer can pay.
         consumer.pay(price);
    }
}
```

## S.O.L.I.D Principle

Adapted from "Design Principles and Design Patterns" by Bob Martin, in 2000.
In 2004, Michael Feathers realised that the principles could arrange to form the acronym SOLID.

Bob Martin started to assemble rules of what makes code easy to maintain in the late 1980s and only finalised them more than a decade after.

1. Single Responsibility Principle (SRP)
2. Open-Closed Principle (OCP)
3. Liskov Substitution Principle (LSP)
4. Interface Segregation Principle (ISP)
5. Dependency Inversion Principle (DIP)

The principles may overlap each other.

### Single Responsibility Principle (SRP)

_A Module should be responsible to one, and only one, actor_

A Module is a part of the program that are closely related, put together to provide functionality. It can be a function, a class, a single file, a single server. This module has only one responsibility in the code.

It also has been described as only having "one reason to change". Because the module has only one responsibility, editing that particular responsibility will be the only reason to change the module. Also think about the nature of the business environment in which the code is undergoing change.

> [Gather together the things that change for the same reasons. Separate those things that change for different reasons. - Bob Martin](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)

The following code does not follow SRP.

```js
const scores = [100, 200, 300];
const calculateTotalScore = (scores) => {
  const totalScore = scores.reduce((sum, score) => {
    return (sum += score);
  });
  console.log(`total score is ${totalScore}`);
};

calculateTotalScore(scores);
```

How can we make it better?

```js
const scores = [100, 200, 300];
const calculateTotalScore = (scores) => {
  return scores.reduce((sum, score) => {
    return (sum += score);
  });
};

const printTotalScore = (total) => {
  console.log(`total score is ${total}`);
};

printTotalScore(calculateTotalScore(scores));
```

### Open/Closed Principle (OCP)

_Software entities should be open for extension but closed for modification_

Bertrand Meyer explained this principle as the open/closed principle, which appeared in his 1988 book Object Oriented Software Construction.

When Bob Martin included it in his SOLID principles, he explained it as:

> You should be able to extend the behavior of a system without having to modify that system. - Bob Martin

Classes should be able to extend without any modification of the internal structure.
If a new requirement that doesn't affect the existing functionality, a code that fulfils OCP, should not result in changes in existing code.

A good real-life example is Nintendo Labo. Nintendo controllers have interfaces that are open to extensions of different controllers and Labo accessories. None of them requires you to edit the original controller itself. It does that by exposing the right interface.

Another example is when we play games we can download mods to add new functionality to the current game without having to edit the code of the current game. Or you could think of them like plug-ins.

We made use of OCP in the Vending Machine lab.

When you add a new container or a new menu item did you have to modify the internal structure?

How did you add a new menu item?

### Liscov Substituion principle

Created by Barbara Liskov in 1988:
_Let Φ(x) be a property provable about objects x of type T. Then Φ(y) should be true for objects y of type S where S is a subtype of T._

In Bob Martin's words:
_Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program._

Child cannot be stricter than its parent.

If we can rely on LSP, it allows us to use polymorphism reliably in our code.

Say we have a vehicle class that can move forward, reverse, turn left, turn right and top-up fuel. If we declare a bicycle as a vehicle (and it is considered a vehicle in real-life), we cannot substitute the bicycle as a vehicle that can top up fuel.

```js
class Vehicle {
  moveForward() {}
  turnLeft() {}
  turnRight() {}
  topUpFuel() {}
  reverse() {}
}

// top up fuel for all vehicles
vehicles.forEach((vehicle) => vehicle.topUpFuel(5));

class Bicycle extends Vehicle {
  topUpFuel() {
    // what to do here??
    throw Error("no fuel for bicycle!");
  }
}
```

### Interface Segregation Principle (ISP)

_Clients should not be forced to depend upon interfaces that they do not use._

There is no true interfaces in JavaScript, only in TypeScript. But we can interpret this ISP rule as: if another module depends on this module, is there something that the other module does not need?

In other words, am I forcing the other module to have a certain logic?

The above example for bicycle also breaks ISP, if we consider that an interface of vehicles would be to implement the method of `topUpFuel()`

```javascript
class Bicycle extends Vehicle {
  topUpFuel() {
    throw new Error("not a fuel powered vehicle");
  }
}
```

Another interpretation of ISP is to not force every instance of the class to run some logic by putting an logic that is supposed to be optional in the constructor.

For example,

```js
class User {
  constructor(user) {
    this.user = user;
    this.initiateUser();
  }

  initiateUser() {
    this.name = this.user.name;
    this.validateUser(); // this is optional!! not every user will need validation
  }
}
```

```js
class User {
  constructor(user) {
    this.properties = user.properties;
    this.initiateUser();
    this.setupOptions = user.options;
  }

  initiateUser(){
    this.name = this.user.name
    this.setupOptions()
  }
}

function validateUser() {
 //some other code...
}
const properties = {
  //some other code...
}
const user = new User({ properties, options: validateUser});
const userWithNoValidation = new User({ properties, options: ()={}};
```

### Composition over inheritance

Sometimes we can use 'composition over inheritance' advice to conform to ISP.

The following code violates ISP as `Car` is forced to own the `fuelLevel` property and the `topUpFuel` method.

```js
class FuelPoweredVehicle {
  constructor(fuelLevel) {
    this.fuelLevel = fuelLevel;
  }

  topUpFuel(amountOfFuel) {
    this.fuelLevel = amountOfFuel;
  }
}

// Using inheritance
class Car extends FueledPoweredVehicle {}
```

Also, we cannot make `Bicycle` class extend `FueledPoweredVehicle`!

Let's use composition instead and see how it looks like:

```js
class FuelTank {
  constructor(fuelLevel) {
    this.fuelLevel = fuelLevel;
  }

  topUpFuel(amountOfFuel) {
    this.fuelLevel = amountOfFuel;
  }
}

class Car {
  constructor(fuelTank) {
    this.fuelTank = fuelTank;
  }

  topUpFuel(amountOfFuel) {
    this.fuelTank.topUpFuel(amountOfFuel);
  }
}
```

Now `topUpFuel` rightly belongs as a method of `FuelTank`. `FuelTank` owns the `fuelLevel` property. The best thing is, if we decide to have the `Bicycle` class, it will not need to have the `topUpFuel` method.

### Dependency Inversion Principle (DIP)

_Any higher-level modules should not depend on lower-level modules, and both should depend on abstract modules._

_Abstraction of modules should not depend on its implementation or details, but the implementation should depend on abstraction._

When using Mongoose, we import a Model. This model implements an abstraction. All models will contain similar generic methods such as `findOneAndUpdate`. Knowing that the object is a Model, the router handler can safely call `findOneAndUpdate`.

The router handler does not depend on the Schema. If the Schema of the Model changes, the handler will not need modification, as it deals with the Model abstraction, not the details of the Schema.

```javascript
const articleSchema = new mongoose.Schema(
  {
    id: { type: String, required: true, unique: true },
    title: { type: String, required: true, unique: true, trim: true },
    topicAndSubtopicArray: [topicSubtopicSchema],
    isPublished: { type: Boolean, default: false },
  },
  { timestamps: true }
);
articleSchema.index(
  { "topicAndSubtopicArray.title": 1 },
  {
    unique: true,
    partialFilterExpression: {
      "topicAndSubtopicArray.title": { $exists: true },
    },
  }
);
const Publish = mongoose.model("Publish", articleSchema);

router.patch(
  "/update/:articleId",
  wrapAsync(async (req, res, next) => {
    const publishingArticle = req.body;
    const updatePublishedArticle = await Publish.findOneAndUpdate(
      {
        id: req.params.articleId,
      },
      publishingArticle,
      { new: true }
    );
    res.status(200).send(updatePublishedArticle);
  })
);
```

## Exercises

### LSP - Square and Rectangle

Another common example could break LSP is that `Square` cannot extend the class `Rectangle`. Why do you think this is so?

Write `Square` and `Rectangle` classes to explain the possible breaking of LSP.

Violations of LSP, in practice, is only a problem when the code using these classes make the wrong assumptions. However, you cannot blame the user of the classes in this case because they expected the instance of the parent to be replaceable by the instance of the child.

### Vending Machine lab

Review your vending machine lab so far and discuss how you can apply the design principles. Can we refactor our existing code?
