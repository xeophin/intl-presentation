class: centered large-font

You have 34 messages.

???
You may know this message. You probably created it yourself. It's easy, really.

---
class: centered large-font

```javascript
"You have " + numberOfMessages + " messages."
```

???
So easy!

---
class: centered 

You have 34 messages.  

--
You have 3 messages.  

--
You have 1 messages.  

--
You have 0 messages.  
???
Okay, so one of them doesn't work. But hey, an easy fix! Just add an condition that changes the text whenever the number is exactly 1.
---
class: centered
You have 34 messages.  
You have 3 messages.  
You have 1 message.  
You have 0 messages.
???
And then you're told that this has to work in other languages, too.
---
class: centered
Sie haben 34 Nachrichten.  
Sie haben 3 Nachrichten.  
Sie haben 1 Nachricht.  
Sie haben 0 Nachrichten.
???
And then for some reason your API screws up, and then
---
class: centered
You have 46.5714285714 messages.
???
- Now have to take care of that as well.
- It's easy, just turn it into a string, and then split it at the period, and only use the first part.
- Unless, of course, one browser uses a comma instead, at which point your code breaks.
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
## We do stuff like this:
???
- [In eisigen Tiefen](https://interaktiv.tagesanzeiger.ch/2017/eisige-tiefen/?openincontroller)
- [Hier finden Sie Ihr Steuerparadies](https://interaktiv.tagesanzeiger.ch/2018/steuerbelastung/?nosome)
- [So hat Ihre Gemeinde abgestimmt](https://interaktiv.tagesanzeiger.ch/2019/tobi-2019-05/)

With the different newsrooms being combined into one, we're also doing this:

- [Voici ce que votre commune a voté](https://interactif.24heures.ch/2019/tobi-2019-05/)

At first, we just created forks of your projects, and translated them. But then we started to become ambitious. And we started to run into problems.

---
name: numbers
## Data, meet Language




???
Front-end development is – among other things – about turning data into language.

--
### Data: 8730023985.98979889334629

???
This is an ugly number. But it's fine. We can use `toString()`, right?

--
```js
uglyNumber.toString()
````
> "8730023985.9898"

???
- This is, frankly, not that much better.
- Now, our code was littered with string manipulation afterwards
- Take periods and replace them with commas.
- Split the string at the period, take the first part, create groups of three characters each, add apostrophes …

--
> "8’730’023’985.99"

???
This is closer to what we want. In the German speaking part of Switzerland, anyway. 

---
## Data has to be translated into language

Data: 8730023985.98979889334629

German: 8’730’023’985.99
French: 8 730 023 985,99
English: 8,730,023,985.99

???
Of course, you can just rewrite your string manipulation code for every language you want to support, but … really?

___
name: dates
## It's not just numbers …

--

Data: 896781600000

???
This is a date. Any guesses?

--
Language 1: "6/2/1998"

--

Language 2: "02/06/1998"

--

Language 3: "2.6.1998"

--

Language 4: "02.06.1998"

--

Language 5: "02/06/1998"

???
1. US English
2. English in Great Britain
3. German in Switzerland
4. French in Switzerland
5. French in France

---
classes: center middle

> ## You have 1 messages

Looks familiar?

---

## Data, meet language

```javascript
const msg = `You have ${numberOfMessages} messages.`
```

> You have 3 messages.
> You have 0 messages.
> You have 1 messages.

---

## Let's improve this

```javascript
const msg = `You have ${numberOfMessages} message${numberOfMessages > 1 ? 's' : ''}.`
```

> You have 3 messages.
> You have 1 message.
> You have 0 message.

---
## Okay, not quite

```javascript
let msg = `You have ${numberOfMessages} messages.`
if (numberOfMessages === 1) {
  msg = `You have ${numberOfMessages} message.`
}
```

> You have 3 messages.
> You have 1 message.
> You have 0 messages.

---
## More languages!

```javascript
let msg = ''

if (lang === 'en') {
  msg = `You have ${numberOfMessages} messages.`
  if (numberOfMessages === 1) {
    msg = `You have ${numberOfMessages} message.`
  }
} else if (lang === 'de') {
  msg = `Du hast ${numberOfMessages} Nachrichten.`
  if (numberOfMessages === 1) {
    msg = `Du hast ${numberOfMessages} Nachricht.`
  }
}
```

> You have 3 messages.
> You have 1 message.
> You have 0 messages.
> Du hast 3 Nachrichten.
> Du hast 1 Nachricht.
> Du hast 0 Nachrichten.

---
## Even more!

- [ ] Find language with more plural forms, like arabic

---
classes: center middle

![Shit](https://media1.tenor.com/images/21771529a548c384aebe67f4db209467/tenor.gif?itemid=8203791)


---
## `Intl`

???
- Internationalisation is a common problem
- Despite this, it has been only recently added to the browser APIs
- 

---
## DateTimeFormat

---
## NumberFormat

---
## FormatMessage

Not available directly as a browser API.

```javascript
import {FormatMessage} from 'intl-format-message'
```

---
## Message Syntax

```javascript
const message = `You have {plural {}}`
```

---
## Status of `Intl` features

| Feature        | Status   | Browser Support       |
| -------------- | -------- | --------------------- |
| Collator       | Standard | Most browsers         |
| DateTimeFormat | Standard | Most browsers         |
| NumberFormat   | Standard | Most browsers         |
| ListFormat     | Draft    | Chrome 72             |
| PluralRules    | Draft    | Chrome 63, Firefox 58 |
| RelativeTime   | Stage 3  | CHrome 71, Firefox 65 |

---
## Polyfill
Polyfill.io

---
