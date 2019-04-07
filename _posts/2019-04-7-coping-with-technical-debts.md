---
layout: post
title: Coping up with technical debts as a developer!
subtitle: >-
    Understanding developer's take on technical debt and things around it.
avatar: /img/avatars/avatar-code.png
gist: >-
    Technical debt may be necessary in products, but how one keep a check on it and happily track it.
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

#### Part 2 of 2

Following article would explain most of scenarios with regards to frontend/web development but simply can be applied as in general software development.

### Table of contents

<!-- toc -->

<!-- tocstop -->

### First, explanation to “if it’s not broke, don’t fix it” disease

“if it’s not broke, don’t fix it” paradigm is not a problem as such, but if that becomes a excuse/reason for all wrong things that are visible in a product/technical every now and then, that becomes a disease. For instance, Instagram nearly fell prey to these growing disease early on. When it launched its iPhone app in October 2010, it ran its operation off of a single server in Los Angeles. It worked without being "broken" for sometime. But after an onslaught of traffic nearly crashed the server, Instagram pivoted in three days to an EC2-hosted database. Co-founder Mike Krieger compared the transfer to open-heart surgery, and he now works to preemptively address technical debt before it leads to catastrophe.

### Understanding the view of debt and making it purely technical

Like a glass that is half filled with liquid, someone would call it half empty. Both the 'view' of situation is correct. Someone would even extend it to say glass is full with half liquid and half air. But neither of view would helpful if there is a no answer to question of interest. Product team may never feel the need to take care of technical debt. If sufficient users are able use the system, what's the problem? If system is able to convert search to book ration in e-commerce site, where is the debt? If with every release, financial targets are being met, technical things should be good. Right? For most of such bird eye view questions, there is never a Yes - No answer.

If we peek down over the system to the last point, we might get "Yes" and a "No" at different parts of system. But as explained in previous blog<insert previous blog> 'Product and Technical Debts' are different view. An addition to technical part is `Design/Architecture Debt` or `Code Debt` or any part you may break it. Code mess is not debt, it is a loss. And there are various development practices to take care of it. Following team guidelines, using Linter, code reviews, design reviews and other mediums to tackle this mess. The decision to make a mess is never rational, is always based on laziness and unprofessionalism, and has no chance of paying of in the future. A mess is always a loss.

A thumb rule used by me to differentiate between the two is, mess is result of laziness and unprofessionalism or lack of desire to make it look correct to oneself. While reasons for technical debt may differ (discussed below)

While is is pretty easy and tempting to go back and forth and argue as to what does or does not constitute a ‘technical debt’ based on how it came about, I think it might be helpful to look at things slightly differently and not get so hung up on the intentions when you took out the loan etc.

### Examples for real life

### Why do technical debt occur?

In a way it is the engineering trade-off’s that software developers and business stakeholders must often make in order to meet schedules and customer expectations. Technical debt decisions are made based on real project constraints. They are risky, but they can be beneficial.

#### Time Constraint

#### Resource Issue

#### Lack of knowledge

#### Customer requirment

### Developer To-do list

#### Pro-active raising the red flag

    declaring bankruptcy on legacy code

#### Reactively bringing debt to notice

https://en.wikipedia.org/wiki/Zeigarnik_effect

#### Code for maintaining the debt

#### Huge documentation

####

### Detecting and paying the debt regularly

#### Regular health check up

Health Checkups are must and should help you in understanding the state of system .If developers are taking longer than normal to iterate features or if you notice your product’s functionality and performance have taken a dive, it certainly raises red flag.

#### Pay the minimum balance each week

A good way to clean up things is to work continously on something that you want to have eventually. Regularly fixing those small debts will help is getting rid of major chunks.

#### Cleanup team

Having a team of developers who keep check on such task helps in the process. Even in Agile, you need a cleanup sprint. These group should help in both detecting and fixing things.

#### Consider scalability/security/resilence in future decisions.

Anything that is implemented today should take account of future requirment, if all requirements cannot be predicted, certain aspect has to be kept open so that unexpected requirement aren't a problem and system scales well.

---

When you decide to take on a technical debt, you had better make sure that your code stays squeaky clean. Keeping the system clean is the only way you will pay down that debt.

---
