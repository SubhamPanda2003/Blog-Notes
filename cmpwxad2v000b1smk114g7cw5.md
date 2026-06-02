---
title: "Adaptive-Intent-Driven Development: Why I Stopped Writing Specs"
datePublished: 2026-06-02T17:39:56.986Z
cuid: cmpwxad2v000b1smk114g7cw5
slug: adaptive-intent-driven-development-why-i-stopped-writing-specs

---

# I built this because specs kept lying to me

> GitHub: [Adaptive-Intent-Driven-Development](https://github.com/SubhamPanda2003/Adaptive-Intent-Driven-Development)

---

There is a common belief that if we document our architecture well enough, the team will stay aligned. Its not true. Or rather — it is true at the moment we write it. But software is living. The document is not.

Six months into a project, a new engineer joined the team. She read our architecture doc, saw the line that said "Redis cache hit rate stays above 80%", and built her feature assuming that was still reality. What nobody had written down anywhere was that the cache dropped to 55% after a config change in month three. The doc wasn't wrong when we wrote it. It just stopped being true. And we had no system that noticed.

That is the problem I kept running into. Not that engineers write bad specs. But that specs are photographs pretending to be windows.

---

## Why the existing tools don't fully solve this

When Kiro came out everyone got excited. And honestly, it is impressive. You describe a feature, it writes a spec, breaks it into tasks, generates the code. For building new things from scratch it works really well. But here is what Kiro's model looks like underneath:

```
intent → spec → code → done
```

The pipeline flows one way. Once the code is generated, the spec is effectively a historical artifact. Kiro does not know that your cache hit rate dropped three months later. It cannot tell you "that assumption from sprint 2? production just violated it." The spec served its purpose — it was scaffolding, and now the building is up and the scaffolding came down.

SpecKit and OpenSpec are solving a different but related problem. They give you structured formats for API contracts, acceptance criteria, alignment between teams. Really useful at the start of a project. But a contract describes the world at the time of signing. What happens to the world after that is not the contract's concern.

We end up in the same place either way. A beautifully formatted spec and a system that quietly drifted away from it while everyone was focused on shipping. Nobody is wrong. Nobody is lazy. The tools just don't close the loop.

---

## What if intent was the primary artifact, not specs?

Think about how a spec is born. An engineer has a goal — reduce checkout latency. An assumption — the cache will hold. A constraint — must stay GDPR compliant. An architectural decision — use Redis, not the database. These are the things that actually matter. The spec is just our best attempt to write them down in a readable format.

What if instead of writing a document, we modeled those things directly? Goals, assumptions, decisions, constraints — each one as a node. Each relationship between them as an edge. And each node carrying a confidence score that can change over time.

That is AIDD — Adaptive Intent-Driven Development. The idea is simple. Intent is not a document you write once. Intent is something you hold, update, and query continuously.

---

## What it looks like in practice

You start by recording what you actually believe:

```bash
aidd intent add --type goal "Checkout latency below 2s at p95"
aidd intent add --type assumption "Redis cache hit rate stays above 80%" --confidence 0.7
aidd intent link <goal-id> <assumption-id> --type depends_on --causal
```

The `--confidence 0.7` is honest. I think this is probably true but I am not certain. The graph remembers that.

Six months later when the cache drops:

```bash
aidd intent confidence <assumption-id> 0.2 --reason "Hit rate at 55% post-deploy"
aidd graph blast-radius <assumption-id>
```

It tells you — three downstream goals affected, two architectural decisions at risk, severity critical. No ctrl+F through Confluence. No Slack thread asking who owns this. The graph knows what depends on what because we told it, and it remembered.

---

## The part that surprised me most

When I pointed AIDD at my own source code with `aidd scan ./src --map-to-graph`, it parsed all the TypeScript files, built the dependency graph, and pulled it into the intent graph. Circular dependencies became `conflicts_with` edges. Package dependencies became constraint nodes. Suddenly code structure and intent structure were in the same place. You can ask which business goals a module supports. That felt like magic when it first worked.

But the bigger surprise was what happened when I gave Claude the intent graph instead of a spec document. Usually when you paste a spec into an AI, it treats everything in it as equally true and equally important. There is no nuance about which parts are solid and which are guesses from six months ago.

With the intent graph, Claude sees the confidence scores. It sees which assumptions are marked low-confidence. It sees the causal chains. And it reasons differently. It flags the risky parts first. It cites intent IDs in its plan. It treats a 🔴 low-confidence assumption the way a senior engineer would — as a risk to call out, not a fact to build on.

```bash
aidd agent plan "Add distributed rate limiting to the auth service"
```

It is a different quality of answer. Like the difference between asking someone who just read the README versus someone who lived through the decisions with you.

No API key? It can still generate a prompt file you paste into Claude or Copilot:

```bash
aidd agent prompt "Add rate limiting to auth"
# → writes aidd-prompt.md
```

---

## The real insight

We built spec tools for a world where humans write, humans read, humans implement. AI changes that equation significantly. When an AI is writing large portions of your code, the most valuable thing you can give it is not a static document. It is structured, queryable, confidence-weighted context about why things are the way they are — and what it is allowed to break.

Specs describe what you decided. AIDD tracks what you believe and how confident you are. That is a small difference in framing but it changes everything about how you react when reality surprises you.

---

## Where it is right now

This is early work. Phases 1 through 3 are complete:

- [x] Intent Graph, event log, SQLite storage, full CLI
- [x] TypeScript and Python AST parser, dependency graph
- [x] Claude integration, context synthesis, AI planning

What I want to build next is the part that closes the loop properly — pulling in real runtime data. OTel traces, error rates, metrics. You record an assumption, production contradicts it, the system notices on its own. That is where the "adaptive" in AIDD stops being a promise and becomes real.

Also on the list: VS Code extension, multi-agent coordination, GitHub plugin to tie intent to PRs.

---

## Try it

If you have ever looked at an architecture doc and wondered whether it is still true — this is for you.

```bash
git clone https://github.com/SubhamPanda2003/Adaptive-Intent-Driven-Development.git
cd Adaptive-Intent-Driven-Development
npm install && npm run build
cd packages/cli && npm link
aidd init
```

Full walkthrough is in the README. Takes about five minutes to get running.

**[https://github.com/SubhamPanda2003/Adaptive-Intent-Driven-Development](https://github.com/SubhamPanda2003/Adaptive-Intent-Driven-Development)**

Would love to hear what you think — especially if you've tried Kiro or any spec tool and have opinions on where it fell short. I have been inside this problem long enough that I have probably lost perspective on what is obvious and what isn't.

Use intent-driven development to amplify your judgment, not to replace it.
