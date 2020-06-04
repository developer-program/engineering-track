# OWASP Juice Shop

It's time to exercise what we've learnt so far in the form of some healthy competition. Introducing... JUICE SHOP!

<img src="../security/_media/juice_shop.png" height="400" />

> [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) is probably the most modern and sophisticated insecure web application! It can be used in security trainings, awareness demos, CTFs* and as a guinea pig for security tools! Juice Shop encompasses vulnerabilities from the entire OWASP Top Ten along with many other security flaws found in real-world applications!

**Juice Shop is written in Node.js, Express and Angular.** The application contains a vast number of hacking challenges of varying difficulty where the user is supposed to exploit the underlying vulnerabilities. The hacking progress is tracked on a score board.

Apart from the hacker and awareness training use case, pentesting proxies or security scanners can use Juice Shop as a "guinea pig"-application to check how well their tools cope with JavaScript-heavy application frontends and REST APIs.

Read more about its architecture here: https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/introduction/architecture.html

### Why the name "Juice Shop"?

In German there is a dedicated word for dump, i.e. a store that sells lousy wares and does not exactly have customer satisfaction as a priority: *Saftladen*. Reverse-translating this separately as *Saft* and *Laden* yields *juice* and *shop* in English. That is where the project name comes from. The fact that the initials JS match with those commonly used for JavaScript was purely coincidental and not related to the choice of implementation technology.

## Let's begin!

1. `$ git clone https://github.com/bkimminich/juice-shop.git`
1. `$ cd juice-shop`
1. `$ npm install` - this only has to be done before the first start or after you changed the source code.
1. `$ npm start`
1. http://localhost:3000

### Tips

1. Explore all the pages and functionalities of Juice Shop.
1. Find the hidden scoreboard. This will contain a list of vulnerabilities that you can catch during this exercise.

---
**CTF = Capture The Flag. A CTF contest is a special kind of cybersecurity competition designed to challenge its participants to solve computer security problems and/or capture and defend computer systems.* 
