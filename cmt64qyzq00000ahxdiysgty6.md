---
title: "Spec-Driven Development: When AI Writes Code, It Should Be Able to Tell You Why"
seoTitle: "Spec-Driven Development:  When AI Writes Code"
seoDescription: "Spec-Driven Development: When AI Writes Code, It Should Be Able to Tell You Why"
datePublished: 2026-08-23T18:17:51.803Z
cuid: cmt64qyzq00000ahxdiysgty6
slug: spec-driven-development-when-ai-writes-code-it-should-be-able-to-tell-you-why

---


AI coding agents are getting very good at writing software. But there is a problem that becomes more important as they become more autonomous:

**How do you know the code actually came from the requirements?**

Tests can tell us whether code works. They don't necessarily tell us whether the agent implemented something that was never requested.

That is the problem I explored in my paper, **“Citation Discipline in Spec-Driven Development.”** ([arXiv][1])

The idea behind **traceSDD** is simple: treat requirements like citations.

Instead of generating code without any connection to the specification, traceSDD asks the agent to attach a requirement identifier to the code it generates.

For example:

```text
[REQ-001.2]
def validate_email(email):
    ...
```

Now the relationship is explicit:

**Requirement → Code**

This creates something interesting. The citations aren't just documentation. They become something that can be checked automatically.

If the generated code references a requirement that doesn't exist in the specification, we have an **orphan requirement**.

That gives us a simple way to detect a certain class of AI hallucination: the agent implementing things that were never requested.

I tested this idea against other approaches to Spec-Driven Development, including GitHub Spec Kit and OpenSpec. The experiments showed an interesting trade-off.

Adding citations makes generated output somewhat less deterministic. In other words, two independent AI runs may produce code that looks more different.

But the same citations make the output **more verifiable**.

That distinction matters.

For traditional software development, we usually optimize for things like correctness, maintainability and consistency. With AI-generated software, we also need to ask:

**Can we prove why this code exists?**

That is where traceability becomes more than a documentation feature.

The implementation is available as an open-source project on GitHub: [TraceSDD](https://github.com/SubhamPanda2003/TraceSDD). It includes the workflow and tooling for applying requirement-level traceability during AI-assisted development.

[TraceSDD on GitHub](https://github.com/SubhamPanda2003/TraceSDD?utm_source=chatgpt.com)

The research paper goes deeper into the experiments, methodology and results:

[Read the paper on arXiv](https://arxiv.org/abs/2606.30689?utm_source=chatgpt.com)

I don't think citations are a silver bullet for AI-generated code.

But I do think we're going to need better answers to a basic question:

**“Why did the AI write this line of code?”**

Traceability is one possible answer.

[1]: https://arxiv.org/abs/2606.30689?utm_source=chatgpt.com "Citation Discipline in Spec-Driven Development: A Cross-Model Empirical Study of Output Determinism and Automated Hallucination Detection in LLM-Generated Code"
