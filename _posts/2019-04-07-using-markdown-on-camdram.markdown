---
layout: post
title:  "How to use Markdown on Camdram"
date:   2019-04-07 14:47:00 +0000
author: "George Bateman"
---

<style>
blockquote {
color: #222; font-style: normal;
}
</style>

Since earlier this year we've supported Markdown on Camdram to format your pages on Camdram. It works on shows, societies, venues and all types of vacancy adverts. Markdown was designed in 2004 by [John Gruber](https://daringfireball.net/projects/markdown/) to be a simple way of typing formatted text into a website. It's supposed to look a lot like a plain text email.

## The basics

Regular unformatted text should just work. Formatting is usually easy.

### Links
You can put links in any of these ways:

```markdown
Watch it here for 3 minutes for pure, unadulterated facts: https://youtu.be/vqsAL8kXz5c
See our [Google Drive](https://drive.google.com/12345) for more details.
Email our producer [Jane Smith](mailto:xy123@example.ac.uk).
```

And you end up with the result:
> Watch it here for 3 minutes for pure, unadulterated facts: <https://youtu.be/vqsAL8kXz5c>
>
> See our [Google Drive](https://drive.google.com/12345) for more details.
>
> Email our producer [Jane Smith](mailto:xy123@example.ac.uk).

### Bold and italic

You can get bold and italic by surrounding a bit of text with underscores or asterisks:

```markdown
**This is bold**, *this is italic*, __bold again__, _italics_. In the middle_of_a_word only ast**er**isks have any effect.
```

>**This is bold**, *this is italic*, __bold again__, _italics_. In the middle\_of\_a\_word only ast**er**isks have any effect.

### Headings

Heading sizes in theory range from 1-6 (with one being the biggest), but since what you write is only ever part of a page we don't display it in the biggest sizes. We decided to interpret what you write as:

```
# Heading size 3 (big)
## Still heading size 3
### Heading size 4 (smaller)
#### Heading size 5
##### Heading size 6 (tiny)
###### Heading size 6 (tiny)
```

### Horizontal lines

Want to split sections of your advert but make it look a bit nicer than just writing `----`?

```markdown
Paragraph 1

------

Paragraph 2
```

becomes
> Paragraph 1
>
> ------
>
> Paragraph 2

(You can have a few more or less hyphens, but make sure there's a gap between paragraph 1 and `------` or else paragraph 1 turns into an enormous heading!)

### Lists

```markdown
- Thing 1
- Thing 2
- Thing 3

1. Another list
2. of things
3. that now are numbered
```

### Quotes

```markdown
> To be, or not to be, that is the question:
> — Prince Hamlet, *Hamlet*, Act 3.
```
> To be, or not to be, that is the question:
>
> — Prince Hamlet, *Hamlet*, Act 3.

## More unusual features

### Tables

These are fiddly, but you probably won't need them much:

```markdown
(notice the blank line between here and the table)

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |
```

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |

### Escaping; or "I just typed one hyphen and everything exploded??"

Sometimes the Markdown software will think you asked for formatting when all you wanted was an asterisk, or whatever. Fortunately, if you put a backslash `\` just before the offending symbol, it will understand what you mean.

```markdown
One asterisk* on a line causes no problems at all.
Two *asterisks cause* trouble.
A couple \*of backslashes\* clear it up nicely.
```

> One asterisk* on a line causes no problems at all.
>
> Two *asterisks cause* trouble.
>
> A couple \*of backslashes\* clear it up nicely.

## Everything else

### HTML?

If you're wondering if you can use [HTML](https://en.wikipedia.org/wiki/HTML), the answer is not really. Some of our older pages may have a bit of HTML in them, so we convert bold, italic, and a couple of other tags to Markdown, then feed the whole thing through a Markdown parser which has been told not to accept any other HTML. We don't recommend you try to use this in new pages though!

### Pictures

For bandwidth reasons, you can only upload one picture per show, society or venue. You don't need to add any special code to the description, just click the upload image button on the main page. (Venues automatically get a map too if you specify the latitude and longitude.)

### Older pages

If you're a society or venue admin you might be able to edit some older pages, and you may come across this sort of thing:

```
[L:https://example.com] (a link)
[E:abc@example.ac.uk] (a link to an email address)
<br>, <b>, <i>, etc. (This is HTML.)
```

This is an alternative to Markdown we created ourselves a number of years ago. It's no longer recommended and at some point we'll probably stop supporting it. (It doesn't bring any features that aren't available with regular Markdown!)

