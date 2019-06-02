classes: center middle
# How to Turn Data into Language

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
