class: centered large-font

You have 36 calls.

???
You may know this message. You probably created it yourself. It's easy, really.

---
class: centered large-font

```javascript
"You have " + numberOfCalls + " calls."
```

???
So easy!

---
class: centered 

You have 36 calls.  

--
You have 3 calls.  

--
You have 1 calls.  

--
You have 0 calls.  
???
Okay, so one of them doesn't work. But hey, an easy fix! Just add an condition that changes the text whenever the number is exactly 1.
---
class: centered
You have 36 calls.  
You have 3 calls.  
You have 1 call.  
You have 0 calls.
???
And then you're told that this has to work in other languages, too. Like, Polish, so Karol can work with it, too.
---
class: centered
Masz 36 połączeń.  
Masz 3 połączeń.  
Masz 1 połączenie.  
Masz 0 połączeń.
???
- … which is bullshit, since there are different plural forms, depending on the preceding number.
- So you add a few new conditions that take care of this …
---
class: centered
Masz 36 połączeń.  
Masz 3 połączenia.  
Masz 1 połączenie.  
Masz 0 połączeń.
???
And then for some reason your API screws up, and then
---
class: centered
You have 36.5714285714 messages.
???
- Now have to take care of that as well.
- It's easy, just turn it into a string, and then split it at the period, and only use the first part.
- Unless, of course, one browser uses a comma instead, at which point your code breaks.
---
classes: centered

![Shit](https://media1.tenor.com/images/21771529a548c384aebe67f4db209467/tenor.gif?itemid=8203791)

---
## Hi, I'm Kaspar
####  I call myself Interactive Storytelling Developer

???
- I work for «Redaktion Tamedia»
- Whenever a journalist wants to do some fancy storytelling in a story, we're going to program it.
- We turn data into visuals, and numbers into language.
- We had a lot of such code in our projects.
- With the different newsrooms being combined into one, we were suddenly tasked to produce our projects for the french speaking part of Switzerland as well – which made it obvious how *utterly* brittle our code really was.

---
class: centered
## Turning data into language
???
- We get pure data, and our task as front end developers is to turn this data into language
- How to display data in a manner that's accessible however is based on convention and custom – which might be different from language to language, and from country to country.

---
class: centered

|Language|Value: 8730023985.98979889336629|
|--------|-------------------------------:|
|German  |8’730’023’985.99                |
|French  |8 730 023 985,99                |
|English |8,730,023,985.99                |

---
class: centered

|Language    |Timestamp: 896781600000|
|------------|-----------------------|
|English (US)|6/2/1998               |              
|English (GB)|02/06/1998             |
|German (CH) |2.6.1998               |
|French (CH) |02.06.1998             |
|French (FR) |02/06/1998             |

???
- Obviously, we're not the first programmers to encounter this problem.
- The process is called **Internationalisation**, and most programming languages contain libraries that facilitate it
- There is a large library that contains all these informations, that's being used by all the major software companies – the [Unicode Common Locale Data Repository](http://cldr.unicode.org/index)
- That data is part of your operating system. And your browser. So take advantage of it!

---
# Intl
???
- JavaScript contains the **Intl** object, which covers numbers and date formatting
- Also, depending on the browser, a few more things

---
# Intl.NumberFormat

```js
const formatForGerman = new Intl.NumberFormat('de-CH')
formatForGerman(8730023985.98979889336629)
//> 8’730’023’985.99
```
---
## Currency
```js
const formatCurrency = new Intl.NumberFormat('de-CH', {
  style: 'currency',
  currency: 'CHF'
})
formatCurrency(8730023985.98979889336629)
//> CHF 8’730’023’985.99
```
---
## Percents
```js
const formatPercents = new Intl.NumberFormat('de-CH', {
  style: 'percent',
  maximumFractionDigits: 1
})
formatPercents(0.643362)
//> 64.3%
```

---
# Intl.DateTimeFormat
```js
const formatDate = new Intl.DateTimeFormat('fr-CH', {
  weekday: 'long', 
  day: 'numeric', 
  month: 'long', 
  year: 'numeric'
})
formatDate(new Date(896781600000))
//> mardi, 2 juin 1998
````
---
## Variations …
```js
const formatDate = new Intl.DateTimeFormat('fr-CH', {
  weekday: 'long', 
  day: 'numeric', 
  month: 'numeric', 
  year: 'numeric'
})
formatDate(new Date(896781600000))
//> mardi, 02.06.1998
````
???
- This is all very nice, but it only solves part of the problems we had above.

---
## FormatMessage

```javascript
import {FormatMessage} from 'intl-messageformat'
```
???
- Not directly available in the browser, but available on NPM as a package
- It uses its own message syntax, and it can be used to set up variations of messages as seen above

---
## Message Syntax

```javascript
const englishMessage = `You have {numberOfCalls, plural,
  zero {no calls}
  one {one call}
  other {# calls}
}.`

const polishMessage = `Masz {numberOfCalls, plural,
  zero {0 połączeń}
  one {1 połączenie}
  few {# połączenia}
  many {# połączeń}
  other {# połączeń}
}.`
```

---
```js
const englishMsgFormat = new FormatMessage(englishMessage, 'en-GB')
englishMsgFormat.format({numberOfCalls: numberOfCalls})
```

---
## Why is it nice?
???
- The message format is entirely text based
- It allows you to create CSV, JSON or YAML files with your translatable strings and let translators take care of the peculiarities of their language


---
exclude: true
## We do stuff like this:
???
- [In eisigen Tiefen](https://interaktiv.tagesanzeiger.ch/2017/eisige-tiefen/?openincontroller)
- [Hier finden Sie Ihr Steuerparadies](https://interaktiv.tagesanzeiger.ch/2018/steuerbelastung/?nosome)
- [So hat Ihre Gemeinde abgestimmt](https://interaktiv.tagesanzeiger.ch/2019/tobi-2019-05/)

With the different newsrooms being combined into one, we're also doing this:

- [Voici ce que votre commune a voté](https://interactif.24heures.ch/2019/tobi-2019-05/)

At first, we just created forks of your projects, and translated them. But then we started to become ambitious. And we started to run into problems.





---
## Status of `Intl` features

| Feature        | Status   | Browser Support       |
| -------------- | -------- | --------------------- |
| Collator       | Standard | Most browsers         |
| DateTimeFormat | Standard | Most browsers         |
| NumberFormat   | Standard | Most browsers         |
| ListFormat     | Draft    | Chrome 72             |
| PluralRules    | Draft    | Chrome 63, Firefox 58 |
| RelativeTime   | Stage 3  | Chrome 71, Firefox 65 |

---
## Polyfill
Polyfill.io

---
