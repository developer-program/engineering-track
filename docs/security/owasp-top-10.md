# OWASP Top 10

The OWASP Top 10 is one of many projects by OWASP. It is a standard awareness document for developers and web application security.

It represents a broad consensus about **the most critical security risks to web applications**, i.e. top 10 most frequently used exploits in a website or application.

The OWASP Top 10 document is globally recognised by developers as the **most effective first step towards changing the software development culture within your organisation** into one that produces more secure code. Companies should adopt this document and start the process of ensuring that their web applications minimise these risks.

Read more here: https://owasp.org/www-project-top-ten/

# 1. Injection

![little bobby tables](_media/little_bobby_tables.png)

Injection flaws, such as SQL, NoSQL, OS, and LDAP (Lightweight Directory Access Protocol) injection, occur when **untrusted data is sent to an interpreter as part of a command or query**. The attacker’s hostile data can trick the interpreter into executing unintended commands or accessing data without proper authorisation.

## Examples

To illustrate this, let's play a game.

1. Go to https://securecodewarrior.com/

2. Click on `PLAY NOW` in the navbar:

![secure code warrior setup](_media/secure_code_warrior_1.png)

3. Enter your name and select `JavaScript Node.js (Express)`:

![secure code warrior setup](_media/secure_code_warrior_2.png)

4. Click on `Enter game mode`. You should end up here: https://portal.securecodewarrior.com/#/website-trial/web/injection/sql/nodejs/express/realm/teaser/level/showcase/quest/injection_sql

5. Follow the game instructions! You'll need to locate the vulnerability, and identify the correct solution.

## Prevention
- Provide a **parameterised interface**
- Do not use string-concatenated queries
![sql injection example](_media/sql_injection.png)
- Validate user-supplied input. Read more [here](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html).
- Validate input server-side with a **whitelist** instead of a blacklist. This way, you can define exactly what IS authorised - everything else is not authorised.
- Use source code review tools
- Add automated testing for all parameters, headers, URL, cookies, JSON, SOAP, and XML data inputs

**More resources:**

https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
https://cheatsheetseries.owasp.org/cheatsheets/LDAP_Injection_Prevention_Cheat_Sheet.html


# 2. Broken authentication

Application functions related to authentication and session management are often implemented incorrectly, allowing attackers to compromise passwords, keys, or session tokens, or to exploit other implementation flaws to assume other users’ identities temporarily or permanently.

## Examples

1. [Brute-force attack](https://en.wikipedia.org/wiki/Brute-force_attack): the attacker submits many passwords with the hope of eventually guessing correctly
1. [Credential stuffing](https://owasp.org/www-community/attacks/Credential_stuffing): the attacker uses a list of **breached username/password pairs** and gains access to user accounts
1. [Session](https://auth0.com/docs/sessions/concepts/session-lifetime) timeout is not appropriately set up

## Prevention

- Implement [rate limits](https://auth0.com/docs/connections/database/rate-limits) - if a user enters their password incorrectly more than X times consecutively from a single IP address, they will be blocked from logging into their account from that IP address
- Implement Two-factor Authentication (2FA), e.g. Google Authenticator with a [TOTP (Time-based One-Time Password)](https://tools.ietf.org/html/rfc6238) generator like [Speakeasy](https://github.com/speakeasyjs/speakeasy)
- Implement Multi-factor Authentication (MFA) e.g. with [Okta](https://www.okta.com/products/adaptive-multi-factor-authentication/)
- Implement (CAPTCHA/reCAPTCHA](https://anydifferencebetween.com/difference-between-captcha-and-recaptcha/)
- Implement weak-password checks (e.g. minimum length, special characters/numbers required, check against well-known [weak/leaked passwords](https://github.com/danielmiessler/SecLists/tree/master/Passwords))
- **Never store passwords in plaintext!** Always [salt and hash passwords](https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords/), e.g. with [bcrypt](https://www.npmjs.com/package/bcryptjs)
- Require reauthentication after every X hours, depending on the sensitivity of the data
- For high-risk data (e.g. financial information), set sessions to **time out after 2-5 minutes of idle time**. It can be set to 15-30 minutes for low-risk applications. e.g. with [express-session](https://www.npmjs.com/package/express-session#cookiemaxage-1) and setting `maxAge`/`expires` value:

```
var hour = 3600000
req.session.cookie.expires = new Date(Date.now() + hour)
req.session.cookie.maxAge = hour
```

**More resources:**

- https://cheatsheetseries.owasp.org/cheatsheets/Credential_Stuffing_Prevention_Cheat_Sheet.html
- https://github.com/danielmiessler/SecLists
- https://auth0.com/blog/when-ux-equals-keeping-or-losing-the-customer/
