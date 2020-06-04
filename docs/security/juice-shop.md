# OWASP Juice Shop

It's time to exercise what we've learnt so far in the form of some healthy competition. Introducing... JUICE SHOP!

<img src="../security/_media/juice_shop.png" height="400" />

> [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) is probably the most modern and sophisticated insecure web application! It can be used in security trainings, awareness demos, CTFs* and as a guinea pig for security tools! Juice Shop encompasses vulnerabilities from the entire OWASP Top Ten along with many other security flaws found in real-world applications!

**CTF = Capture The Flag. A CTF contest is a special kind of cybersecurity competition designed to challenge its participants to solve computer security problems and/or capture and defend computer systems.* 

**Juice Shop is written in Node.js, Express and Angular.** The application contains a vast number of hacking challenges of varying difficulty where the user is supposed to exploit the underlying vulnerabilities. The hacking progress is tracked on a score board.

Apart from the hacker and awareness training use case, pentesting proxies or security scanners can use Juice Shop as a "guinea pig"-application to check how well their tools cope with JavaScript-heavy application frontends and REST APIs.

Read more about its architecture here: https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/introduction/architecture.html

### Why the name "Juice Shop"?

In German there is a dedicated word for dump, i.e. a store that sells lousy wares and does not exactly have customer satisfaction as a priority: *Saftladen*. Reverse-translating this separately as *Saft* and *Laden* yields *juice* and *shop* in English. That is where the project name comes from. The fact that the initials JS match with those commonly used for JavaScript was purely coincidental and not related to the choice of implementation technology.

## Let's begin!

**Find someone to pair with!** We will assign you to your breakout rooms on Zoom.

Next, **pick 1 person** to do the following steps:

1. Log in to your Heroku account [here](https://www.heroku.com/).
1. Click [here](https://heroku.com/deploy) to deploy an instance of Juice Shop.
1. Share the link with your pair, so that both of you can work on the same Juice Shop.
1. On the first load, there will be a modal welcoming you to Juice Shop. Click on `Help getting started`.

Next, **decide on who will be the first Driver**. The other person will be the Navigator. Remember to switch roles! You can switch roles every 20 minutes, for example. Take 5-minute breaks in between if you need.

### Driver:
1. Share your screen on Zoom so that your pair can see what you see.
1. Vocalise your thought process, e.g. "I think I'll start by exploring ___"
1. If you get stuck, ask your pair for help! If you remain silent, your pair may think that you're deep in thought, and they might not want to interrupt your flow.
1. Actively engage your pair, ask them questions to validate your strategy if you're not sure, e.g. "I suspect ____, what do you think?"

###  Navigator:
1. You can explore the Juice Shop on your machine (especially on your first pairing round, to familiarise yourself with the application), but...
1. Remember that your main task as a navigator is to observe what your pair is doing.
1. If your pair is stuck, and you know the solution, voice it out - you can also use the Annotation tool on Zoom.
1. In addition to observing what your pair is currently doing, you can help think about what other steps may come next. Make suggestions, e.g. "Maybe we can try clicking on ____?"

### Tips

1. Explore all the pages and functionalities of Juice Shop.
1. Find the hidden scoreboard. This will contain a list of vulnerabilities that you can catch for this exercise.
