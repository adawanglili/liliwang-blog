---
title: 'So, How Bad Are Wildfires in Quebec, Really?'
description: 'A personal rabbit hole into Quebec wildfire data: I expected the numbers to confirm my worry about wildfire smoke near Montreal. Instead they kept contradicting my assumptions.'
pubDate: 'Aug 26 2026'
heroImage: '../../assets/wildfire-smoke-montreal-hero.png'
category: 'Environment & Data'
tags: ['Wildfires', 'Quebec', 'Air Quality', 'Montreal', 'Data', 'Climate']
---

## The headline that sent me down the rabbit hole

Last month I stumbled on a headline that stopped me mid-scroll: Trump was talking about making Canada pay for wildfire smoke drifting south, with talk of tying the cost to tariffs. Looking closer, most of the smoke behind that story was coming from Ontario, not Quebec. But by then, I had a different question. I live on Montreal's South Shore, so my first reaction wasn't really political: how bad are the wildfires here, actually? How are we doing compared to 2023, and were we seeing more fires than usual? And how much of what we breathe here every summer is actually wildfire smoke, rather than ordinary poor air quality?

I didn't have good answers, so I pulled SOPFEU's season reviews, Quebec's forestry ministry data, and the city's own air quality reports into a small dashboard.

I expected the numbers to confirm my worry. They didn't. At least, not in the way I expected.

## Wait. 2025 had almost as many fires as 2023?

The first number that stopped me:

| Season | Fires (Intensive Protection Zone) | Area burned |
|---|---|---|
| 2023 | 566 | ~1.1 million ha |
| 2024 | 352 | 13,884 ha |
| 2025 | 528 | 1,314 ha |

> Same-ish number of fires in 2023 and 2025. Completely different fire seasons.

The Intensive Protection Zone is the roughly 130 municipalities where SOPFEU actively fights fire. Beyond it is a vast northern zone that's mostly left to burn. That's where most of 2023's damage happened (another 3.2 million hectares on top of the 1.1 million above). SOPFEU's own word for 2025 was "atypical": lots of small fires, almost nothing large, activity pushed later into the year than usual.

So the number of fires in a season told me almost nothing about how bad that season actually was.

## Then I found a number that changed the story

> **99.9%**
>
> The share of the area burned in Quebec's Intensive Protection Zone in 2023 that was ignited by lightning.

On average, about 80% of Quebec's fires are human caused (!), and about 20% come from lightning or go undetermined. But the season that burned the most forest wasn't primarily about any of those things. A record drought met repeated dry lightning storms across the north, and many of the largest fires occurred in the northern zone, where fires are generally left to burn rather than fought.

And, honestly, this was one of those details I didn't expect to find: ATVs really can start forest fires. SOPFEU estimates they cause around 10 fires a year, usually when a hot exhaust pipe touches dry brush.

2025 flipped the script again: 87% of that year's fires were human caused, but they stayed small, in part because there was no comparable lightning event pushing them north. Most fires are started by people, but the biggest fire seasons can be driven by lightning and drought.

## I followed the smoke to Montreal…

So far, fire count had turned out to be a poor predictor of how bad a season actually was. But I still hadn't answered the question that started this: what does any of this mean for someone breathing the air on Montreal's South Shore?

| Year | Smog days | Notes |
|---|---|---|
| 2023 | 12 | 34 poor air quality days total, 33 tied to fine particles, 19 days of wildfire-smoke air-quality alerts |
| 2024 | 5 | close to none from wildfire smoke |

That's when the story changed again. The thing I had been thinking about as a forest fire problem was, from where I live, really an air quality problem.

## A bad fire season doesn't necessarily mean a bad Montreal summer

Here's the part I didn't expect: a bad fire season in the forest and a bad air day in Montreal are only loosely connected.

Forest fire severity depends on fire count, area burned, and location. What Montreal actually experiences depends on smoke transport, wind, atmospheric conditions, and how much fine particulate (PM2.5) that smoke is carrying by the time it arrives. 2021 had 7 smog days, 4 of them from smoke drifting in from Ontario and Manitoba, nothing burning in Quebec at all. 2022 had 4 smog days, and the city didn't even break down the cause.

So a huge fire in northern Quebec doesn't automatically mean terrible air in Montreal. And a small fire far away can still give us a smoky day if the wind lines up.

## Then 2026 complicated the story

And then, while I was writing this, 2026 happened.

By July 20, 2026, SOPFEU reported 336 fires had affected about 124,000 hectares in the Intensive Protection Zone, and 278 fires had affected about 600,000 hectares in the northern zone. The situation was serious enough that Quebec expanded its open-fire ban and brought in extra crews from Alberta, British Columbia, New Brunswick, and Prince Edward Island. That's the kind of pressure that likely fed into the tariff conversation happening in Washington around the same time, even though the smoke making headlines there was mostly Ontario's.

Suddenly my original question didn't seem quite so silly.

But there was a catch: 2026 wasn't over.

That same day, conditions had improved enough that Quebec lifted the forest access and open-fire restrictions. It's too early to compare an unfinished season with a completed one like 2023. So far, though, nothing published suggests it's heading there.

And that's probably the biggest thing I took away from digging into the data: the headline numbers don't always tell the story you think they do.

A lot of fires doesn't necessarily mean a lot of forest burned. A huge wildfire season doesn't necessarily mean Montreal will have a terrible summer. And smoke making headlines in the U.S. doesn't necessarily mean it came from Quebec.

Sometimes you have to follow the numbers a little further than the headline.

After going through all these numbers, I also wondered what any of this means at an individual level. We can't do much about lightning or drought, but we can avoid adding to the problem: check fire restrictions, be careful with campfires and outdoor burning, and take extra care with equipment that can get hot and ignite dry vegetation. And when smoke does arrive, pay attention to the air quality where you are, not just what's happening on the fire map.

If you want to poke at the numbers yourself, I put the data and sources into a small dashboard: [Feux de Forêt, Quebec](https://adawanglili.github.io/quebec-wildfires/).

Sources: [SOPFEU season reviews](https://www.sopfeu.qc.ca/en/review/), [Données Quebec wildfire dataset](https://www.donneesquebec.ca/recherche/dataset/feux-de-foret), [Ville de Montreal air quality reports](https://montreal.ca/sujets/qualite-de-lair), [SOPFEU, July 20 2026 lifting of restrictions](https://www.sopfeu.qc.ca/communiques/levee-totale-de-l-interdiction-de-faire-des-feux-a-ciel-ouvert-en-foret-ou-a-proximite_20260720/), [Gouvernement du Quebec, July 20 2026](https://www.quebec.ca/en/news/actualites/detail/forest-fires-total-lifting-of-the-prohibition-of-traffic-in-and-access-to-forests-72053), [NPR on the US tariff threat](https://www.npr.org/2026/07/18/nx-s1-5899009/us-canada-wildfire-smoke).
