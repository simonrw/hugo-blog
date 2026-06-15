---
Title: The missing knowledge transfer in AI-written code
date: 2026-06-15
tags:
- ai
- software
- engineering
summary: AI-assisted coding changes what authorship means, because writing the code is no longer the same as understanding it.
---

For a long time, software teams have relied on a fairly simple model of understanding.

Someone makes a change. They read the code, think through the problem, try a few approaches, hit the awkward edges, and eventually arrive at a solution. By the time they open a pull request, they have built up a detailed mental model of what changed and why.

Then someone reviews it. The reviewer may understand the change deeply. They may only understand part of it. They may be skimming between meetings. But at least one person - the author - has usually paid the full cost of understanding.

That model is changing.

Increasingly, the flow looks more like this:

- A human prompts an AI to make a change.
- The AI edits the code.
- The human author checks that the output roughly matches the request.
- A reviewer looks at the diff, with varying levels of context.

The obvious concern is that the AI might produce bad code. That is real, but it is not the only problem. The quieter and more interesting problem is that teams can lose the knowledge that used to be created during the act of making the change.

When a person writes code by hand, understanding is a by-product of the work. You learn the shape of the module. You discover the naming conventions. You notice the strange edge case in the tests. You find out which abstraction is doing too much. Even if the final diff is small, the author has travelled through the surrounding territory.

When an AI writes the code, that journey can disappear.

The human may end up with a diff they can approve as plausible, without having gone through the reasoning that produced it. They may know what they asked for, and they may know what changed at a high level, but they may not know the local invariants, trade-offs, or failure modes. The pull request still has an author, but authorship no longer guarantees understanding.

That matters because code review has never been a complete substitute for authorship.

Reviewers are a second line of defence. They catch mistakes, ask questions, spot missing tests, and challenge design choices. But reviewers often rely on the author to have done the deep local reasoning already. A good review is a conversation with someone who understands the change from the inside.

If the author no longer has that understanding, review becomes weaker in a subtle way. The reviewer may ask, "Why did you do it this way?" and the real answer may be, "Because the AI produced it and it seemed to work." That is not a design rationale. It is an audit trail gap.

The risk is not that every AI-assisted change is bad. Many will be fine. The risk is that organizations start accumulating code that nobody has ever really understood. The AI generated it. The author accepted it. The reviewer skimmed it. The tests passed. And now it is part of the system.

This changes what good engineering practice needs to look like.

We should not treat AI-generated code as if it has passed through the same knowledge-building process as hand-written code. It has not. That does not mean rejecting it. It means adding the missing step back in.

The author still needs to become responsible for the change. Not just responsible in the administrative sense of having opened the pull request, but responsible in the engineering sense: able to explain the design, the edge cases, the tests, the risks, and the alternatives.

A useful standard might be: if you cannot explain the change without referring to the prompt, you are not ready to ask for review.

That has practical consequences.

Authors using AI should spend time reading the resulting diff as if it came from someone else. They should trace the execution path. They should ask why each file changed. They should make the tests fail and pass. They should remove code they do not understand. They should rewrite parts where necessary, not because hand-written code is morally superior, but because ownership requires comprehension.

Reviewers may also need to change their expectations. For AI-assisted changes, they should ask more explicitly for rationale. Why this approach? What alternatives were considered? What did the AI get wrong on the first attempt? Which parts did the human verify manually? These questions are not bureaucracy. They are a way of restoring the knowledge transfer that used to happen implicitly.

The real shift is that authorship is becoming less about who typed the code and more about who understands it.

That may turn out to be a healthy distinction. Plenty of code was poorly understood before AI. Plenty of human-written pull requests were cargo-culted, rushed, or copied from elsewhere. AI has not invented shallow understanding. It has made it easier to scale.

So the challenge is not to preserve the old workflow for its own sake. The challenge is to preserve the thing the old workflow often produced: a human who had wrestled with the change enough to own it.

If AI lets us produce code faster but leaves us with fewer people who understand the code, we have not simply gained productivity. We have converted understanding into throughput, and the bill will arrive later.

The teams that use AI well will not be the ones that blindly accept more diffs. They will be the ones that build new habits around comprehension, explanation, and ownership.

The question for every AI-assisted pull request should not be, "Did a human write this?"

It should be, "Does a human understand this?"
