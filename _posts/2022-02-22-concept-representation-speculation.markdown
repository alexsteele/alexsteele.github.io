---
layout: post
title:  "Concept Representation Speculation"
permalink: /:title/
date:   2022-02-22 00:00:00 -0800
---

_How will computers represent knowledge in the future? I speculate._

One thing that intrigues me about today’s ML models is that most of their knowledge is internal. It’s stored as weights within the model. These weights are typically represented as matrices of numbers, all carefully laid out according to the model architecture.

But imaging more intelligent systems of the future, my guess is that this will change. A lot of knowledge will move outside the model. And not just simple, discrete facts. High dimensional, mushier data that we might call concepts[^1]. Reasoning tasks will still be performed by big models with specialized knowledge. But these general concept representations will be an important input.

This would have some obvious advantages. Models could share what they learn, instead of each having distinct representations. They also won’t need to be as big[^2], since they can only fetch the knowledge they need when they need it.

What will these concepts look like? It seems like a fairly sure bet that high dimensional vectors will be an important representation. Something like the word or document embeddings we have today. But my intuition is that this won’t be enough. You might be able to capture a compressed representation of Hamlet in a document vector. But can you capture the conceptual model that created Hamlet with a set of vectors in a vector space?

Maybe. The hard part is not just the individual facts, but how they’re all related. And concepts exist at many different levels, related to each other in many different ways. Indeed, humans seem able to refine, relate, combine, and divide concepts ad infinitum. So I’m somewhat skeptical that a single vector space model could capture concepts in these ways.

My guess is we'll need some sort of explicit knowledge graph to capture useful relations between concepts. But where traditional knowledge graphs are built around discrete facts, future concept graphs will be built on learned ideas.

No matter the base representation, though, you need to be able to find the right concepts to solve a given problem, so indexing will also be important. Vector indexes for nearest-neighbor type search, but also more traditional indexes that tie discrete facts to concepts. A search engine for concepts instead of human language documents.

How big would a good concept model be? Consider a few objects for reference. A human genome is about 3GB. The GPT-3 language model (which can write semi-coherent responses to prompts) is about 300GB. The library of congress is 15TB. Estimates of human memory capacity put us at around a few petabytes (with big error bars). And total digital storage capacity on Earth is around 300 exabytes.

So we can get very interesting behavior with only a few hundred gigabytes. And with only a few terabytes, we can get a whole lot of knowledge, enough that no individual human knows it all. Of course, to use all this knowledge, humans have a very complex brain, containing a lot of models and concepts. But we almost certainly have the digital storage today to capture many hundreds of thousands of brains. And we could probably get to a very rich concept model with much much less. If our representations improve, I wouldn’t be surprised if we could see very smart systems with a few terabytes of storage.

I'll be very curious where the next decade takes us. Models are getting bigger, and it seems that will continue. At some point, models might get big enough and general representations good enough for this concept store to make sense. Maybe it will be designed by humans. Maybe it will be designed by computer systems. Maybe it won't pan out. We'll see.

Finally, I’ll leave off with a few questions that are interesting to ponder.

* What fraction of total computer storage will be concepts vs discrete facts vs models?
* Will the representation structures be fairly simple and well defined like in most of today’s digital systems, or will they be far more varied like in biological systems?
  * My guess is that the human designed representations of the not-so-distant future will still be simple vector representations. But they’ll be a lot bigger, and they’ll be learned by much bigger models on much bigger datasets. But I also suspect that long term there will be a much greater variety of representations reflecting different use cases.
* If this prediction is wrong, why will it be wrong? Some guesses:
  * A pessimists view: We’re never able to generalize well enough to make good use of this idea.
  * Reasoning is too task/model specific, and general vectors just aren’t all that useful. But this is silly. It just doesn’t line up with what we see in biological systems and natural language. Symbolic representations and concept-level abstractions are clearly very useful.
  * Discrete representations don’t work well. Concepts are best captured with interactions between models. In the brain, it seems unclear that there is any single discrete representation of ideas. Maybe they’re captured in the topology and interactions of neurons.
  * We will use general representations, but they will continue to be interior representations that are tied to specific models. I intuitively don’t see why concept representations need to be tied to model structure. But maybe it will turn out better for efficient, practical systems. One reason might be that smart models are small enough to fit on future hardware, so there's no pressing need to move the ideas out of them.

[^1]: We do have general representations today, like word and document embeddings, but they aren't good enough yet. At the time I wrote this, I also hadn’t heard of thought vectors discussed by Hinton in 2015. Hinton thought we were about an order of magnitude off from being able to make use of these in hardware. I'm not very familiar with transfer learning, but that's another area I'd like to dig into.

[^2]: This isn’t to say that we won’t have large models. They’ll probably be much larger than today’s models. But they’ll also be able to draw from a useful bank of concepts.
