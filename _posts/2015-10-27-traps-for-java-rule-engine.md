---
layout: posts
title: Traps for Java Rule Engine
---

### Java Rule Engine - JSR-94
There is this bright, old idea of automating processes. We all do it in builds, tests, deployments, etc. (at least we should!). Would't that be great to automate development and business logic? To get rid of those stiff and non-flexible development cycles? And to let business people dynamically formulate business rules? After all we aim for being agile.
This idea has been already introduced with the [JSR-94](https://jcp.org/en/jsr/detail?id=94) which describes Java Rule Engine API. 

> "The Business Rules Group defines a business rule as a statement that defines or constrains some aspect of the business;
> a business rule is intended to assert business structure or to control or influence the business's behavior.
> A rule engine evaluates and executes rules, which are expressed as if-then statements.
> The power of business rules lies in their ability both to separate knowledge from its implementation logic and to be changed without changing source code."
- [Getting Started with Java Rule Engine API](http://www.oracle.com/technetwork/articles/javase/javarule-139829.html)

Many respected vendors implemented libraries and frameworks supporting this idea. But wasn't it too sloppy or mobbed by companies?

### Temptation
In the past we were faced with an old PHP application. Customer had problems maintaining it and enriching with new functionalities. Indeed, it was a poorly written code full of if-else conditions which looked like fast patches to new business requirements. Apart of extremely low [bus factor](https://en.wikipedia.org/wiki/Bus_factor), and lack of tests, there was both time and money given, so we accepted the challenge to re-engineer the whole thing.

In the first days, it turned out the customer himself did not know exactly their business. This is especially bad as whenever you ask for business matter's clarification you hear - "Do it as in the old system", in return. And you do not want to rewrite the old spaghetti code. As it is often the case, with significant money involved customer wants to have great flexibility as well. But not just the flexibility enabled by the architecture of the solution - the flexibility to configure _"the details"_ themselves. It is not a surprise the costs of developments are high so it is always better not to involve developers with every minor change. Since they are called software architects, they can make the overall system configurable to the [greatest extend](http://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf), can't they?

This all fits almost perfectly the idea of Business Rules Engine and we were very close to take the approach. Luckily, a small prototype proved it to be wrong and I am going to discuss it here.

### Getting it wrong
So what could have we ended up with? Well, we would have been using cool technology - `Drools`, `JRules`, or others, source code would have been simpler and customer closely involved into business logic creation, the logic would have been separated from its implementation and changed without changing the source code. Where is the problem?

If you follow the OOP principles you would already have had a feeling the business logic rules in a special meta language (`RuleML` perhaps) are all revealed to the public, freely versatile. This is in contradiction to the encapsulation, clean object responsibilities. Objects should have well-defined, separated accountability, highly-preferably immutable. Instead, here we have a common, public place where anyone can define and redefine universal behavior.

Very probable having business units maintaining the meta language's programmable rules of business logic does not make thing simpler. With all due respect, programmers (the good ones) have more experience, discipline and culture to organize language syntax, extract common variables, introduce abstractions, write documentation etc. You may get surprised but business perspective is often short-term, especially regarding tools, and their quality. It is the business that matters the most and changes should be applied asap. Eventually you may end up with one big ruleset with hundreds of rules in it. And from personal experience business likes the extremes - there will be only one person assigned for the maintenance (low bus factor) or every is allowed to add their two cents. Either way, it shortly turns into a mess.

Oh, and the rules' order matter which adds a bit of difficulty to add a new rule. Some may be in exclusive, in conflict with each other. This can lead to further rule separation with higher exposure of internal states and breakage of encapsulation. And I am not ever saying about the rule's change or removal. Due to the disproportionately small gain (readiness, quality, maintainability) to the risk they bring, they would be discarded at all cost endlessly making the ruleset more and more complex.

Covering your business rules with tests should assure instantly meeting the requirements while being exposed to unintended change. But the domain of changes is huge and so many cases are needed with not necessarily time to make them right. In this case isolated rules working are not that important as the entire knowledge base. It reminds me of an artificial neural network were one tries to control the inter-connections between the neurons in the different layers manually.
Do not forget it is not all about static rules configuration it is meant to provide dynamic and real-time changes. How would one do it with such high exposure to misconfiguration?

### Any chances?
Complete implementation of JSR-94 approach would have ended up with replacing the code full of if-else statements with special purpose Meta-language maintained by non-programmers still full of if-else statements. Where is the improvement then?

I am not claiming using the JSR-94 is impossible or impractical but I would love to hear about its outstanding adoption.

## Update
Few comments from [reddit](https://www.reddit.com/r/java/comments/3r49ya/traps_for_java_rule_engine/) on the subject.


