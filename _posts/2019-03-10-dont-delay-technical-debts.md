---
layout: post
title: Don't delay your technical debts!
subtitle: >-
    // Todo, the foundation of your future technical limitation and frustration
    that you planned today.
avatar: /img/avatars/avatar-code.png
gist: >-
    The author feels that gist of this article can be written down some other day
    due to shortage of time and would like to  add the technical debt!
categories:
    - TechTalk
    - TechDebt
    - Opinion
tag:
    - TechTalk
    - TechDebt
    - Opinion
draft: false
---

#### Part 1 of 2

Read the second part - [Coping up with technical debts](/2019/04/07/coping-with-technical-debts/)

Following article would explain most of scenarios with regards to frontend/web development but simply can be applied as in general software development.

### Table of contents

<!-- toc -->

-   [First, What is this Technical Debt?](#first-what-is-this-technical-debt)
-   [Why can't debt be delayed?](#why-cant-debt-be-delayed)
-   [Product debt vs Technical debt](#product-debt-vs-technical-debt)
-   [Side-effects](#side-effects)
    -   [Frustrations and De-motivation for Development Teams.](#frustrations-and-de-motivation-for-development-teams)
    -   [“if it’s not broke, don’t fix it” disease.](#if-its-not-broke-dont-fix-it-disease)
    -   [Prolonged design limitation](#prolonged-design-limitation)
    -   [Reactive product developement instead of proactive.](#reactive-product-developement-instead-of-proactive)

<!-- tocstop -->

### First, What is this Technical Debt?

There may be multiple ways to define this jargon, to simple put it, `Technical debt`, in terms of software development, is implied cost of additional rework caused by choosing an easy solution rather than a known better approach/solution that would take longer time to implement. Sometimes this so called short cut, is either due to lack of time, lack of resources, urgency of cause, or simply monetary. Although it is not always avoidable but should be taken into account before moving ahead with development. As the article title says, "don't delay" rather than "don't keep" technical debt.

### Why can't debt be delayed?

Simply, debt always increases and most of the times in non-linear fashion.

Debt, even in financial aspects as well should be cleared off as soon as possible and shouldn't be delayed. If you use a credit card for your financial needs, you will understand the burden that adds due to delay in the monthly payment of dues. Some credit card can add interest upto 13-15% monthly or 40% annually. But what if you end up paying the entire bill amount by end of payment cycle and do not delay it? No pain no harm. More delay equals more burden to your pocket.

Same is the case with technical debts. The difference being, no theoretical means can estimate the eventual interest piled up since not only money is involved but also human factor comes into play. For example, during prototyping you can probably opt for in-memory storage for things (say session storage for our case) or something similar considering time limitation. This is fine and perfect and would suffice the requirement. But development team would be aware of 'better solution', like in case, session storage to be on distributed object caching system like memcache, redis, etc. Now, prototype would work, would also end up in production after acceptance, time goes by, everything works fine for sometime. Eventually, things will start to show the 'debt'. Usage of in-memory would spread across layers because multiple developers worked on it, race conditions are hard to handle, session storage is acting as a cache for multiple flows, functions and other sharable things across layer are part of this 'in-memory' storage. With time, since your product has grown and also its users, it would require scaling, horizontally and vertically. But problem is now, its hard (not impossible) to change across layer and introduce distributed storage. What could had been done in days,is now estimated to be done in weeks. Plus some post damages in terms of regression bugs to fix them.

### Product debt vs Technical debt

Understanding what part of missing piece is technical debt rather than product debt is essential. Let us consider this aspect with a product example. Consider this blog site, has set of limited articles(as of now). There is this feature associated with articles about approximate reading time like "~5 min read" or "10 min read", on blogging sites like medium/wordpress, which helps user to judge it. Either mark it as a bookmark, or keep it in one of hundred tabs in browser, or read it now, or get a coffee before starting to read, or simply move from chair to couch for comfortably reading it. It is established feature that helps the user on such a blogging site. Something like shown in the pic,

![Read length feature image](/img/post/dont-delay-time-feature.png 'Read length')

This blogging site does not have this feature for now (as of now, but will eventually). This is simple product debt and not technical. We may not choose it to implement at all. But learning that this is an urgent requirement, I may choose to implement it statically, i.e each article at the top would have static indication '10 min read' based on article length and average words read by user, say each 50 words add up to 1 minute. This implementation seems to be technical debt. Because I can on a heuristic manner, add this to each article.

Why is this technical debt? Because there is a better solution technically. Since each article is generated via static site generator - jekyll, a function can be implemented which takes post length, predict the reading time and associate it with each article. This is better for, 1. not rely on author understanding 2. Future proof to tweak this function.

Seems a simple debt, but imagine if after 100 articles, for which I have been adding reading time manually and investing manual time in predicting the length, I learn that users of this blog actually work on different reading speed, say 100 words per minute. If done manually, I would need to account this, change all previous post (say 100) as per new math. Later we realise that this feature relies on the category of post, technical post with code, are low in reading time while verbose ones, have high reading time. Again this would require re-calculations for all post. This is a simple example of debt of changing each post everytime.

Managing this feature for long term would be tedious since purpose of blog site was mainly articles. It would be better if a product feature itself was designed with future requirements governing technical aspects as well. A developer always pays and will need to pay their technical debts no matter what!

Even though this is very small and basic example, re-work were also in magnitude of the technical debt. Simply, if you take 100$ as loan for a year with 10%, you will need to pay 10$ extra, while on same rate 20 million $ would require additional 2 million. Certainly, 10$ example is no match to 2million\$. But the discussion is not about interest but the rate, i.e 10%. However small, but would always be relative. Technical debt is like a loan. The longer you postpone paying it back, the more interest you pay, irrespective of small or big piece.

### Side-effects

Apart from monetary and time aspect that is pumped for technical debt, here are few other side effects that add up to the unwanted cause.

#### Frustrations and De-motivation for Development Teams

This is one of the biggest side-effect that generates over time. If couple of high issues keep adding, and technically product is not at 'state-of-the-art' level. Developer and teams working on such product often feel frustrated, and of course this frustration would result into other side effects. Nonetheless, it also demotivates and team takes a hit on its morale.

#### “if it’s not broke, don’t fix it” disease

If debt are huge, developer and in fact, teams hessit to clear up the huge task which was tiny in the beginning. This further results into 'turning a blind eye' behavior, not taking up issue unless it becomes a must fix. Final nail in the coffin is spread of this behavior/culture across devs, teams, orgs and company. For any organization, this is a huge cultural damage and hard to recover from.

#### Prolonged design limitation

A part of the entire system, if suffers from technical limitations, consumer of the service also suffer over long time. If core service (say authentication) can only handle 'x' api calls per minute, other parts of the product will need to design its feature in accordant manner. Thus, adding a design limitation to multiple service and product as a whole.

#### Reactive product development instead of proactive

With legacy system, one has to always do work-arounds to accomplish and implement latest features and trending stuff. This pushes the product into reactive mode and takes away proactive product development mode, implying when the need would arise based on external forces things would be implemented.

---

Technical debt are always compared in terms of numbers and finances, like in this post. But more then the calculated metrics, it is something that is beyond numbers. Something like health, the more prolonged bad state it has, the harder it becomes to recover, in fact sometimes it also becomes unrecoverable damage.

So do you have any important tech-debt? Fix it now, because if it is not done now, it will probably never be done.

---

Read the second part - [Coping up with technical debts](/2019/04/07/coping-with-technical-debts/)
