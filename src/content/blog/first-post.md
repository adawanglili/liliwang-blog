---
title: 'What I Finally Understand About AI'
description: 'A few simple ideas that helped me understand how large language models actually work.'
pubDate: 'Aug 28 2026'
heroImage: '../../assets/blog-placeholder-3.jpg'
category: 'AI & Technology'
---

I've spent quite a bit of time around AI, but for a long time I realized that I could talk about AI without really understanding what was happening underneath.

So I decided to go back to the basics.

The simplest way I now think about a large language model is this:

> It learns patterns from an enormous amount of text and uses those patterns to predict what comes next.

That's it.

Of course, what happens underneath is incredibly complicated. But this simple idea helped me connect many of the concepts that had previously felt disconnected.

## Tokens

Before a language model can work with text, it breaks text into smaller pieces called tokens.

A token might be a word, part of a word, punctuation, or something else.

The model doesn't actually "read" a sentence the way we do.

It works with numbers representing these tokens.

## The model learns patterns

During training, the model sees enormous amounts of text.

It repeatedly tries to predict what comes next.

For example:

> The sky is...

A model might learn that words such as "blue", "clear", or "dark" are plausible continuations depending on the context.

After doing this billions or trillions of times, the model becomes extremely good at recognizing patterns in language.

## Why does it look intelligent?

This is the part I find most interesting.

The model isn't simply storing sentences and looking them up.

It learns relationships between concepts, words, structures, and contexts.

As the model becomes larger and is trained on more data, surprisingly sophisticated behaviors can emerge.

That's why something that started as "predict the next token" can eventually write code, summarize documents, translate languages, reason through problems, and hold a conversation.

## I'm still learning

I don't think understanding AI means memorizing every technical detail.

For me, the more useful goal is to build a mental model that is simple enough to use and accurate enough to be useful.

This blog is partly an attempt to do exactly that:

to take complicated ideas, understand them, and explain them in plain language.