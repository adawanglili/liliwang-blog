---
title: 'Localization Already Had Retrieval. So What Does AI Add?'
description: 'Where RAG and LLMs add value to the localization systems we already have.'
pubDate: 'Sep 14 2026'
heroImage: '../../assets/localization-ai-pipeline-hero.png'
category: 'AI & Technology'
series: 'Rethinking Localization in the Age of AI'
seriesPart: 1
seriesTotal: 5
tags: ['Localization', 'AI', 'TMS', 'RAG']
---

After more than a decade working with localization platforms, I wanted to try something from a different angle: building a RAG-based architecture using localization as the domain.

As I worked through the experiment, it prompted me to step back and look more closely at how localization platforms are evolving around AI. The more I looked, the more interesting the question became: **if mature TMSs already retrieve linguistic knowledge, where does AI actually add value?**

That question became the starting point for this series.

## Localization has been retrieving context for years

A modern TMS rarely starts translation from a blank page. Translation Memory provides exact and fuzzy matches. Termbases provide approved terminology and business rules. Primary TM, background TMs, metadata, content types, matching thresholds, priorities and penalties can all influence what gets surfaced. That is already sophisticated retrieval.

RAG is a term that surfaced in my world in 2024. Localization has long practiced workflows that are *similar in spirit* to retrieval-augmented generation, even though a traditional TMS and modern RAG are not the same architecture. Traditional TMS retrieval is highly structured and often deterministic; RAG commonly retrieves heterogeneous evidence for a generative model. Retrieval itself is not new.

## The story starts after retrieval

Consider a source segment as simple as:

> **Apply**

A TMS may have a strong approved translation from previous projects. But "Apply" can represent very different actions depending on the product context: applying a filter, applying a discount, applying a setting, or applying for a service. The previous translation may be linguistically valid and still not be the right translation for the new occurrence.

The TMS has done something valuable: **it has retrieved relevant linguistic history.** The remaining question is how that history should be interpreted in the current context.

Imagine the new UI request in the TMS includes a screenshot showing that **Apply** is the button used to apply a set of search filters. A human linguist can look at the screenshot and immediately use that information when choosing the translation.

But whether the **translation engine** can use that screenshot is a different architectural question.

A conventional NMT engine receives the source text and whatever contextual signals its integration provides. It does not automatically gain the same visual understanding that the linguist has simply because a screenshot exists in the TMS.

If the TMS instead provides a textual explanation such as "This button applies the selected filters," and that explanation is actually passed to the NMT engine as context, then NMT can use that information. The issue is therefore not that NMT cannot use context. The issue is **what context is supplied to the engine, in what form, and how the engine can consume it.**

A multimodal LLM could go a step further by receiving the source string together with the screenshot and reasoning over both. But that is a capability of the model and its integration, not simply a consequence of the screenshot existing somewhere in the localization workflow.

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

## Not every localization problem needs AI

The fact that AI can interpret context does not mean every localization decision needs an AI model.

Many localization requirements are explicit and deterministic. If the requirement is:

- preserve this placeholder;
- use the approved product name;
- do not translate this brand term;
- use the required date format;

then there is little ambiguity to resolve. Traditional terminology management and automated QA already enforce or detect these requirements effectively.

The more interesting cases are where the requirement depends on **meaning, situation, or interpretation**.

Consider a style requirement such as:

> Use a reassuring tone when communicating a service disruption.

A traditional QA rule can check whether a required term appears, whether a placeholder was preserved, or whether a number is correct. But determining whether a translation actually sounds reassuring is a different kind of problem. It requires evaluating the language against the intended context and the rule.

TMS vendors are beginning to turn that human-readable guidance into machine-usable instructions.

