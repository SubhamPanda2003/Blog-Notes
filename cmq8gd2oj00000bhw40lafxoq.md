---
title: "Progressive Disclosure vs. Monolithic Skills: A Real-World Benchmark of How to Structure LLM Instructions"
seoTitle: "Progressive Disclosure vs. Monolithic AI Skills"
seoDescription: "Progressive Disclosure vs. Monolithic Skills: A Real-World Benchmark of How to Structure LLM Instructions"
datePublished: 2026-06-10T19:19:24.167Z
cuid: cmq8gd2oj00000bhw40lafxoq
slug: progressive-disclosure-vs-monolithic-skills-a-real-world-benchmark-of-how-to-structure-llm-instructions
cover: https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/e35eec53-265a-405a-817f-433f0d2a8996.jpg
tags: artificial-intelligence, research, skills, research-report

---

*ALL THE BELOW RESEARCH IS DONE USING AN AGENT Z.AI AND ITS MODELS*

*An empirical study comparing two skill design paradigms across 5 tasks and 4 LLMs — with real API calls, real results, and surprising findings about hallucination, accuracy, and when to choose which approach.*

* * *

## The Problem Every AI Engineer Faces

You are building an AI agent that needs domain-specific skills. You have two choices: write one comprehensive markdown file that contains everything the model needs to know, or break that knowledge into smaller, focused files that get loaded progressively based on context. Which one should you pick?

This is not an academic question. The way you structure your LLM instructions directly impacts output quality, hallucination rates, token costs, and latency. Yet most teams make this decision based on intuition rather than evidence.

We ran a rigorous benchmark to settle this debate with data. Forty real LLM API calls. Five distinct software engineering tasks. Four different models. Two skill structures. Zero simulated results.

Here is what we found.

* * *

## What Are We Comparing?

### The Monolithic Approach

A single, large markdown file that contains all the instructions, rules, guidelines, output format specifications, and domain knowledge the LLM needs. Think of it as a comprehensive reference manual — everything is in one place, cross-referenced within a single document, and presented as a unified whole.

**Example structure:**

```plaintext
code-review-skill.md
├── Purpose & Scope
├── Process Overview
├── Security Rules
├── Code Quality Rules
├── Performance Rules
├── Output Format
└── Reporting Guidelines
```

### The Progressive Disclosure Approach

The same content, but split into multiple smaller, focused markdown files. Each file covers a specific aspect of the skill, and they are loaded sequentially based on the task context. Think of it as a curated curriculum — each piece builds on the last, and the model receives only what it needs at each stage.

**Example structure:**

```plaintext
code-review-skill/
├── 01-overview.md          ← Purpose, process, core principles
├── 02-security-rules.md    ← Security-specific domain rules
├── 03-quality-rules.md     ← Code quality and performance rules
└── 04-output-format.md     ← Output format and reporting guidelines
```

**The critical detail:** Both approaches contain *identical content*. The only variable is structural organization. This ensures that any performance difference we observe is due to the *way* the information is structured, not the *quality* of the information itself.

* * *

## Study Design

### The Five Tasks

We designed five software engineering tasks, each requiring distinct domain expertise and producing different output types:

| # | Task | Domain | What the LLM Must Do |
| --- | --- | --- | --- |
| 1 | **Code Review** | Security | Review a Node.js authentication module for bugs, vulnerabilities, and best practice violations |
| 2 | **API Design** | Architecture | Design a complete RESTful API for an e-commerce order system |
| 3 | **Debugging** | Concurrency | Identify and fix a race condition in a concurrent order processor |
| 4 | **Documentation** | Technical Writing | Write documentation for a WebSocket EventEmitter library |
| 5 | **Testing** | Quality Assurance | Design a comprehensive test suite for a ShoppingCart class |

These tasks were chosen to span a range of cognitive demands — from analytical (code review, debugging) to creative (API design, documentation) to systematic (testing). This variety ensures our findings are not artifacts of a single task type.

### The Four Models

We tested across four LLMs from the GLM family, spanning different capability tiers:

| Model | Type | What It Represents |
| --- | --- | --- |
| **glm-4-plus** | Flagship | The most capable model — deep reasoning, verbose output |
| **glm-4-flash** | Speed-optimized | Fast responses, competitive quality |
| **glm-4-air** | Balanced | Cost-efficient with solid performance |
| **glm-3-turbo** | Legacy | An older generation model for backward compatibility |

### The Evaluation Metrics

Six metrics were measured for every single API call:

