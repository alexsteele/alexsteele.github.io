---
layout: post
title:  "Memory v Compute"
permalink: /:title/
date:   2026-03-08 00:00:00 -0800
---

How do we think about the tradeoff between memory and computation in LLMs?

We can think about a model as allocating information across three mediums.

```
knowledge = weights + context + tools
```

The cost is:

```
cost = weight memory + inference + tools + errors
```

Suppose a task `x` requires information `z` to produce answer `y`.

We can recover `z` in several ways:
1. weights `W`
1. context `C`
1. tools `T`

The model then approximates 

```
p(y | x,W,C,T)
```

Where do we encode `z`?  An optimization objective is:

$min_{x,y} E[loss(y, \hat{y}) + cost(weights) + cost(context) + cost(tools)]$

A key training goal is

>  put z in weights if cost(training + serving) < cost(runtime) * uses

And a useful decomposition is:

```
cost(weights) = cost(capacity) + cost(train) + cost(serve)
```

The research commmunity as a whole pareto-searches the frontier of systems
optimizing model sizes, context budgets, and tool budgets. Different tasks
assign different costs to each component. There is no one-size-fits-all. Bigger
models memorize more, smaller models compute more. Fewer tool calls require more
weights and context, while more tool calls reduce these.

For an individual task we explore
* weights only
* weights + retrieval
* weights + tools
* weights + retrieval + tools

across model sizes and token budgets.

And overall we seek to maximize marginal quality over marginal cost.

$\frac{\Delta quality}{\Delta cost}$
