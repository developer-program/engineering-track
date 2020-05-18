# Technical debt

## What exactly is technical debt?

_Technical debt_ is a metaphor first mentioned by Ward Cunningham. He was creating a financial software and used a financial analogy to talk about the refactoring they were doing.

In this analogy, designing software without knowing what the best design should be is like borrowing money. You can do something with the money sooner (or even do greater things!), but then until you pay back that money you'll be paying interest. Interest, in this case, is slowing down the development of new features.

However, this is easily misunderstood. This does not mean you can [code poorly down with the intention of paying back later](http://wiki.c2.com/?WardExplainsDebtMetaphor).

![technical debt quadrant](https://martinfowler.com/bliki/images/techDebtQuadrant.png)
(From martinfowler.com)

**Prudent** debt is that the team knows that they are having a debt. It is well-calculated and will be dealt with eventually.

**Reckless** debt is a team taking on debt without knowing the consequences of their design decisions.

**Delibrate** debt is when technical debt is created on purpose,

**inadvertent** is when it is not.

Is there really prudent-inadvertent debt?

There is. You are learning about the project as you are programming, often you will have to program for a while before you realise what the best design should have been. If you focus on developing a program just by adding features and not reorganising it to reflect your new understanding on those features as a whole, you will accumulate such technical debt. It will get harder and harder to develop new features.

Read [TechnicalDebt on martinfowler.com](https://martinfowler.com/bliki/TechnicalDebtQuadrant.html)

## Mess is not debt

> A mess is not a technical debt. A mess is just a mess. Technical debt decisions are made based on real project constraints, not because of procrastination. Clean code can still have technical debt. [Uncle Bob](https://sites.google.com/site/unclebobconsultingllc/a-mess-is-not-a-technical-debt)

You should still write clean code to make future technical debt easy to pay off.

> I'm never in favor of writing code poorly, but I am in favor of writing code to reflect your current understanding of a problem even if that understanding is partial.
> In other words, the whole debt metaphor, let's say, the ability to pay back debt, and make the debt metaphor work for your advantage depends upon your **writing code that is clean enough** to be able to refactor as you **come to understand** your problem.
> [Ward Cunningham](http://wiki.c2.com/?WardExplainsDebtMetaphor)

[Watch Ward talk about technical debt](https://www.youtube.com/watch?v=pqeJFYwnkjE)

## Managing Technical Debt

1. Keep track of technical debt, one way is to write the "debts" on post-its and paste them on a Tech Debt board / mahjong paper.

2. Prioritise technical debt by effort and pain.

Effort is the relative effort the team needs to put in to remove the debt.

![Tech Debt](https://agileboardhacks.files.wordpress.com/2012/04/techdebtretroeffortvspainhighreturnnotworth.png)

3. If there is too much technical debt to be handled, the team can have a short discussion to decide on the priority.

“The relative effort that the team would have to spend in order to pay this debt.“

“Pain is the direct impact on productivity that this debt causes. (interest)”
https://agileboardhacks.com/tag/tech-debt/

## Exercises

Read the following technical debt post-its.

- Remove magic numbers in utils
- Clean up vending machine tests
- Reduce duplication of code in messy file
- Remove npm packages dependencies that are not actually used in project

Try to place these on a priortisation graph to decide what technical debt to priortise. You should discuss this with the rest.

![product value matrix](https://reforge-brevity-uploads-prod.s3.amazonaws.com/brief/uploads/post/image_path/58/emma1530748538874.png)