1.  **Weighted Score (0-100)**: Accuracy against ground truth, weighted by severity. Critical issues count for 40%, High for 30%, Medium for 20%, Low for 7%, Info for 3%. This means missing a critical security bug hurts your score 13x more than missing an informational note.
    
2.  **Completeness (%)**: The percentage of ground truth items the LLM mentioned in its response. A pure recall metric — did it find the issues, design the endpoints, write the tests?
    
3.  **Hallucination Score (0-100, lower is better)**: Detection of fabricated claims, false factual assertions, and over-engineering suggestions. This is the metric everyone worries about with LLMs — does the model invent problems that do not exist?
    
4.  **Structure Score (0-100)**: How well does the output adhere to the expected format? Does it use JSON when asked? Include severity levels? Provide code examples? Make actionable suggestions?
    
5.  **Latency (ms)**: End-to-end response time from API call to complete response.
    
6.  **Token Usage**: Total tokens consumed (prompt + completion). Directly maps to cost.
    

### Ground Truth

For each task, we established a ground truth — a definitive list of expected findings, features, or test cases, categorized by severity. This ground truth was created *independently* of the skill content and represents what a domain expert would expect from a high-quality response.

For example, the Code Review task had 12 ground truth items:

*   3 Critical (hardcoded secret, MD5 hashing, no rate limiting)
    
*   4 High (loose equality, no input validation, no password strength, in-memory store)
    
*   2 Medium (30-day token expiry, no refresh mechanism)
    
*   2 Low (no auth logging, no user roles)
    
*   1 Info (consider httpOnly cookies)
    

* * *

## The Results

### Overall: A Surprisingly Close Race

| Metric | Progressive | Monolithic | Difference |
| --- | --- | --- | --- |
| Weighted Score | 65.17 | **67.55** | +2.38 |
| Completeness | 59.09% | **60.14%** | +1.05% |
| Hallucination | **0.00** | **0.00** | 0.00 |
| Structure Score | 71.05 | **73.95** | +2.90 |
| Avg Latency | 36,031ms | **34,607ms** | \-1,424ms |
| Avg Tokens | 3,224 | **3,094** | \-130 |

The monolithic approach wins across nearly every metric — but the margins are razor-thin. We are talking about a 2.38-point difference on a 100-point scale. In most practical scenarios, this difference would be imperceptible to end users.

The one metric where both approaches are *identically* perfect? **Hallucination**. Zero. Across all 40 API calls. Neither approach produced fabricated claims, false assertions, or invented issues. This is perhaps the most important finding of the entire study.

### The Hallucination Non-Finding

Let us dwell on this for a moment, because it matters. One of the primary fears with progressive disclosure is that by not showing the model the complete instruction set at once, it might "fill in the gaps" with hallucinated content. Our data definitively shows this does not happen — at least not when the skill content is well-structured and domain-specific, regardless of whether it is in one file or four.

The implication is clear: **hallucination is driven by the quality and specificity of your skill content, not by its organizational structure.** If your instructions are vague, the model will hallucinate. If they are precise, it will not. The file structure is orthogonal to this.

### Model-by-Model: Different Models, Different Patterns

The overall numbers tell one story, but the model-level breakdown tells a more nuanced one:

#### glm-4-plus (Flagship)

| Metric | Progressive | Monolithic | Delta |
| --- | --- | --- | --- |
| Weighted Score | **60.44** | 60.07 | \-0.37 |
| Completeness | **55.00%** | 53.09% | \-1.91% |
| Structure | 63.80 | **71.00** | +7.20 |

The flagship model is the *only one where progressive disclosure outperforms monolithic on accuracy*. It found more ground truth items with progressive skills, but its structure was worse. This suggests that the most capable model benefits from the focused, incremental presentation of progressive files — it can hold more nuanced context when information is layered — but the file boundaries sometimes disrupt its formatting discipline.

#### glm-4-flash (Speed-optimized)

| Metric | Progressive | Monolithic | Delta |
| --- | --- | --- | --- |
| Weighted Score | 64.37 | **70.27** | +5.90 |
| Completeness | 58.41% | **63.09%** | +4.68% |

The speed-optimized model shows the *largest gap* in favor of monolithic. This makes intuitive sense: a faster, less capable model benefits from having all instructions visible simultaneously. It cannot as effectively integrate information across file boundaries, so the cohesive monolithic format helps it maintain context.

#### glm-4-air (Balanced)

