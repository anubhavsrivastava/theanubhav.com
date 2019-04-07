---
layout: post
title: Coping up with technical debts as a developer!
subtitle: Understanding developer's take on technical debt and things around it.
avatar: /img/avatars/avatar-code.png
gist: >-
    Technical debt may be necessary in products, but how one keep a check on it
    and happily track it.
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

Read the first part - [Don't delay your technical debts](/2019/03/10/dont-delay-technical-debts/)

Following article would explain most of scenarios with regards to frontend/web development but simply can be applied as in general software development.

### Table of contents

<!-- toc -->

-   [First, explanation to “if it’s not broke, don’t fix it” disease](#first-explanation-to-if-its-not-broke-dont-fix-it-disease)
-   [Understanding the view of debt and making it purely technical](#understanding-the-view-of-debt-and-making-it-purely-technical)
-   [Developer To-do list](#developer-to-do-list)
    -   [Life choices](#life-choices)
    -   [Pro-active raising the red flag](#pro-active-raising-the-red-flag)
    -   [Reactively bringing debt to notice](#reactively-bringing-debt-to-notice)
    -   [Huge documentation](#huge-documentation)
    -   [Communication](#communication)
    -   [Implementation](#implementation)
    -   [Tracking practices](#tracking-practices)
-   [Detecting and paying the debt regularly](#detecting-and-paying-the-debt-regularly)
    -   [Regular health check up](#regular-health-check-up)
    -   [Pay the minimum balance each week](#pay-the-minimum-balance-each-week)
    -   [Cleanup team](#cleanup-team)
    -   [Consider scalability/security/resilence in future decisions.](#consider-scalabilitysecurityresilence-in-future-decisions)

<!-- tocstop -->

### First, explanation to “if it’s not broke, don’t fix it” disease

“if it’s not broke, don’t fix it” paradigm is not a problem as such, but if that becomes a excuse/reason for all wrong things that are visible in a product/technical every now and then, that becomes a disease. For instance, Instagram nearly fell prey to these growing disease early on. When it launched its iPhone app in October 2010, it ran its operation off of a single server in Los Angeles. It worked without being "broken" for long time. But after an onslaught of traffic nearly crashed the server, Instagram pivoted in three days to an EC2-hosted database. Co-founder Mike Krieger compared the transfer to open-heart surgery, and he now works to preemptively address technical debt before it leads to catastrophe.

### Understanding the view of debt and making it purely technical

Like a glass that is half filled with liquid, someone would call it half empty. Both the 'view' of situation is correct. Someone would even extend it to say glass is full with half liquid and half air. But neither of view would helpful if there is a no answer to question of interest. Product team may never feel the need to take care of technical debt. If sufficient users are able use the system, what's the problem? If system is able to convert search to book ration in e-commerce site, where is the debt? If with every release, financial targets are being met, technical things should be good. Right? For most of such bird eye view questions, there is never a Yes - No answer.

If we peek down over the system to the last point, we might get "Yes" and a "No" at different parts of system. But as explained in [previous blog](/2019/03/10/dont-delay-technical-debts/) 'Product and Technical Debts' are different view. An addition to technical part is `Design/Architecture Debt` or `Code Debt` or any part you may break it. Code mess is not debt, it is a loss. And there are various development practices to take care of it. Following team guidelines, using Linter, code reviews, design reviews and other mediums to tackle this mess. The decision to make a mess is never rational, is always based on laziness and unprofessionalism, and has no chance of paying of in the future. A mess is always a loss.

A thumb rule used by me to differentiate between the two is, mess is result of laziness and unprofessionalism or lack of desire to make it look correct to oneself. In a way tech debt is the engineering trade-off’s that software developers and business stakeholders must often make in order to meet schedules and customer expectations. Technical debt decisions are made based on real project constraints. They are risky, but they can be beneficial.

While is is pretty easy and tempting to go back and forth and argue as to what does or does not constitute a ‘technical debt’ based on how it came about, I think it might be helpful to look at things slightly differently and not get so hung up on the intentions when you took out the loan etc.

### Developer To-do list

Some Development Teams fall victim to the way that ‘the business’ keeps prioritizing new features over improving the codebase, while on the other hand holding them responsible for bugs, broken code and the results of technical debt. The Development Team can do no right in that case. But a clear communication to stakeholder will certainly help. Here are list of things one can do as a dev,

#### Life choices

As a software developer, you have to add a new functionality to a software or system. There’s two paths to choose:

The easier route – made up of messier code or design, will get you there faster.
The harder route – made up of cleaner code and design that takes a lot more time.

Choose a path such that after sometime, result is same as one coming from harder route. You may choose easier one for now, but eventually make sure that it is transformed into the harder route, by making it planned technical debt.

#### Use Code Metrics to quantify Technical Debt

Metrics offer a wonderful opportunity to make something subjective and abstract more objective and tangible. It also gives you a measurable goal to improve towards.
Cyclomatic complexity, Code coverage, Continous Integration, Cost of Delay,
Bugcount, etc. Make sure you set up things so that you can measure them and also analyse them on timely basis.

#### Pro-active raising the red flag

Below are some warning signs that a project has created technical debt which a developer can better understand,

-   Code smells that impact overall performance quality than cause a crash.
-   Higher levels of complexity — when technologies overlap each other.
-   Product bugs that will cause an entire system crash.
-   Issues with coding style.

At each detection, one should raise a red flag, declare bankruptcy on code and plan it out. Not everything would be do-able at once. Once should, document that in code itself via `TODO`, `FIXME` comment. Opening a ticket in tracking system to track for future. Add it to Product Backlog.
Asking and Developing a coding style guide and sticking to it, will help to overcome few of above issue.

#### Reactively bringing debt to notice and communicating

One may not have idea beforehand about a thing, but over a period of time, one might understand a better way to do same thing. Thus sensing some debts. In such cases, developer should declare EOL for such part/piece. It would be better to discuss things with team and propose the solution to help future selves. As a team,
one of the most important steps to take in managing technical debt is to acknowledge that it exists in the first place and share that discovery with key stakeholders. It should be the responsibility of technical management to set the tone and communicate to non-technical managers about the true cost of technical debt. The head must also explain the importance of paying down technical debt sooner rather later.

#### Continous documentation

Always rely on documentation, use it every where. As the saying goes, "if it is not documented, if was never done". Starting with small documentation of say, function also helps. Going furthur with entire design and thought process without documentation will certainly not help. A good developer always documents, whatever he can, not for others but for himself.

#### Implementation considerations

There are three options to consider in terms of managing technical debt during development.

-   Waive the requirement altogether. In other words, let the non technical people know about the trade off so that the organization decides to live with the system as it is and no longer deems the requirement as necessary. If you cannot waive the requirement, then you will need to refactor or replace the application.
-   Refactor the application. This option is aimed at reducing complexity, removing duplicates and improving the structure of the code. Refactoring is the only way to improve a code’s internal structure without changing the behavior of the program.
-   Replace the application. While this will introduce new technical debt, the idea is to address it quickly and minimize it as much possible. This is at most cases, beyond control of developer.

#### Tracking practices

As the debts can survive multiple development cycles, tracking it is crucial. Here’s quick list to follow,

-   Maintain list of technical debts. (This includes all instances where the developers know the code isn’t as clean as it should or needs to be for future development.)
-   Segregate and group deferred tasks into workable chunks.
-   Note the consequences of ignoring each chunk.
-   Keep the list transparent and visible.
-   Inform teams that rely on delivery releases

Schedule regular and frequent time to pay off technical debt.

### Detecting and paying the debt regularly as a team

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