Phrase, for example, is moving style guidance into structured **Rules**, with eligible rules generating corresponding **AI Checks** for quality evaluation. Smartling similarly provides **Style Rules for AI** that can be used in translation and quality workflows. [Phrase Rules](https://support.phrase.com/hc/en-us/articles/28818120405788-Rules) · [Phrase AI Checks](https://support.phrase.com/hc/en-us/articles/28818120438300-AI-Checks) · [Smartling Style Rules for AI](https://help.smartling.com/hc/en-us/articles/41970369123227-Style-Rules-for-AI)

The significance is not simply that an LLM is being added to QA.

It is that **some guidance that was previously written for humans can now be expressed as a machine-evaluable rule.**

Phrase's documentation also makes an important qualification: adherence to individual rules by its AI Translation Agent remains probabilistic. Turning a style guideline into an AI-evaluated rule does not make the requirement deterministic.

So there are really two different kinds of checks:

```text
Explicit / deterministic requirement
        ↓
Traditional QA / terminology checks
        ↓
"Is the placeholder preserved?"
"Is the approved term used?"
"Is the date format correct?"

Contextual / interpretive requirement
        ↓
AI-assisted evaluation
        ↓
"Does this sound appropriately reassuring?"
"Does this follow the intended tone?"
"Does this translation fit the described context?"
```

This distinction matters because it tells us where AI is actually adding something new. The opportunity is not to replace deterministic localization controls. It is to extend automated evaluation into areas that previously required a human to interpret the requirement and judge the result.

## So where does RAG fit?

Once we separate deterministic checks from contextual interpretation, the role of RAG becomes clearer.

A localization system can retrieve evidence from TM, TB, rules and metadata. AI can then use that evidence to interpret the current context, generate or adapt a translation, and evaluate whether the result follows the relevant requirements.

A useful system would therefore route content according to the strength of the available evidence:

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

These examples point to an important distinction: RAG does not eliminate the value of TM. It can change how TM is used.

Instead of treating a retrieved translation as simply *the answer*, the system can use it as evidence for generating or adapting a new translation.

## Context quality matters as much as retrieval quality

Once retrieved information becomes input to a generative model, **its quality and relevance become part of the generation problem.**

A TM containing excellent translations for one type of content may not be equally useful for another.

For example, Lokalise explicitly notes that mixed or inconsistent TM content can cause AI output to inherit the wrong tone or style. [Lokalise AI Profiles](https://docs.lokalise.com/en/articles/11894216-ai-profiles)

This brings us back to the *Apply* example.

Suppose the TMS retrieves an excellent translation for **Apply**.

The question isn't whether the translation is good.

The question is whether the **reason it was good in the previous context also applies here**.

That is the harder problem: not finding a translation, but understanding when the retrieved translation should influence the next decision. It changes what we should ask of a localization system: not whether a match is strong, but whether the retrieved context is actually relevant, and whether the system can use it appropriately.

## The bigger question

RAG becomes interesting for localization not because retrieval is new, but because it gives a generative system access to organizational knowledge at the moment a decision is being made.

That knowledge can include TM, terminology and rules.

But the "Apply" example exposes a larger possibility.

The screenshot may be in the localization platform, the design spec in Figma, the implementation in GitHub, the feature requirement in Jira. The product behavior itself may be documented somewhere else entirely.

The localization system may know that the string exists, while another system contains the information needed to understand what the string actually does.

That leads to the next question:

**What if the context needed to localize something does not live in the TMS at all?**

**Next in the series:** *The Context Localization Needs Doesn't Always Live in the TMS.*

### Industry examples

- [Phrase Rules](https://support.phrase.com/hc/en-us/articles/28818120405788-Rules)
- [Phrase AI Translation Agent](https://support.phrase.com/hc/en-us/articles/20660272640284-AI-Translation-Agent)
- [Smartling AI Adaptive Translation Memory](https://help.smartling.com/hc/en-us/articles/25163532193307-AI-Adaptive-Translation-Memory)
- [Smartling Style Rules for AI](https://help.smartling.com/hc/en-us/articles/41970369123227-Style-Rules-for-AI)
- [Lokalise AI Profiles](https://docs.lokalise.com/en/articles/11894216-ai-profiles)
