# TakeMeter

**TakeMeter** is a text classifier that labels what kind of contribution a Reddit comment makes to a discussion. This version focuses on public Reddit comments about *Frieren: Beyond Journey's End*. The goal is not to decide whether a comment is right or wrong, but to classify the type of discourse it represents.

## Labels

| Label       | Meaning                                                                                                                     |
| ----------- | --------------------------------------------------------------------------------------------------------------------------- |
| `analysis`  | Explains a point using specific reasoning about story, characters, themes, pacing, animation, worldbuilding, or adaptation. |
| `reaction`  | Mainly expresses an emotional response, personal feeling, or immediate impression.                                          |
| `hot_take`  | Makes a strong, exaggerated, controversial, or absolute claim without enough specific support.                              |
| `lore_info` | Asks for or provides factual information about plot, characters, worldbuilding, episodes, or the manga/anime adaptation.    |

Comments are labeled by their **main purpose**. A comment can mention a feeling and still be `analysis` if its main purpose is to explain an idea. More detailed label definitions and edge cases are in `planning.md`.

## Dataset

The dataset uses public Reddit comments from Frieren-related discussions, including r/Frieren, r/anime episode threads, pacing debates, Mage Exam arc discussions, character threads, and lore/adaptation discussions.

I excluded fanart-only posts, meme-only posts, one-word comments, and comments that could not be understood without missing context.

| Dataset Detail    | Value                                               |
| ----------------- | --------------------------------------------------- |
| Community         | Frieren-related Reddit discussions                  |
| Labels            | `analysis`, `reaction`, `hot_take`, `lore_info`     |
| Test set size     | 17                                                  |
| Test distribution | analysis: 5, reaction: 5, hot_take: 4, lore_info: 3 |

## Models

| Model                                  | Role                  | Setup                                                       |
| -------------------------------------- | --------------------- | ----------------------------------------------------------- |
| `distilbert-base-uncased`              | Fine-tuned classifier | Fine-tuned on my labeled Frieren dataset.                   |
| `llama-3.3-70b-versatile` through Groq | Zero-shot baseline    | Prompted with label definitions, no task-specific training. |

The goal was to compare whether a small fine-tuned model could outperform a larger zero-shot LLM on this discourse classification task.

## Evaluation Results

| Model                    | Accuracy |
| ------------------------ | -------: |
| Zero-shot Groq baseline  |   64.71% |
| Fine-tuned DistilBERT    |   17.65% |
| Random guessing baseline |   25.00% |

The fine-tuned model performed worse than both the zero-shot baseline and random guessing. This suggests that the model did not learn the intended label boundaries.

## Per-Class Metrics

### Fine-tuned DistilBERT

| Label         | Precision |   Recall |       F1 | Support |
| ------------- | --------: | -------: | -------: | ------: |
| `analysis`    |      0.00 |     0.00 |     0.00 |       5 |
| `reaction`    |      0.00 |     0.00 |     0.00 |       5 |
| `hot_take`    |      0.20 |     0.75 |     0.32 |       4 |
| `lore_info`   |      0.00 |     0.00 |     0.00 |       3 |
| **macro avg** |  **0.05** | **0.19** | **0.08** |      17 |

### Zero-shot Groq Baseline

| Label       |            Precision | Recall |     F1 |
| ----------- | -------------------: | -----: | -----: |
| `analysis`  | [fill from notebook] | [fill] | [fill] |
| `reaction`  | [fill from notebook] | [fill] | [fill] |
| `hot_take`  | [fill from notebook] | [fill] | [fill] |
| `lore_info` | [fill from notebook] | [fill] | [fill] |

## Confusion Matrix: Fine-Tuned Model

Rows are true labels. Columns are predicted labels.

| True Label ↓ / Predicted Label → | `analysis` | `reaction` | `hot_take` | `lore_info` |
| -------------------------------- | ---------: | ---------: | ---------: | ----------: |
| `analysis`                       |          0 |          0 |          5 |           0 |
| `reaction`                       |          0 |          0 |          5 |           0 |
| `hot_take`                       |          0 |          1 |          3 |           0 |
| `lore_info`                      |          0 |          1 |          2 |           0 |

The main pattern is that the fine-tuned model collapsed toward predicting `hot_take`. It predicted `hot_take` for 15 of 17 test examples and never predicted `analysis` or `lore_info`.

## Sample Classifications

The table below shows three correctly predicted examples from the fine-tuned model.

