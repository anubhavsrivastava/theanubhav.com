---
layout: post
title: >-
    Don't delay your technical debts!
subtitle: >-
    // Todo, the foundation of your future technical limitation and frustration that you planned today.
avatar: /img/avatars/avatar-code.png
gist: >-
    The author feels that gist of this article can be written down some other time due to shortage of time!
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

Following article would explain most of scenarios with regards to frontend/web development but simply can be applied as in general software development.

### Table of contents

<!-- toc -->

<!-- tocstop -->

### First, What is this Technical Debt?

There may be multiple ways to define this jargon, to simple put it, `Technical debt`, in terms of software development, is implied cost of additional rework caused by choosing an easy solution rather than a known better approach/solution that would take longer time to implement. Sometimes this so called short cut, is either due to lack of time, lack of resource, urgency of cause, or simply monetary. Although it is not always avoidable but should be taken into account before moving ahead with development. As the articles title says, "don't delay" rather than "don't keep" technical debt.

#### Why can't debt be delayed?

Simply, debt always increases and most of times in non-linear fashion.

Debt, even in financial aspects as well should be cleared off as soon as possible and shouldn't be delayed. If you use credit card for your financial needs, you will understand the burden you add in delaying the monthly payment of dues. Some credits card can add interest upto 13-15% monthly or 40% annually. But what if you end up paying entire bill amount by end of payment cycle and do not delay it? No pain no harm. More delay equals more burden to your pocket.

Same is the case with technical debts. The difference being, no theoretical means can estimate the eventual interest piled up since not only money is involved but also human factor comes into play. For example, during prototyping you can probably opt for in-memory storage for things (say session storage for our case) or something similar considering time limitation. This is fine and perfect and would suffice the requirement. But development team would be aware of 'better solution', like in our session storage to be on distributed object caching system like memcache, redis, etc. Now, prototype would work, would also end up in production after acceptance, time goes by, everything works fine for sometime. Eventually, things will start to show the 'debt'. Usage of in-memory would spread across layers because multiple developers worked on it, race conditions are hard to handle, session storage is acting as cache for multiple flows, functions and other sharable things across layer are part of this 'in-memory' storage. With time, since your product has grown and also its use, it would require scaling. But problem is now, its hard (not impossible) to change across layer and introduce distributed storage. What could had been done in days now is estimated to weeks. Plus some post damages in terms of bugs.

#### Product debt vs Technical debt

---
