---
title: 'Localization Already Had Retrieval. The Harder Problem Was Always What to Do With It.'
description: 'What RAG and LLMs add to mature TMS retrieval.'
pubDate: 'Sep 14 2026'
heroImage: '../../assets/01-retrieval-interpretation-orchestration.png'
category: 'AI & Technology'
series: 'Rethinking Localization in the Age of AI'
seriesPart: 1
---

*What TM, MT, style rules, RAG, and LLMs are actually changing in localization.*

I wanted to trial a RAG-based architecture using a domain I know deeply: localization. The exercise raised a more useful question than whether an LLM can produce a good translation: **if mature TMSs already retrieve linguistic knowledge, where does AI actually add value?**

## Localization has been retrieving context for years

A modern TMS rarely starts translation from a blank page. Translation Memory provides exact and fuzzy matches. Termbases provide approved or forbidden terminology. Background TMs, linked component TMs, metadata, content types, matching thresholds, priorities and penalties can all influence what gets surfaced.

That is already sophisticated retrieval.

It is reasonable to say that localization has long practiced workflows that are *similar in spirit* to retrieval-augmented generation, even though a traditional TMS and modern RAG are not the same architecture. Traditional TMS retrieval is highly structured and often deterministic; RAG commonly retrieves heterogeneous evidence for a generative model.

The important point is that retrieval itself is not new.

## The interesting problem starts after retrieval

Imagine a new sentence receives a 95% TM match. The match is strong, but it came from a hotel cancellation workflow while the new content concerns airline tickets.

The TMS can already use metadata, TM priority, content type and other configuration to improve relevance. But there is still a judgment call: should the match be reused, adapted, or rejected?

A human linguist can make that decision.

MT can interpret the source and generate a translation.

An LLM can be given the retrieved examples, terminology and rules and asked to reason over those signals before generating.

So the architecture is better described as:

```text
retrieve
   ↓
interpret context
   ↓
reuse / adapt / generate
   ↓
evaluate
```

Retrieval is evidence. It is not automatically the answer.

## MT already understands context

It is tempting to frame this as a failure of machine translation to understand context. That would be too simplistic.

Modern neural MT can model sentence-level context very well. The broader localization problem is that the relevant context can be distributed across many sources: previous translations, terminology, metadata, product conventions, style guidance and sometimes information that does not exist inside the TMS at all.

The difficult cases involve deciding which signals matter and how they should influence the output.

## Some decisions should remain deterministic

There are plenty of problems where an LLM adds unnecessary complexity.

For example:

- preserve this placeholder;
- use the approved product name;
- do not translate this brand term;
- use the required date format.

Traditional QA and terminology systems are excellent at these jobs.

The interesting cases are contextual.

> Use a reassuring tone when communicating a service disruption.

A string can be grammatically correct, use the approved terminology and still sound too blunt for the situation. That is much harder to express as a simple deterministic check.

Phrase is now moving style guidance into structured **Rules**, with eligible rules generating corresponding **AI Checks** for quality evaluation. Smartling similarly provides **Style Rules for AI** that can be used in translation and quality workflows. [Phrase Rules](https://support.phrase.com/hc/en-us/articles/28818120405788-Rules) · [Phrase AI Checks](https://support.phrase.com/hc/en-us/articles/28818120438300-AI-Checks) · [Smartling Style Rules for AI](https://help.smartling.com/hc/en-us/articles/41970369123227-Style-Rules-for-AI)

## So where does RAG fit?

A useful system would route content according to the strength of the available evidence:

```text
SOURCE
  ↓
TM / TB / RULES / METADATA
  ↓
ASSESS CONTEXT
  │
  ├─ strong, appropriate match → reuse TM
  │
  ├─ strong but not exact → use TM as adaptation context
  │
  └─ weak / insufficient → MT or LLM generation
                                  ↓
                              QA / evaluation
```

Current products already show pieces of this model. Smartling's AI Adaptive Translation Memory uses an LLM to optimize fuzzy matches between 50% and 99.9%. Lokalise can retrieve TM or reviewed project translations as RAG context for AI translation. [Smartling AI Adaptive TM](https://help.smartling.com/hc/en-us/articles/25163532193307-AI-Adaptive-Translation-Memory) · [Lokalise AI Profiles](https://docs.lokalise.com/en/articles/11894216-ai-profiles)

## Match confidence and business risk are different

Two dimensions are particularly useful.

**Match confidence:** How much can we trust the context we retrieved?

**Business risk:** How much does it matter if we get the translation wrong?

| | Low risk | High risk |
|---|---|---|
| **High confidence** | Reuse + basic QA | Reuse + strict QA |
| **Low confidence** | MT/LLM + AI QA | MT/LLM + deeper QA + human review |

The exact routing will vary by organization. The important principle is that human review can be risk-based rather than automatically applied to every string.

A low-risk UI label with a strong contextual match may need very little intervention. A high-impact communication with weak evidence may deserve much more.

## The bigger question

RAG becomes interesting for localization not because retrieval is new, but because it gives a generative system access to organizational knowledge at the moment a decision is being made.

That knowledge can include TM, terminology and rules.

But the next question is even broader:

**What if the context needed to localize something does not live in the TMS at all?**

Next in the series: *The Context Localization Needs Doesn't Always Live in the TMS.*

### Industry examples
- [Phrase Rules](https://support.phrase.com/hc/en-us/articles/28818120405788-Rules)
- [Phrase AI Translation Agent](https://support.phrase.com/hc/en-us/articles/20660272640284-AI-Translation-Agent)
- [Smartling AI Adaptive Translation Memory](https://help.smartling.com/hc/en-us/articles/25163532193307-AI-Adaptive-Translation-Memory)
- [Lokalise AI Profiles](https://docs.lokalise.com/en/articles/11894216-ai-profiles)
