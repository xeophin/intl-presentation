You have 36 calls.

???
You may know this message. You probably created it yourself. It's easy, really.

---

```javascript
"You have " + numberOfCalls + " calls.";
```

???
So easy!

---

You have 36 calls.<br/>

--
You have 3 calls.<br/>

--
You have 1 calls.<br/>

--
You have 0 calls.  
???
Okay, so one of them doesn't work. But hey, an easy fix! Just add an condition that changes the text whenever the number is exactly 1.

---

You have 36 calls.  
You have 3 calls.  
You have 1 call.  
You have 0 calls.
???
And then you're told that this has to work in other languages, too. Like, Polish, so Karol can work with it, too.

---

Masz 36 połączeń.  
Masz 3 połączeń.  
Masz 1 połączenie.  
Masz 0 połączeń.

???
- … which is bullshit, since there are different plural forms, depending on the preceding number.
- It's easy to think that the "one form for one object, another form for everything else is common in all languages".
- Polish is a bit different:
  - One form for 1 object
  - Another form for "a few" objects, from 2–4
  - From 5–21 objects, you have a form for _many_ objects
  - 22–24 objects are just few objects, again
- So you add a few new conditions that take care of this …

---

Masz 36 połączeń.  
Masz 3 połączenia.  
Masz 1 połączenie.  
Masz 0 połączeń.

???

And then for some reason your API screws up, and delivers a really precise number of calls.

---

You have 36.5714285714 messages.

???

- Now have to take care of that as well.
- It's easy, just turn it into a string, and then split it at the period, and only use the first part.
- Unless, of course, one browser uses a comma instead, at which point your code breaks.
- You went from one line of code to a massive, steaming pile of … conditions

---

![Shit](https://media1.tenor.com/images/21771529a548c384aebe67f4db209467/tenor.gif?itemid=8203791)

???
- You might have a condition.

---

## Hi, I'm Kaspar

#### I call myself Interactive Storytelling Developer

???

- Fancy way of saying front end developer – with a bit more responsibilities
- Our team of four developers work for paid media
- Whenever a journalist wants to do some fancy storytelling in a story, we're going to program it. Can be data visualisations, interactive pieces, large multimedia stories
- We turn data into visuals, and numbers into language.
- We also assist with the storytelling, we decide on the UX, we do the graphic design

---

## Turning data into language

???

- We get pure data, and our task as front end developers is to turn this data into language
- In a lot of cases, we try to create sensible sentences out of the data. Going from "what is the data" to "what does it mean – to you personally". So we create a lot of "You have such-and-such many calls"-type messages.
- All our code was very German specific
- with the big reshuffling of the department, we were also tasked to create visuals and standalones for the western part of Switzerland – so our stuff had to work in French as well
- This is where we realised that conventions on how data is represented will differ from language to language


---

| Language | Value: 8730023985.98979889336629 |
| -------- | -------------------------------: |
| German   |                 8’730’023’985.99 |
| French   |                 8 730 023 985,99 |
| English  |                 8,730,023,985.99 |

???
- Different decimal and grouping separators
- Depending on the number, this can be ambiguous

---

| Language     | Timestamp: 896781600000 |
| ------------ | ----------------------- |
| English (US) | 6/2/1998                |
| English (GB) | 02/06/1998              |
| German (CH)  | 2.6.1998                |
| French (CH)  | 02.06.1998              |
| French (FR)  | 02/06/1998              |

???

- It even depends on the region
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
const formatForGerman = 
  new Intl.NumberFormat("de-CH").format;

formatForGerman(23985.9897);
//> 23’985.99
```

---

## Currency

```js
const formatCurrency = new Intl.NumberFormat("de-CH", {
  style: "currency",
  currency: "USD"
}).format;

formatCurrency(23985.9897);
//> $ 23’985.99
```

???
- Uses correct currency symbol

---

## Percents

```js
const formatPercents = new Intl.NumberFormat("de-CH", {
  style: "percent",
  maximumFractionDigits: 1
}).format;

formatPercents(0.643362);
//> 64.3%
```

???
- No more rounding!

---

# Intl.DateTimeFormat

```js
const formatDate = new Intl.DateTimeFormat("fr-CH", {
  weekday: "long",
  day: "numeric",
  month: "long",
  year: "numeric"
}).format;

formatDate(new Date(896781600000));
//> mardi, 2 juin 1998
```

---

## Variations …

```js
const formatDate = new Intl.DateTimeFormat("fr-CH", {
  weekday: "long",
  day: "numeric",
  month: "numeric",
  year: "numeric"
}).format;

formatDate(new Date(896781600000));
//> mardi, 02.06.1998
```

???

- We only changed one property, but the format adapted other parts as well
- This is all very nice, but it only solves part of the problems we had above.
- We still have the plural rules
---

## FormatMessage

```javascript
import { FormatMessage } from "intl-messageformat";
```

???
- Not directly available in the browser, but available on NPM as a package
- It uses its own message syntax, and it can be used to set up variations of messages as seen above

---

## Message Syntax

```javascript
message['en-GB'] = `You have {numberOfCalls, plural,
  zero {no calls}
  one {one call}
  other {# calls}
}.`;
```
---
## Message Syntax
```js
message['pl-PL'] = `Masz {numberOfCalls, plural,
  zero {0 połączeń}
  one {1 połączenie}
  few {# połączenia}
  many {# połączeń}
}.`;
```

---
## Usage

```js
const msgFormat = new FormatMessage(
  message["pl-PL"], 
  "pl-PL"
);

msgFormat.format({
  numberOfCalls: 114
});
//> "Masz 114 połączeń"
```

---

## Why is it nice?

???

- The message format is entirely text based
- It allows you to create CSV, JSON or YAML files with your translatable strings and let translators take care of the peculiarities of their language
- A lot less code to maintain

---

# Does it work everywhere?

| Feature        |  Status  | Browser Support       |
| -------------- | -------- | --------------------- |
| Collator       | Standard | Most browsers         |
| DateTimeFormat | Standard | Most browsers         |
| NumberFormat   | Standard | Most browsers         |
| ListFormat     | Draft    | Chrome 72             |
| PluralRules    | Draft    | Chrome 63, Firefox 58 |
| RelativeTime   | Stage 3  | Chrome 71, Firefox 65 |

???

- NumberFormat and DateTimeFormat are even implemented in IE11
- From version 3 on, message format started to rely on an additional Intl object, called PluralRules, so that needs to be polyfilled
- I like to use polyfill.io for that

---

# Thanks!

Kaspar Manz  
@xeophin

Links and presentation can be found under  
<https://github.com/xeophin/intl-presentation>