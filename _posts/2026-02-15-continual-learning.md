---
layout: post
title:  "Continual Learning in AI Systems"
permalink: /:title/
date:   2026-02-15 00:00:00 -0800
---

How do we get AI systems that learn continuously in their environment?

Current LLMs do not learn. Weights are not updated after training. They learn only from context (inputs).

Continuous training causes **catastrophic forgetting**, the loss of what a model already learned.
Performance improves on new tasks and declines on old tasks.

Solutions include: 
1) Regularization - elastic weight consolidation (EWC), identify important weights and penalize changes
2) Replay - Revisit old data
3) Architectural - Train a separate teacher model, distill into the original.

**Distillation** - Transfer the function of a teacher model to student. Try to preserve the student's knowledge
from prior tasks. `loss = student_loss + alpha * distill_loss` where `distill_loss = KL(p_teacher||p_student)`.
Minimize the KL divergence between the teacher and student output distributions (logits).

At small scale distillation works well. It does not work well at frontier LLM scale yet. It is still too unstable.

**Self-Distillation Fine Tuning (SDFT)** - 2026 approach for LLMs. Use a frozen version of the model as the teacher
while fine-tuning the student model. Then optimize `L = loss_student + beta * KL(p_teacher||p_student)`. 
For each sample, the teacher gets both the query and the expert output.
The student sees only the query and must repro the expert output.
The frozen teacher anchors the student penalizing divergence.

```KL(T||S) = sum(T(i) * log(T(i) / S(i))```

KL divergence penalizes the relative difference between teacher/student, applying a harsh penalty to "confidently wrong" answers.
