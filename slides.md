# You have<br>36 calls.

???
You may know this message. You probably created it yourself. It's easy, really.

---

```javascript
"You have " + numberOfCalls + " calls.";
```

???
So easy!

Let's say you create a super simple video call app, that also works for your grandma, because you should call grandma more often.

---

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

You have 36 calls.  
You have 3 calls.  
**You have 1 call.**  
You have 0 calls.

???
Now, your coworker from Poland walks past you and mentions that this would be great for her grandmother as well, but it needs to be translated to Polish, since her grandmother only speaks Polish. So you take to Google Translate …

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
**Masz 3 połączenia.**  
Masz 1 połączenie.  
Masz 0 połączeń.

???
This is more how it should look like.

So, at this point your code consists of at least 5 different conditions.

Now, another coworker sees what you are doing, and they have this lovely aunt they should call more often, and the aunt lives in Lebanon, and speaks Arabic.

You know where this is going.

---
exclude: true

You have 36.5714285714 messages.

???

- Now have to take care of that as well.
- It's easy, just turn it into a string, and then split it at the period, and only use the first part.
- Unless, of course, one browser uses a comma instead, at which point your code breaks.


---

![Shit](https://media1.tenor.com/images/21771529a548c384aebe67f4db209467/tenor.gif?itemid=8203791)

???
- You went from one line of code to a massive, steaming pile of … conditions
- You might have a condition.

---

## Hi, I'm Kaspar
### I call myself Interactive Storytelling Developer

???
- Fancy way of saying front end developer – with a bit more responsibilities
- Our team of four developers work for Tamedia
- Whenever a journalist wants to do some fancy storytelling in a story, we're going to program it. Can be data visualisations, interactive pieces, large multimedia stories
- We turn data into visuals, and numbers into language.
- We also assist with the storytelling, we decide on the UX, we do the graphic design

---
???
Now that we have been aquainted, let's do a little exercise.

I will show you a number, on the count of three. Please shout in your mothertongue its rounded value.

--
class: center
# 10,001

???
Raise your hand if you said 10

Raise your hand if you said 10'000

Both is correct – depending on where you are from.
---

#### Numbers don't look the same in all the languages

|  | Value: 23985.989 |
| :-------- | :------------------------------- |
| In a German text   |                 23’985.99 |
| In a French text  |                 23 985,99 |
| In an English text |                 23,985.99 |

???
- Different decimal and grouping separators
- Depending on the number, this can be ambiguous

---

## Turning data into text
### means taking into account the **language** and **region** of our readers

???

- In a lot of cases, we try to create sensible sentences out of data. Going from "what is the data" to "what does it mean – to you personally". So we create a lot of "You have such-and-such many calls"-type messages.
    - Example: Tax Rate Map of Switzerland
- We have copy editors that make sure that all the text we produce is correct
- Tamedia also has properties in Western Switzerland, so we had to produce the same content in French as well.
- Obviously, we're not the first programmers to encounter this problem.
- The process is called **Internationalisation**, and most programming languages contain libraries that facilitate it
- There is a large library that contains all these informations, that's being used by all the major software companies – the [Unicode Common Locale Data Repository](http://cldr.unicode.org/index)
- That data is part of your operating system. And your browser. So take advantage of it!

---
exclude: true

| Language     | Timestamp: 896781600000 |
| ------------ | ----------------------- |
| English (US) | 6/2/1998                |
| English (GB) | 02/06/1998              |
| German (CH)  | 2.6.1998                |
| French (CH)  | 02.06.1998              |
| French (FR)  | 02/06/1998              |

???
- It even depends on the region


---

# Intl
## lives right in your browser

???
- JavaScript contains the **Intl** object, which covers numbers and date formatting
- Also, depending on the browser, a few more things

---
layout: true

### Intl.NumberFormat

---
```js
const formatForGerman = 
  new Intl.NumberFormat("de-CH").format;

formatForGerman(23985.9897);
//> 23’985.99
```

---
exclude: true

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
layout: true
### Intl.DateTimeFormat

---
exclude: true
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
exclude: true

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
layout: false

## Formatting Messages
???
- Now, we know how to format numbers, but how do we deal with the plural forms?
- Not directly available in the browser, but available on NPM as a package
--

```javascript
import { FormatMessage } from "intl-messageformat";
```

???
- It uses its own mini syntax, and it can be used to set up variations of messages as seen above

---
layout: true
### Message Syntax
---

```javascript
const englishMessage = 
  `You have {numberOfCalls, plural,
    zero {no calls}
    one {one call}
    other {# calls}
  }.`;
```

???
- This syntax is a standard proposed by the ICU, the body that collects all the localisation data.
---

```js
const polishMessage = 
  `Masz {numberOfCalls, plural,
    zero {0 połączeń}
    one {1 połączenie}
    few {# połączenia}
    many {# połączeń}
  }.`;
```

---
layout: false
### Usage

```js
const msgFormat = new FormatMessage(
  polishMessage, 
  "pl-PL"
);

msgFormat.format({
  numberOfCalls: 114
});
//> "Masz 114 połączeń"
```

---

## Why is it nice?

--
- Format is text based – import strings from CSV, JSON or YAML files

--
- … and let someone else translate those messages

???
Leave it to the experts
--
- Also helps with gendered language

???
Some languages are more heavily gendered than English – take Spanish

--
- Less code to maintain

???
Meaning: less opportunities for bugs to creep in

---

## Does it work everywhere?

| Feature        |  Status  | Browser Support       |
| -------------- | -------- | --------------------- |
| NumberFormat   | Standard | Most browsers         |
| DateTimeFormat | Standard | Most browsers         |
| Collator       | Standard | Most browsers         |
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