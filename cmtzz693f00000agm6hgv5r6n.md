---
title: "Skill Lift: The metric for effectiveness"
seoTitle: "Skill Lift: The metric for effectiveness"
seoDescription: "Skill Lift: The metric for effectiveness"
datePublished: 2026-09-13T15:34:52.314Z
cuid: cmtzz693f00000agm6hgv5r6n
slug: skill-lift-the-metric-for-effectiveness
tags: skills, llm, ai-agent, skill-lift, skill-eval

---

By now we are all convinced that [skill.md](http://skill.md) is a booster for an Agent's performance. And people all over the world are creating skills left, right and center.

Without collaboration, there will be duplication of efforts and no real gain in creating masterpiece skill that will completely solve a particular task or problem. And to collaborate, we need guardrails. We need to make sure that someone's change doesn't effect the other. Or in simple terms we need test cases for skills that would exactly behave like Junit tests or Integration tests.

The specification for writing skill test is clearly depicted by Anthropic in [agentskills.io](http://agentskills.io) , but two of the things that are missing is how effective are your tests and skills and what is the test coverage. Are all the behavioral constraints touched or executed by the agent while running the tests?

In this article, we will focus on the first point. Is my skill tests effective enough and Is my skill effective enough.  
The most important question is how do we define effectiveness here. When would we call a test as effective. Lets compare this with our old java unit tests. If there a function lets say f that takes input as x and returns y as output. Now if x is 10 then y should be 20. Now my test should fail is the input is 10 and output is not 20.  
This is exactly what is handled using assertions in [agentskills.io](http://agentskills.io) 's evaluation specification. This solves the first part, the test effectiveness.

But the second question: is the skill effective. When is a skill effective? When it makes the agent do things that is not there in its own training data. Now that makes it simple right.  
Run the test with skill. And then run the tests without skills.  
Ideally all the tests should pass with skill and fail without skill. But practically that does not happen.  
Now we have our skill lift.  
<mark class="bg-yellow-200 dark:bg-yellow-500/30">Skill Lift = % test passes with skill - % test passes without skill.</mark>

  
Now a low skill lift doesn't always mean that the skill is not effective. The corresponding tests might not be effective as well. We might be writing tests that just tests the generic knowledge that's already available instead of the specific proprietary knowledge that the skill carries.  
So a low skill lift can mean two things: either the skill is ineffective or the tests are ineffective or both. <mark class="bg-yellow-200 dark:bg-yellow-500/30">But for sure there is a problem with one of them.</mark>

To debug the issue: the easiest way would be to see the tests first. Are all the test focusing on one behavior of the skill. Is there any constraints that is getting missed.  
If all the behavioral constraints of the skill has test and proper assertion.  
Then its a bigger and more serious problem: the skill is ineffective and doesn't add any extra value on the top of the training data that the LLM/agent already has.  
  
Thanks for reading till the end.  
If you want more s content subscribe to me newsletter: [Subscribe](https://subham-panda-newsletter.beehiiv.com/)