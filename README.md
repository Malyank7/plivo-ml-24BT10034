# RUNLOG

## Objective

Improve the baseline GPT language model under the constraints of:
- Maximum 2000 optimizer steps
- Maximum 2M parameters
- CPU only
- No pretrained models

---

## Run 1 – Baseline

Hypothesis

Run the provided implementation to establish a baseline BPB and understand the training behaviour.

Changes

- No modifications.

Observation

The baseline converged but used a constant learning rate, Adam optimizer, no regularization, no gradient clipping and a short context window.

Conclusion

The baseline served as the reference for future experiments.

---

## Run 2 – AdamW Optimizer

Hypothesis

AdamW should improve generalization through decoupled weight decay.

Changes

- Adam → AdamW
- Weight decay = 0.01

Observation

Training became more stable and validation BPB improved.

Conclusion

Kept AdamW.

---

## Run 3 – Cosine Learning Rate Scheduler

Hypothesis

A decaying learning rate should improve convergence within the limited 2000-step budget.

Changes

- Added CosineAnnealingLR scheduler.

Observation

Loss decreased more smoothly during later training.

Conclusion

Kept cosine scheduler.

---

## Run 4 – Gradient Clipping

Hypothesis

Gradient clipping should prevent unstable parameter updates.

Changes

- Gradient clipping = 1.0

Observation

Training remained stable throughout optimization.

Conclusion

Kept gradient clipping.

---

## Run 5 – Weight Tying

Hypothesis

Sharing embedding and output projection weights should reduce redundant parameters while improving language modeling.

Changes

- tie_weights=True

Observation

Parameter efficiency improved without increasing model size.

Conclusion

Kept weight tying.

---

## Run 6 – Improved Initialization

Hypothesis

Smaller initialization variance should improve optimization stability.

Changes

- std = 0.02

Observation

Training became smoother during the early optimization phase.

Conclusion

Kept the initialization.

---

## Run 7 – Larger Context Window

Hypothesis

Increasing context length should allow the model to capture longer dependencies.

Changes

- block_size = 192

Observation

Evaluation BPB improved compared to the shorter context.

Conclusion

Kept block size 192.

---

## Run 8 – Hyperparameter Tuning

Hypothesis

A larger batch and higher learning rate would utilize the 2000-step budget more effectively.

Changes

- Batch size = 16
- Learning rate = 1e-3

Observation

Training converged faster while maintaining stable optimization.

Conclusion

Kept the new hyperparameters.

---

## Final Configuration

Optimizer: AdamW

Weight decay: 0.01

Scheduler: Cosine Annealing

Gradient clipping: 1.0

Learning rate: 1e-3

Batch size: 16

Dropout: 0.1

Block size: 192

Weight tying: Enabled

Initialization std: 0.02

Final BPB

2.4181