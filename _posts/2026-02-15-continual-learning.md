---
layout: post
title:  "Continual Learning in AI Systems 2026"
permalink: /:title/
date:   2026-02-15 00:00:00 -0800
---

How do we get AI systems that learn continuously in their environment? My brief notes.

Current LLMs do not learn. Weights are not updated after training. They learn only from context (inputs). Everything that the LLM needs to know, we pass to it in the prompt. Chain-of-thought builds on this by having the LLM iterate on its own output until it converges on good answer.

Continuous training causes **catastrophic forgetting**, the loss of what a model already learned. Performance improves on new tasks and declines on old tasks. This is a challenging problem to solve for dense parametric models, since all the knowledge is mushed together in the weights and training updates weights.

Solutions include: 
1) Regularization - elastic weight consolidation (EWC), identify important weights and penalize changes
2) Replay - Retrain on old data to avoid forgetting. Usually mix old/new samples.
3) Architectural - Add new layers and modules for new data. Leave the old data untouched.
4) Distillation - Train a separate teacher model, distill into the original. Incorporate the original loss signal to penalize forgetting.

**Distillation** - Transfer the function of a teacher model to student. Try to preserve the student's knowledge
from prior tasks. `loss = student_loss + alpha * distill_loss` where `distill_loss = KL(p_teacher||p_student)`.
Minimize the KL divergence between the teacher and student output distributions (logits).

At small scale distillation works well. It does not work well at frontier LLM scale yet. It is still too unstable. Recent research into this area produced the next approach.

**Self-Distillation Fine Tuning (SDFT)** - 2026 approach for LLMs. Use a frozen version of the model as the teacher
while fine-tuning the student model. Then optimize `L = loss_student + beta * KL(p_teacher||p_student)`. 
For each sample, the teacher gets both the query and the expert output.
The student sees only the query and must repro the expert output.
The frozen teacher anchors the student penalizing divergence.

**KL Divergence**

KL divergence measures the similarity of two distributions.

```KL(T||S) = sum(T(i) * log(T(i) / S(i))```

It penalizes the relative difference between teacher/student, applying a harsh penalty to "confidently wrong" answers.
