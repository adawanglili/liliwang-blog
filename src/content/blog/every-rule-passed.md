---
title: 'Every Rule Passed. The Translation Was a Page That Should Not Exist.'
description: 'A short hotel title went into my AI translation pipeline and came out as an invented guest information page. Every check I had built said it was fine.'
pubDate: 'Oct 8 2026'
heroImage: '../../assets/every-rule-passed-hero.png'
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

The AI made something up. Fine. What caught my attention was what happened when I ran it through the checks: **every rule passed**.

The checks were not lazy. The brand name stayed in English, as the glossary requires. The reader was addressed as "vous", times were in Quebec format, and Quebec terms were used where France French terms would be wrong.

All of that was true. The invented page was very well formatted French.

My checks confirmed that what was present was correct. Not one of them asked whether it should be present at all. A glossary check can tell you the hotel's name is spelled right. It cannot tell you the paragraph around it is fiction.

> **Validation is not correctness.**
> A system can satisfy every rule you wrote and still produce the wrong thing.

The most important requirement was "translate this, and only this". I had told the model. I had never turned it into a check.

## A louder prompt was not the fix

My prompt already said: "Reply with the translation only. No notes or explanations." The model had that instruction and wrote a page anyway.

My first instinct was to say it louder. Add another rule. Add capital letters. Write "do NOT invent content". But a stronger instruction is still only a request. If the model ignored it once, it could ignore it again.

So the prompt could not be the whole fix. I needed to understand why a title, of all things, turned into a page.

I went back through the earlier runs. They had a different problem. The glossary and reference translations sat in the same message as the title, and the model echoed them back into its output. Moving them to the system prompt fixed that. The invented page appeared right after that change. One fix had exposed another failure.

Looking at that run, the prompt was working against itself. It told the model it was a translator "for a hotel website". It gave a reference translation describing the hotel. Then it sent six words that read like the heading of a guest page. One rule even said to translate the text inside `<source>` tags, but the message had no source tags. An instruction is not a boundary if nothing enforces it. Nothing in the workflow constrained the length of the answer either. For a long paragraph, none of this mattered. For a short title, the context was much bigger than the text. The request looked less like "translate this label" and more like "here is a hotel, write its guest page".

The model didn't simply ignore an instruction. It understood the domain and lost the boundary of the task. Short strings look like prompts.

I rewrote the prompt. It now describes a translation engine that never writes new content. It says a title must stay a title. Short strings get no reference translations. The next run returned a title: "Harbour View Hotel Montreal – Renseignements pour les clients". I made those changes together, so I can't say which one was decisive. And a prompt is still only a request. I needed more than that.

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

**Prevent.** The prompt describes a translation engine and says a title must stay a title. Reference material sits in the system prompt, apart from the text to translate. Short strings (under 8 words) get no reference translations, because examples of full sentences invite full sentences.

**Constrain.** Output length is capped relative to the source. This doesn't guarantee a correct answer. It makes a large expansion much harder, and it gives the pipeline one more rule that does not depend on a model.

**Detect.** New checks look at structure. Is the output more than twice the length of the source? Does it have more lines? Does it contain placeholder text, or words from my own instructions? These checks are crude. They are also cheap, and they catch the shape of this failure.

**Escalate.** A failed check always beats a good score. If any check fires, the translation goes to a review sheet and a reviewer gets a Slack notice. It does not go to the translation memory or the CMS until a person approves it. Only output that passes every check and scores 0.85 or higher skips review.

**Learn.** Reviewer edits are checked again, then saved as approved translations. When the same text comes back, the pipeline reuses the approved version instead of translating it again. Similar text can use approved translations as examples, but never unreviewed machine output.

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