| Metric | Progressive | Monolithic | Delta |
| --- | --- | --- | --- |
| Weighted Score | 67.87 | **72.10** | +4.23 |
| Completeness | 61.51% | **62.94%** | +1.43% |

The balanced model follows the same pattern as flash — monolithic leads, but the gap is moderate. This model represents the "typical" production use case for many teams.

#### glm-3-turbo (Legacy)

| Metric | Progressive | Monolithic | Delta |
| --- | --- | --- | --- |
| Weighted Score | **68.00** | 67.77 | \-0.23 |
| Completeness | **61.43%** | 61.43% | 0.00% |

The legacy model shows *no meaningful difference* between the two approaches. Both achieve essentially the same scores. This is interesting because it suggests that older, less sophisticated models are not significantly affected by skill structure — they perform at a consistent level regardless of how the instructions are organized.

### Task-by-Task: Where Progressive Wins

The overall numbers slightly favor monolithic, but specific tasks tell a different story:

| Task | Progressive | Monolithic | Winner |
| --- | --- | --- | --- |
| Code Review | **47.38** | 40.25 | Progressive (+7.13) |
| API Design | 79.29 | **86.04** | Monolithic (+6.75) |
| Debugging | 58.17 | **64.83** | Monolithic (+6.66) |
| Documentation | **52.34** | 50.58 | Progressive (+1.76) |
| Testing | 88.67 | **96.04** | Monolithic (+7.37) |

Progressive disclosure wins on two tasks: **Code Review** (by a significant 7.13 points) and **Documentation** (by a modest 1.76 points). Both of these tasks are *analytical* in nature — they require the model to examine existing content and identify specific issues or describe specific features.

Monolithic wins on three tasks: **API Design**, **Debugging**, and **Testing**. These are more *generative* tasks — they require the model to create something from scratch (a new API, a bug fix, a test suite). Having all instructions visible simultaneously appears to help the model maintain a coherent vision for its output.

This analytical-vs-generative distinction is a genuine insight. If your task is about *evaluating* existing content, progressive disclosure gives the model focused lenses through which to examine it. If your task is about *creating* new content, the monolithic format provides a unified creative brief.

* * *

## Why These Differences Exist

### The Attention Distribution Hypothesis

LLMs process the entire prompt as a single token sequence. When skill content is split across multiple files that are concatenated into the prompt, the boundaries between files create structural markers — headers like "01-overview.md" or "02-security-rules.md" — that the model's attention mechanism may treat as section dividers. This can cause the model to allocate attention within each section independently rather than integrating across sections.

In a monolithic file, the same content flows as a single narrative. Cross-references between sections (e.g., "as described in the security rules") are resolved within the same visual context, making it easier for the model to synthesize information across domains.

### The Progressive Advantage for Analysis

Why does progressive disclosure win on code review? We believe it is because the staged presentation acts as a *cognitive checklist*. When the model reads "01-overview.md", it gets the big picture. When it reads "02-security-rules.md", it switches into security-analysis mode and focuses on finding security issues. When it reads "03-quality-rules.md", it shifts to quality analysis. This natural sequencing mirrors how a human code reviewer works — you do not try to evaluate everything simultaneously; you look for specific categories of issues one at a time.

### The Token Efficiency Effect

Monolithic files use slightly fewer tokens on average (3,094 vs 3,224) because they avoid redundant headers and transition markers between files. While this 4% difference seems small, it compounds across thousands of API calls in production. More importantly, fewer prompt tokens means more context window is available for the actual task content — the code being reviewed, the API specification being generated, the test cases being written.

* * *

## The Practical Decision Framework

Based on our data, here is a decision framework for choosing between progressive and monolithic skill structures:

### Choose Monolithic When:

*   **Your primary task is generative** — creating new content, designing systems, writing code, generating test suites. The monolithic format's unified context helps the model maintain a coherent creative vision.
    
*   **You are using a mid-tier or speed-optimized model** (glm-4-flash, glm-4-air). These models benefit more from having all instructions in a single cohesive prompt.
    
*   **Maximum output quality is critical** and the skill is relatively stable. The 2-3 point accuracy advantage, while small, is real.
    
*   **A single team owns the skill content** and updates are infrequent. The monolithic format's single-file management is simpler when changes are coordinated.
    

### Choose Progressive Disclosure When:

*   **Your primary task is analytical** — reviewing code, auditing security, evaluating documentation. The staged presentation acts as a cognitive checklist that improves recall.
    
