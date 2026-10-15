---
title: "AI Skill Test Coverage: Can we create line coverage for AI skills Eval?"
seoTitle: "AI Skill Test Coverage: Can we create line coverage"
seoDescription: "AI Skill Test Coverage: Can we create line coverage for AI skills Eval?"
datePublished: 2026-10-15T19:20:41.997Z
cuid: cmv9xbxfu000002oxdvd20pjd
slug: ai-skill-test-coverage-can-we-create-line-coverage-for-ai-skills-eval
tags: ai, skills, ai-skills, anthropic, spec-driven-development, anthropic-skills

---

Before getting deep into the above topic, we need to understand more on the skill lift. [Attaching a link to the article on skill lift](https://blog.subhampanda.com/skill-lift-the-metric-for-effectiveness). Simple in one line, there might be n tests, run those with skill - run those without the skill. Now this gives you the effectivity of the skill.

For each line of the skill should be tagged to a test/eval. Multiple line can be tagged to a single test.

The structure can be written as:  
Skill eval {  
test: .......  
skill used: \[{0,10},{15,28},{110,128}\]

}

The part of the skill used should be tagged to the eval. Now come the interesting part, we will use skill lift but this time not deleting the whole skill but just removing the part skill used. The main point is the test should fail without those lines. This guarantees that the skill is getting used.  
All these removals can be done by a python script.  
And the summation of all the characters used in "skill used" across all the skill tests will give us the total percentage of the skill used. That is our line coverage of the skill. Or better it is the character coverage of our skill.  
  
I did some testing and experiment and converted it to research paper.

**Read the paper:** [**Layered Skill Coverage on Zenodo**](https://zenodo.org/records/23196290)