| Example Text                                                                                                                     | True Label | Predicted Label | Confidence | Notes                                                                                                                         |
| -------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ---------: | ----------------------------------------------------------------------------------------------------------------------------- |
| “Do we know that that elf is dead? For all we know they’re chilling in the middle of the woods in some corner of the continent.” | `hot_take` | `hot_take`      |       0.28 | This prediction is reasonable because the comment makes a speculative claim without much factual support.                     |
| “Serie is the one that acts smarter than the teacher, but she's always correct.”                                                 | `hot_take` | `hot_take`      |       0.29 | This is a broad claim about a character’s correctness without specific evidence, so `hot_take` fits.                          |
| “Wasn't she depressed hiding in her valley for a thousand years after her master died?”                                          | `hot_take` | `hot_take`      |       0.28 | This comment frames a strong interpretation as a question, but it does not provide much support, so `hot_take` is reasonable. |

Even though these predictions were correct, the confidence scores were low, around 0.28–0.29. This suggests that the model was not strongly confident even when it chose the right label.

## Wrong Prediction Analysis

The biggest error pattern was `analysis → hot_take` and `reaction → hot_take`. The model treated many comments as `hot_take`, even when they were actually supported explanations or personal feelings.

For `analysis`, the model seemed to focus on opinionated wording instead of whether the comment gave evidence. For example, one misclassified comment argued that Qual was powerful because he quickly analyzed Defensive Magic after being sealed for 80 years. The true label was `analysis`, but the model predicted `hot_take`. This failed because the model ignored the comment’s specific evidence and focused more on its confident tone.

For `reaction`, the model missed the difference between “I feel this way” and “the show is objectively bad/overrated.” A comment like “I love her chill demeanor about everything!!” was truly a `reaction`, but the model predicted `hot_take`. This shows that the model did not reliably separate emotional response from broad unsupported claims.

For `lore_info`, the model struggled to recognize factual clarification. One example explained that something in the episode was “not empty” because it contained beheaded corpses, and noted that this was said in the episode. The true label was `lore_info`, but the model predicted `reaction`. This suggests that the model did not learn the factual-information category well.

## What the Model Captured vs. What I Intended

I intended the model to learn four discourse types: supported interpretation, emotional reaction, unsupported strong opinion, and factual clarification. Instead, the fine-tuned model mostly learned to predict `hot_take`.

This means the model did not capture the full label taxonomy. It overfit to surface-level cues that looked opinionated or argumentative, instead of learning the deeper difference between a supported argument, a feeling, and a factual explanation.

The zero-shot Groq baseline performed much better, which suggests that the larger LLM was better at applying the label definitions directly. For this small and subjective dataset, prompting worked better than fine-tuning.

## Did It Meet the Definition of Success?

No. In my planning document, I defined success as around 70–80% accuracy with reasonable per-class F1 scores. The fine-tuned model reached only 17.65% accuracy and had zero F1 for `analysis`, `reaction`, and `lore_info`.

The zero-shot baseline came closer at 64.71%, but it still did not meet the original success target. I would not deploy the fine-tuned model as a real community tool.

## What I Would Do Differently

If I improved this project, I would collect more labeled examples, especially hard boundary cases between `reaction` and `hot_take` and between `analysis` and `lore_info`. I would also check the training split, class balance, learning rate, number of epochs, and whether the model was actually learning during training.

Given these results, the practical version of TakeMeter would probably use the zero-shot or few-shot LLM baseline instead of the fine-tuned DistilBERT model.

## Spec Reflection

The spec helped guide my implementation by making me define my labels before collecting data. That helped me avoid vague categories like “good” and “bad” and forced me to think about real boundary cases.

My implementation diverged from the ideal outcome because fine-tuning did not improve performance. Instead, it revealed a failure case: small-data fine-tuning can perform badly on subjective discourse classification. I kept this result because the project emphasizes honest evaluation and failure analysis.

## AI Tool Usage

I used AI assistance in a few parts of this project.

First, I used ChatGPT to help stress-test my label definitions. I asked it to generate borderline Frieren comments between labels like `reaction` and `hot_take` or `analysis` and `lore_info`. I used this to clarify my decision rules before annotation.

Second, I used AI to help organize and draft parts of my planning document and README. The AI helped format my results and explain the confusion matrix, but I edited the final wording and based the interpretation on my actual model outputs.

If AI was used to suggest labels during annotation, I manually reviewed the suggestions and used my own final label for training and evaluation.

## Repository Structure

```text
.
├── README.md
├── planning.md
├── evaluation_results.json
├── confusion_matrix.png
└── data/
    └── frieren_takemeter_dataset.csv
```

## Final Takeaway

Fine-tuning was not automatically better than prompting. For this small, subjective classification task, the zero-shot Groq baseline performed much better than the fine-tuned DistilBERT model. The fine-tuned model collapsed toward predicting `hot_take`, showing that the dataset and training setup were not enough for it to learn the intended discourse boundaries.