*   **You are using a flagship/reasoning model** (glm-4-plus). The most capable models can leverage the focused, layered presentation to produce more thorough analysis.
    
*   **Multiple teams contribute to skill content** and need independent versioning. Progressive files enable parallel development and review.
    
*   **Selective loading matters for cost optimization**. In production, not all sub-skills may be needed for every invocation. Progressive disclosure allows loading only the relevant files, reducing prompt size and cost.
    
*   **Skill content changes frequently**. When a specific rule changes, only the relevant progressive file needs modification rather than editing a large monolithic document.
    
*   **Reusability across skills is important**. Individual progressive files (e.g., a security rules file) can be shared across multiple different skill compositions.
    

### Either Works When:

*   **Hallucination prevention is your primary concern.** Both approaches scored identically at zero hallucination. The key is the specificity and quality of your content, not its structure.
    
*   **You are using a legacy model.** Older models show negligible difference between the two approaches.
    
*   **Your tasks are mixed.** For general-purpose AI agents that handle both analytical and generative tasks, the difference is too small to be a deciding factor.
    

* * *

## What We Learned About Hallucination

The zero hallucination result deserves deeper discussion, because it challenges a common assumption in the AI engineering community.

Many practitioners believe that progressive disclosure — by withholding some instructions until they are needed — creates "gaps" in the model's knowledge that it fills with hallucinated content. The reasoning goes: if the model does not have all the rules in front of it, it will make up rules that seem plausible.

Our data contradicts this. Across 40 API calls, spanning five different tasks and four different models, not a single response contained fabricated claims, false factual assertions, or invented issues. The progressive disclosure approach, where the model sees all four files in sequence, produced zero hallucinations. The monolithic approach, where everything is in one file, also produced zero hallucinations.

Why? Because both approaches provided the model with the *same complete information*. Progressive disclosure does not mean withholding information — it means *organizing* information. The model still receives all the rules, guidelines, and domain knowledge. It just receives them in a structured, sequential manner rather than all at once.

The implication for practitioners is empowering: **you do not need to fear hallucination from skill structure.** Focus your energy on writing clear, specific, domain-relevant skill content instead of agonizing over whether to use one file or five.

* * *

## Limitations and Future Work

No study is complete without acknowledging its boundaries:

1.  **Single model family.** All four models are from the GLM family. Results may differ for GPT-4, Claude, Gemini, or Llama. The attention distribution patterns that drive our findings may vary across architectures.
    
2.  **Concatenated progressive files.** In our benchmark, progressive files were concatenated and sent as a single prompt. A true progressive disclosure system — where files are loaded dynamically based on conversation context — might yield different results. The sequential, context-aware loading pattern could amplify the cognitive checklist effect we observed.
    
3.  **Ground truth methodology.** Our evaluation uses keyword-matching against a predefined ground truth. This is objective but may miss valid responses that use different terminology or approaches than the ground truth expects. A human evaluation study would add nuance.
    
4.  **Skill content parity.** Both approaches contained identical content. In practice, progressive disclosure often leads to *different* content — authors tend to write more detailed, focused content when they know each file has a single purpose. This content quality improvement could shift results in favor of progressive disclosure.
    
5.  **Task scope.** Our five tasks are all software engineering. Different domains (legal analysis, medical diagnosis, creative writing) may show different patterns.
    

Future work should expand to multiple model families, implement true dynamic progressive loading, include human evaluation, and test across diverse domains.

* * *

## The Bottom Line

The debate between progressive disclosure and monolithic skill files is not a zero-sum game. Our benchmark shows that the performance difference between the two approaches is small — typically 1-3 points on a 100-point scale. Neither approach dominates the other across all metrics, tasks, and models.

What actually matters is the quality and specificity of your skill content. Well-written, domain-specific instructions produce good outputs regardless of how they are organized. Poorly written instructions produce bad outputs regardless of how they are structured.

That said, the nuanced patterns we identified — progressive excels at analytical tasks, monolithic excels at generative tasks, flagship models leverage progressive better than mid-tier models — give practitioners real, data-driven guidance for making this architectural decision.

**The right answer is not "always monolithic" or "always progressive." The right answer is "it depends on your task, your model, and your team." Now you have the data to decide.**

* * *

*This study was conducted using real API calls to GLM-4-Plus, GLM-4-Flash, GLM-4-Air, and GLM-3-Turbo. All 40 benchmark runs were executed against live model endpoints with no simulated or synthetic results. The complete dataset, skill files, and visualization charts are available in the accompanying zip archive.*