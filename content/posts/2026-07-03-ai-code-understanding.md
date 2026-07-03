---
Title: The missing knowledge transfer in AI-written code
date: 2026-07-03
tags:
- ai
- software
- engineering
summary: The atrophy of product and implementation understanding
---

When developing features before AI, there was usually one person who thought deeply: the feature set, the implementation implications and what the feature *actually was*, not just what was *planned.* In this new AI world, that deep knowledge of what was **actually built** has gone.

Before AI the best case software development workflow was:

1. author thinks about the problem deeply, understanding it on a fundamental level;
2. author writes code;
3. author self-reviews code when submitting for review;
4. reviewer skims change, but is not expected to have the same mental model in their head: they check for errors, maybe test it, then hit the LGTM button;
5. change lands.

Now the flow is:

1. author prompts agent;
2. author skims results or worse sees tests written, make assumptions about code quality, and submits for review;
3. the reviewer is expected to review 10k lines of changed code which is unreasonable, and so they understandably smash the LGTM button.

We have lost the detailed understanding phase where *someone* intimately knows what the code does, and more importantly: what functionality has been created. It used to be the author who wrote the change. Through the process of writing the code they kept a mental model of the feature, chose tests that would define the *behaviour* of the feature, and constructed the implementation.

There is now a **lack of knowledge of what the feature does** and how well the tests cover that functionality. I have experienced this myself. We now rely on up front definition and prompting to capture the entire feature, plus perhaps some minor manual testing to confirm that the change is a good one.

> [!NOTIFY]
> System notification: Your password will expire in 30 days.

Ask yourself: do you really **know** that you covered the edge cases with your last vibecoded 10k changed LOC PR?

Maybe this is a skill issue on my part. I can certainly be more mindful of what the LLM writes in the first place. But I have always been a “doing” learner. I have written out thousands of quick start guides or tutorials instead of copying and pasting so that it sticks in my brain. I feel I am losing that with LLMs. 

I don't want to give up on pace either. The first time I prompted an agent to work on a hobby project was a rush. I could code on my phone while out of the house!

One aspect I think LLMs excel at is talking through problems and open ended research. I've learnt a lot from asking vague questions into ChatGPT, treating it as a “better google”. 

I also think LLMs that can **act** (ie agents) have their place. And they unlock a huge amount of power, e.g. performing web searches, searching code bases, running tests etc.  It is making me think that the way forward is an agent for a tiny well scoped change, and iterate slowly. Letting the agent do the heavy lifting as we go along. I gain the benefit of 

- correctness (the agent can run tests),
- understanding (I am reviewing the changes as they happen, not at the end with a big pr), and
- velocity (the agent can type quicker and better than I can).

Try using agents for tight well scoped work with clear outcomes, and review the outputs at each stage. Ensure you remain familiar with what you are building at a feature level, and I think you will create more maintainable applications in the long run.
