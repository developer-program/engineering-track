# Refactoring

> Refactoring (noun): a change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its observable behavior.

-- Martin Fowler, Refactoring: Improving the Design of Existing Code

Martin Fowler, the Chief Scientist at ThoughtWorks, has written a second edition of his classic book [Refactoring: Improving the Design of Existing Code](https://www.amazon.com/Refactoring-Improving-Existing-Addison-Wesley-Signature/dp/0134757599) which uses JavaScript in their examples.

## When to refactor?

As soon as possible.

In test driven development, we first write one failing test, we then write the minimum amount of code to pass the test, and lastly, we refactor the code without adding any new functionality. This is an essential part of test driven development. It is not enough that we have code that works, but code that is also well written and easier to extend for future development.

Leaving code to rot and not refactor as soon as possible will lead to worse code as a whole eventually.
We can apply the same ideas in the [broken window theory](https://en.wikipedia.org/wiki/Broken_windows_theory) in software development. It is a criminal theory that describes how irrelevant and small crimes such as minor vandalism, having broken windows on houses in the neighbourhood will start a negative feedback loop. People will gradually be emboldened to do bigger crimes and this leads to total disorder.
This can also be applied to code.

> Don't leave "broken windows" (bad designs, wrong decisions, or poor code) unrepaired. Fix each one as soon as it is discovered.

- Pragmatic Programmer

When we have a well organized closet, its easy for us to find the clothes we want. When we have to do the laundry, we're more inclined to keep it organized. However, if we already have a disorganized closet, it takes a lot more effort to keep the closet tidy when we have to do the laundry. You will also not be motivated to keep it tidy.

## How to make refactoring more "safe"

You might dread refactoring because you might have done refactoring without working tests or sufficient test coverage. Thus refactoring might lead to some features not working, or even lead to great discomfort as you feel insecure about whether your code is working as intended or not. 

You can follow the following rules to make refactoring more safe and rewarding:

1. Only do refactoring when existing tests are passing and have sufficient coverage
This ensures that you can be more confident of your refactoring.

2. Refactoring should be done as a series of small changes
For example, if you want to refactor three different files, you might only change one functions or a few functions at a time. Make sure the tests are still passing after each small change. Don't change all three files at the same time.

If you are moving from A to Z, move from A to B, check if tests are passing, then move from B to C etc.

3. Code should become cleaner after refactoring
if you find yourself writing worse code after refactoring, then either your refactoring is not successful or the original code was already a disaster. Maybe the code really needs a full rewriting?

4. Tempting as it is, don't try to create new functionality during refactoring. 
It is easy to make a bigger mess of the code when trying to refactor and develop at the same time.

4. If the tests keep failing with every small refactoring, your tests could be too low-level. Are you testing private methods? Are you testing every single function interaction? Maybe your tests could be made to be more high-level.


## How to learn refactoring?

### Code smells
Code smells are indications of deeper problems in the code. The term was coined by Kent Beck in Martin Fowler's Refactoring book. They are just indications and might not mean that there is an actual problem. 

They are good to read about but you do not need to memorise every single code smell. They will eventually become natural as you do more refactoring.

One common code smell is **Long Parameter List**. [Having more than three or four parameters for a method](https://refactoring.guru/smells/long-parameter-list) might indicate a problem with the cohesion or duplication of the code. 

However, we cannot be sure that there is a problem, because the long parameter list could be because we want to reduce dependencies between classes or we do not want the parameters to be moved to a new class because it will be more of just a data class. Therefore, it really depends on the actual code.

You can read more about refactoring and code smells on websites like https://refactoring.guru/refactoring.

We recommend getting hands-on practice when learning refactoring. Here are two popular katas for you to practice:

- [Tennis](https://github.com/emilybache/Tennis-Refactoring-Kata)
- [Gilded Rose](https://github.com/emilybache/GildedRose-Refactoring-Kata)

## Exercises

Lets do Glided Rose together.
