---
title: 'Every Rule Passed. The Translation Was a Page That Should Not Exist.'
description: 'A short hotel title went into my AI translation pipeline and came out as an invented guest information page. Every check I had built said it was fine.'
pubDate: 'Oct 8 2026'
category: 'AI & Technology'
series: 'Building Reliable AI Workflows: Lessons from a Localization Lab'
seriesPart: 1
tags: ['AI', 'evaluation', 'AI reliability', 'guardrails', 'localization', 'machine translation', 'human in the loop']
---

## The failure

I was testing an AI translation workflow for a fictional hotel in Old Montreal. The workflow retrieves the hotel's glossary and style rules. It generates a translation into Canadian French. It runs a set of automatic checks. Then it sends the result to a quality estimation (QE) model before anything reaches a human. A QE model estimates how good a translation is by comparing it with the source, without a human reference.

Then I gave it a six word title: "Harbour View Hotel Montreal – Guest Information".

It didn't translate the title. It wrote a page.

The output was a full guest information section in French. It had check out times. It had a fitness centre the hotel does not have. It ended with a helpful placeholder: "[insérer l'adresse courriel]" (insert email address). None of it was in the source.

## Why my checks said yes

The obvious story is that the AI made something up. That happens. The story I care about is what happened next. I ran the output through my rule checks, and **every rule passed**.

The checks were not lazy. The brand name stayed in English, as the glossary requires. The reader was addressed as "vous", times were in Quebec format, and Quebec terms were used where France French terms would be wrong.

All of that was true. The invented page was very well formatted French.

My checks confirmed that what was present was correct. Not one of them asked whether it should be present at all. A glossary check can tell you the hotel's name is spelled right. It cannot tell you the paragraph around it is fiction.

> **Validation is not correctness.**
> A system can satisfy every rule you wrote and still produce the wrong thing.

The most important requirement was "translate this, and only this". I had never written it down as something a machine could check.

## My first fix was wrong

My first reaction was to tell the model not to do it. Add "do not invent content" to the prompt and move on.

Then I looked at the prompt. The instruction was already there: "Reply with the translation only. Never add, remove or explain anything." The model had read it and written a page anyway.

So the prompt could not be the whole fix. I needed to understand why a title, of all things, turned into a page.

One important part of the answer was how I passed context. I was sending the glossary and a few reference translations in the same message as the text to translate. For a long paragraph, that worked. For a six word title, the context was far longer than the source. The request looked less like "translate this label" and more like "here is a hotel, write about it".

I don't think context placement was the only cause. A short source, a large prompt and a generative model with no output limit all played a part. But it taught me something useful. Short strings look like prompts.

## The fix was layers, not a better prompt

No single control could have caught this alone. The fix was to layer them, so that each one covers what the others miss.

```
PREVENT    give the model the right context, in the right place
   ↓
CONSTRAIN  limit how much it can reasonably produce
   ↓
DETECT     check the shape of what came out, not only the words
   ↓
ESCALATE   send anything suspicious to a person
   ↓
LEARN      feed approved human edits back into the system
```

**Prevent.** Context moved to the system prompt, separating the reference material from the text to translate. The user message now holds only the text to translate. Short strings (under 8 words) get no reference translations, because examples of full sentences invite full sentences.

**Constrain.** Output length is capped relative to the source. This doesn't guarantee a correct answer. It makes a large expansion much harder, and it gives the pipeline one more rule that does not depend on a model.

**Detect.** New checks look at structure. Is the output more than twice the length of the source? Does it have more lines? Does it contain placeholder text, or words from my own instructions? These checks are crude. They are also cheap, and they catch the shape of this failure.

**Escalate.** A failed check always beats a good score. If any check fires, the translation goes to a review sheet and a reviewer gets a Slack notice. It does not go to the translation memory or the CMS until a person approves it. Only output that passes every check and scores 0.85 or higher skips review.

**Learn.** Reviewer edits are checked again, then saved as approved translations. When the same text comes back, the pipeline reuses the approved version instead of translating it again. Similar text can use approved translations as examples, but never unreviewed machine output.

After these changes, the same title came back as a title: "Harbour View Hotel Montreal – Renseignements pour les clients".

## What each layer can catch

| What can go wrong | Layer that catches it |
|---|---|
| Wrong or misleading context | Prevent |
| Too much output | Constrain |
| Wrong terms, format or structure | Detect (rule checks) |
| Wrong meaning | Detect (model evaluation) |
| Wrong business decision | Escalate (human review) |
| The same mistake next time | Learn |

One failure changed the architecture of the workflow. Before, it was a straight line from translation to checks to review. After, a decision point sits in the middle. Each translation is either trusted to skip review or sent to a person, and the rules for that choice are written in code anyone can read.

## Why this matters beyond translation

The translation example is useful because the failure is easy to see. The product problem is much broader. An AI system needs more than an evaluation score. It needs a definition of its operating boundaries.

| System | Boundary it must not cross |
|---|---|
| Customer support assistant | Don't invent policy |
| Document generator | Don't add commitments nobody asked for |
| Coding agent | Don't change files outside the task |
| Data agent | Don't write to systems without authorization |
| Translation workflow | Don't add content that wasn't in the source |

In every one of these, the dangerous output can look polished. It passes the checks that ask "is this well formed?" It fails the one that asks "should this exist?"

Reliable AI does not come from finding one better evaluator. It comes from defining several kinds of correct, and giving each one the right mechanism. Some are rules in code. Some are model scores. Some need a person.

## What I still don't know

My structure checks work because a title has an obvious expected shape. A title should be short. Most content is not that easy. How long should the French version of a 3 sentence cancellation policy be? What does "nothing added" mean for a marketing paragraph that is supposed to be adapted, not translated word for word?

I don't have a universal answer. But I learned that I don't need to list every possible bad output. I can define what should stay the same: the shape, the scope, the required terms, the number of segments, the fields that are allowed to change, and the wording a person already approved. These are **invariants**. When I can't predict how a system will fail, I can still check that it kept its promises.

That may be one of the harder problems in putting generative AI into real workflows. The question is not only "is this output good?" It is also "what is this system never allowed to change?"

*Scope: a small test pipeline with a handful of entries, run in October 2026. One failure studied closely, not a benchmark.*
