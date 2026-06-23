# TakeMeter Planning Document

## Project Overview

For this project, I am building **TakeMeter**, a text classifier that categorizes the type of discourse in online discussion. My classifier will focus on Reddit comments about *Frieren: Beyond Journey’s End*. The goal is not to decide whether a comment is right or wrong, but to identify what kind of contribution it makes to a discussion.

I will classify comments into four labels: **Analysis**, **Reaction**, **Hot Take**, and **Lore/Info**. These categories separate comments that explain ideas with reasoning from comments that mainly express feelings, make unsupported strong claims, or provide factual clarification.

## Community Choice

I chose Frieren-related Reddit discussions because the community is active, text-heavy, and varied in discourse quality. Fans discuss pacing, character writing, emotional storytelling, fantasy worldbuilding, manga/anime adaptation choices, and whether the show deserves its popularity.

This community is a good fit because Frieren naturally creates different kinds of comments. Some fans write thoughtful interpretations about memory, grief, time, and character development. Others post immediate emotional reactions, ask factual questions, or make broad claims like “Frieren is overrated” or “Frieren is a masterpiece.” These differences make the community useful for a classification task.

## Community and Label Description

This project focuses on Reddit comments about *Frieren: Beyond Journey’s End*, especially discussion threads where fans debate the show’s pacing, themes, characters, emotional impact, and worldbuilding. I am using **Analysis**, **Reaction**, **Hot Take**, and **Lore/Info** to distinguish between supported interpretations, emotional responses, unsupported strong opinions, and factual explanations. These distinctions matter because Frieren discussions often center on whether the show’s slow pacing and emotional storytelling are genuinely meaningful or simply overhyped.

## Label Taxonomy

### Analysis

A comment is labeled **Analysis** if it explains a point about *Frieren* using specific reasoning about the story, characters, themes, pacing, animation, worldbuilding, or adaptation.

**Examples:**

“Frieren’s slow pacing works because the story is about how differently she experiences time compared to humans. The quiet moments after Himmel’s death show that she is only beginning to understand the emotional weight of relationships after they are already gone.”

“The Mage Exam arc changes the tone of the show, but I don’t think it ruins it. It expands the world and shows how magic is treated socially and politically, instead of only using magic as a battle system.”

**Uncertain case:**
“Frieren is slow, but that’s kind of the point.”

This could be **Analysis** because it recognizes intentional pacing, but it does not explain much. I will label a comment as **Analysis** only if it gives a clear reason, example, or connection to a theme, character, scene, or arc. Otherwise, short impressions like this will usually be **Reaction**.

### Reaction

A comment is labeled **Reaction** if it mainly expresses an emotional response, personal feeling, or immediate impression without developing a full argument.

**Examples:**

“This episode made me cry. Himmel scenes always hit so hard.”

“I loved this episode so much. It was quiet, cozy, and really beautiful.”

**Uncertain case:**
“I was bored during the Mage Exam arc because it felt like a totally different show.”

This is mostly **Reaction** because it centers on a personal feeling, even though it gives a small reason. If the comment expands into a more detailed explanation of pacing, structure, genre shift, or storytelling choices, I will label it **Analysis**.

### Hot Take

A comment is labeled **Hot Take** if it makes a strong, exaggerated, controversial, or absolute claim about *Frieren* without enough specific support.

**Examples:**

“Frieren is only popular because people want to pretend slow anime is deep.”

“Fern ruins the show. She is annoying and badly written.”

**Uncertain case:**
“Frieren is overrated because the story barely has anything happening for most of the season.”

This could be **Hot Take** because “overrated” is a broad and controversial claim, but it gives a vague reason. I will label a comment as **Hot Take** when it makes a large claim with general, dismissive, or weak support. If it supports the criticism with specific scenes, arcs, character moments, or pacing patterns, I will label it **Analysis**.

### Lore/Info

A comment is labeled **Lore/Info** if it asks for or provides factual information about the plot, characters, manga/anime adaptation, worldbuilding, episodes, or viewing context.

**Examples:**

“The first season adapts the early parts of the manga and includes the Mage Exam arc, but there is still more manga content after the anime ends.”

“Serie is an ancient elf mage, and her view of magic is very different from Frieren’s because she values power and talent more directly.”

**Uncertain case:**
“The magic system is interesting because spells seem tied to imagination and visualization.”

This could be **Lore/Info** because it describes the magic system, but it could become **Analysis** if the comment uses that detail to interpret the story. I will label factual clarification as **Lore/Info** and arguments based on those facts as **Analysis**.

## Edge Cases and Mutual Exclusivity

Because real comments can contain multiple elements, I will label each comment based on its **main purpose**. A comment can mention a feeling and still be **Analysis** if the main purpose is to explain an idea. A comment can mention a fact and still be **Analysis** if it uses that fact to build an interpretation.

My decision rules are:

* If the comment builds an argument with specific reasoning or examples, label it **Analysis**.
* If the comment mainly asks for or gives factual clarification, label it **Lore/Info**.
* If the comment makes a strong claim without enough support, label it **Hot Take**.
* If the comment mainly expresses a feeling or immediate impression, label it **Reaction**.

The hardest overlaps will probably be **Reaction vs. Hot Take** and **Analysis vs. Lore/Info**. A reaction says how the viewer felt, while a hot take makes a broader judgment about the show, characters, fans, or quality. Lore/Info provides facts, while Analysis uses facts to make an interpretation or argument.

If many comments fit two labels equally well during annotation, I will revise the boundary rules before finishing the dataset. If two labels overlap too much, I may merge them or redefine them.

## Hard Edge Cases

These are specific comments I found difficult to label during annotation, along with why they were hard and how I decided.

### Case 1: Overall impression with a brief reason — Reaction vs. Analysis

**Comment:**
"Really hard show to get people into in my experience. I personally love the slow start and the pacing, and I think it really makes the higher intensity arcs shine brighter because they actually build the characters. Definitely a one of a kind anime in my repertoire."

**Why it was hard:**
The comment expresses the commenter's overall feeling about the show, but it also gestures at a reason — that the slow pacing builds characters and makes the higher-intensity arcs stronger. That partial reasoning pulls it toward **Analysis**, while the personal, impression-based framing pulls it toward **Reaction**.

**Decision:**
I labeled it **Reaction**. Although the commenter is responding to the show, they are describing a general feeling about it as a whole rather than analyzing a specific scene, arc, or moment. The reasoning stays at the level of broad personal impression ("I love the slow start," "one of a kind"), so the comment defaults to the more general feeling rather than a developed argument.

### Case 2: Questions about the magic system — Analysis vs. Lore/Info

**Comment:**
"It's stated many times that imagination is everything in spellcasting, but really what's even the limit to this? Or what does imagination really determine? For example, Übel could cut a defensive magic so strong that no one could penetrate just because 'I imagined cutting the cloak because cloaks can be cut.' I don't think a magic system as great as Frieren's would have a flaw like this — is there anything I have missed about this explanation, or is this really it? If it's really all about imagining whether a spell can do something or not, then wouldn't it make extremely delusional people extremely strong mages?"

**Why it was hard:**
The comment leans toward **Analysis** because the commenter is not just stating facts about the magic system — they are interpreting it, probing its logic, and raising their own theories (e.g., that delusional people might become strong mages). At the same time, the comment is framed almost entirely as open questions about how the established lore works, which pulls it toward **Lore/Info**.

**Decision:**
I labeled it **Lore/Info**. The main purpose of the post is to ask how the magic system actually works and to invite clarification, rather than to commit to a developed argument. The interpretive parts are posed as questions, not conclusions. This was also the thread that started a larger discussion, where the more developed **Analysis** appeared in the replies. Since this comment's primary function is to raise the question and request explanation, I treated it as **Lore/Info** and reserved the **Analysis** label for the responses that built arguments from it.

### Case 3: Personal impression that turns into a claim — Reaction vs. Hot Take

**Comment:**
"I've never heard of it described as the saddest thing ever, just that the first episode will make you cry. Not might make you cry, will make you cry. The rest of the show never has that lingering feeling, but lives in the present."

**Why it was hard:**
The first part of the comment reads like a **Reaction** — the commenter is expressing their own emotional impression of the show and how it affected them. But the comment ends with a broader claim about the show as a whole ("the rest of the show never has that lingering feeling, but lives in the present"), which is a general, somewhat absolute statement about its tone. That shift pulls it from a personal feeling toward a **Hot Take**.

**Decision:**
I labeled it **Hot Take**. The closing statement is doing the main work of the comment: the commenter uses their reaction and feeling to support a broad characterization of the entire show rather than just describing how it made them feel. Because the comment builds toward that sweeping claim instead of staying a raw emotional response, I treated it as a **Hot Take** rather than a **Reaction**.

## Data Collection Plan

I plan to collect at least **200 public text examples** from Frieren-related Reddit discussions. I will mainly collect comments rather than posts because comments are more varied and easier to classify by discourse type.

Possible sources include:

* r/Frieren discussion threads
* r/anime Frieren episode discussions
* threads about whether Frieren is overrated, boring, or overhyped
* threads about the Mage Exam arc
* character discussions about Frieren, Fern, Stark, Himmel, Serie, and others
* lore, adaptation, and manga/anime comparison threads

I will avoid fanart-only posts, meme-only posts, one-word comments, image reactions with little text, and comments that cannot be understood without missing context.

My target is a balanced dataset:

| Label     | Target Count |
| --------- | -----------: |
| Analysis  |           50 |
| Reaction  |           50 |
| Hot Take  |           50 |
| Lore/Info |           50 |
| **Total** |      **200** |

If one label is underrepresented, I will collect from thread types where that label is more common. For example, **Hot Take** may require debate threads about whether Frieren is overrated or boring, while **Lore/Info** may require question, adaptation, and worldbuilding threads. If a label remains too rare, I will reconsider whether the label should be merged or redefined.

## Evaluation Metrics

I will use more than accuracy because accuracy alone can hide problems with class imbalance. A model could look accurate while mostly predicting the most common label.

I will report:

* **Overall accuracy** to measure the percent of correct predictions.
* **Per-class precision** to see how reliable the model is when it predicts each label.
* **Per-class recall** to see whether the model misses certain labels.
* **Per-class F1 score** to balance precision and recall.
* **Confusion matrix** to see which labels the model mixes up.

The confusion matrix is especially important because I expect mistakes between **Reaction** and **Hot Take**, **Analysis** and **Lore/Info**, and **Hot Take** and **Analysis**. These errors will show whether the model is struggling because of weak label boundaries or because the text itself is ambiguous.

I will also compare my fine-tuned model against a zero-shot Groq llama-3.3-70b-versatile baseline on the same test set. This will show whether fine-tuning actually improves performance over a general LLM with no task-specific training.

## Definition of Success

Since this is a four-label classification task, random guessing would be around 25% accuracy. I would consider the project successful if the fine-tuned model reaches around **70–80% accuracy** on the test set and has reasonable per-class F1 scores, especially for **Analysis** and **Hot Take**.

For a real community tool, I would want at least **80% overall accuracy**, no extremely weak label category, and error patterns that are understandable. I would not use this model to automatically remove or punish comments. A safer use would be organizing discussion, surfacing analytical comments, or helping moderators understand the types of discourse happening in the community.

A useful model should usually be able to distinguish between a thoughtful critique of Frieren’s pacing, a simple emotional reaction, an unsupported “Frieren is overrated” claim, and a factual explanation about the story or adaptation.

## AI Tool Plan

I plan to use AI tools for **label stress-testing**, **annotation assistance**, and **failure analysis**.

For label stress-testing, I will give an AI tool my label definitions and boundary rules, then ask it to generate 5–10 Frieren-related comments that sit between two labels. These generated examples will not be part of my dataset. I will only use them to check whether my labels are clear enough before annotating 200 real examples. I will especially test the boundaries between **Analysis vs. Reaction**, **Reaction vs. Hot Take**, **Hot Take vs. Analysis**, and **Lore/Info vs. Analysis**.

For annotation assistance, I may use ChatGPT or another LLM to suggest labels for some real examples before I review them myself. I will not treat AI labels as final. If I use this workflow, I will track it with columns such as `ai_suggested_label`, `final_human_label`, and `ai_assisted`. Only `final_human_label` will be used for training and evaluation. I will disclose this in my README or AI usage section.

For failure analysis, I will give an AI tool a list of wrong predictions after testing the model. I will include the original comment, true label, and predicted label, then ask it to identify possible error patterns. I will look for patterns like confusion between short analysis and reaction, negative criticism being mislabeled as hot take, lore explanations being mislabeled as analysis, overprediction of the most common label, or problems with sarcastic/context-dependent comments.

The AI’s suggestions will not be treated as final conclusions. I will verify any pattern myself by checking the actual misclassified examples and only reporting patterns that appear across multiple test cases.

## Initial Reflection

The hardest part of this project will likely be annotation consistency. I need to avoid labeling based on whether I personally agree with a comment. A negative comment can still be **Analysis** if it gives specific reasoning, and a positive comment can still be a **Hot Take** if it makes an exaggerated claim without support.

My main focus during annotation will be deciding what the comment is primarily doing. This should help keep the labels mutually exclusive and make the final model easier to evaluate.
## Fine-Tuned Model Evaluation

I fine-tuned `distilbert-base-uncased` on my labeled Frieren discourse dataset using four labels: `analysis`, `reaction`, `hot_take`, and `lore_info`. The fine-tuned model was evaluated on a held-out test set of 17 examples. The zero-shot Groq baseline achieved an accuracy of **0.6471**, while the fine-tuned DistilBERT model achieved an accuracy of **0.1765**. This means the fine-tuned model performed **0.4706 worse** than the zero-shot baseline on the same test set.

### Results Comparison

| Model                    | Accuracy |
| ------------------------ | -------: |
| Zero-shot baseline: Groq |   0.6471 |
| Fine-tuned DistilBERT    |   0.1765 |
| Difference               |  -0.4706 |

The fine-tuned model performed worse than expected. Since this is a four-label classification task, random guessing would be around 25% accuracy, so an accuracy of 17.65% suggests that the model did not learn the label distinctions well. This is likely due to a combination of small dataset size, small test set size, and difficulty learning subjective discourse categories from limited examples.

### Fine-Tuned Model Confusion Matrix

| True Label | Predicted analysis | Predicted reaction | Predicted hot_take | Predicted lore_info |
| ---------- | -----------------: | -----------------: | -----------------: | ------------------: |
| analysis   |                  0 |                  0 |                  5 |                   0 |
| reaction   |                  0 |                  0 |                  5 |                   0 |
| hot_take   |                  0 |                  1 |                  3 |                   0 |
| lore_info  |                  0 |                  1 |                  2 |                   0 |

The confusion matrix shows that the fine-tuned model predicted `hot_take` for most examples. It never correctly predicted `analysis`, `reaction`, or `lore_info`, and it only correctly classified 3 examples from the `hot_take` class. This suggests that the model may have collapsed toward one dominant prediction pattern rather than learning the full label taxonomy.

### What Went Wrong

One issue is that the test set was very small, with only 17 examples. With such a small test set, each mistake has a large effect on the final accuracy. However, the confusion matrix shows a deeper issue than just test set size: the model heavily overpredicted `hot_take`.

A likely reason is that the dataset was too small for DistilBERT to learn subtle distinctions between subjective discourse labels. Labels like `analysis`, `reaction`, and `hot_take` can overlap in real comments. For example, a comment can express frustration while also making a broad claim, or it can include factual details while using them to make an interpretation. A larger dataset with more examples of each boundary case would likely help the model learn these differences more reliably.

Another possible issue is that the model may have learned surface-level cues instead of deeper reasoning. Words associated with strong opinions, such as “overrated,” “boring,” “bad,” or “best,” may have pushed the model toward `hot_take`, even when the comment actually contained enough reasoning to count as `analysis`.

### Comparison to Zero-Shot Baseline

The zero-shot Groq baseline performed much better than the fine-tuned DistilBERT model. This makes sense because the Groq model is a much larger language model with stronger general reasoning ability. It can use the label definitions directly at inference time, while DistilBERT had to learn the classification boundaries from a small training dataset.

This result shows that fine-tuning is not automatically better than prompting. For this project, the zero-shot model was better at applying the label definitions, while the fine-tuned model struggled to generalize.

### Reflection

The main lesson from this result is that label design and dataset size matter more than simply running a fine-tuning pipeline. My labels were meaningful, but they were subjective and sometimes close together. With a small dataset, the fine-tuned model did not have enough examples to learn the boundaries between `analysis`, `reaction`, `hot_take`, and `lore_info`.

If I improved this project, I would collect more labeled examples, especially difficult boundary cases between `reaction` and `hot_take` and between `analysis` and `lore_info`. I would also check the training split, make sure the labels are balanced across train/validation/test sets, and experiment with more training epochs or class weighting. I would not deploy the current fine-tuned model as a real community tool because it overpredicts `hot_take` too often. However, the evaluation is still useful because it shows where the model failed and why the zero-shot baseline was stronger for this task.
## Sample Classifications

The table below shows example Frieren comments run through the fine-tuned DistilBERT model. Each row includes the model’s predicted label and confidence score.

| Example Text                                                                                  | Predicted Label | Confidence | Correct? | Notes                                                                                                                                            |
| --------------------------------------------------------------------------------------------- | --------------- | ---------: | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| “Frieren is only popular because people want to pretend slow anime is deep.”                  | `hot_take`      |       0.74 | Yes      | This prediction is reasonable because the comment makes a broad, unsupported claim about the show and its fans without giving specific evidence. |
| “This episode made me cry. Himmel scenes always hit so hard.”                                 | `hot_take`      |       0.61 | No       | This should have been `reaction` because the comment mainly expresses an emotional response.                                                     |
| “Does season 1 cover the whole manga, or is there more after the anime?”                      | `reaction`      |       0.43 | No       | This should have been `lore_info` because it asks for factual adaptation information.                                                            |
| “The Mage Exam arc changes the tone by making magic feel more institutional and competitive.” | `hot_take`      |       0.58 | No       | This should have been `analysis` because it explains a specific interpretation of the arc.                                                       |
| “Fern is annoying and ruins the show.”                                                        | `hot_take`      |       0.69 | Yes      | This prediction is reasonable because the comment makes a strong character judgment without much support.                                        |

These examples show that the fine-tuned model was most comfortable predicting `hot_take`, even when the text belonged to another category. It sometimes recognized unsupported strong opinions correctly, but it struggled with emotional reactions, factual questions, and shorter analytical comments.

## Reflection: What the Model Captured vs. What I Intended

My intended label taxonomy separated Frieren comments into four types of discourse: `analysis`, `reaction`, `hot_take`, and `lore_info`. The goal was for the model to learn the difference between supported interpretation, emotional response, unsupported strong opinion, and factual clarification. In other words, I wanted the model to classify what a comment was mainly doing in the discussion.

However, the fine-tuned model did not fully capture these distinctions. Based on the confusion matrix, it mostly predicted `hot_take`, even for comments whose true labels were `analysis`, `reaction`, or `lore_info`. This suggests that the model’s learned decision boundary was much narrower than my intended label definitions. Instead of learning the full structure of Frieren discourse, it seemed to overfit toward one category.

The model may have overfit to surface-level cues associated with strong opinions. Words or phrasing related to criticism, disagreement, or judgment may have pushed the model toward `hot_take`, even when a comment actually gave reasoning and should have been labeled `analysis`. It also missed the difference between a personal feeling and a broader unsupported claim. For example, “I was bored” should be treated as `reaction`, while “Frieren is boring and overrated” should be treated as `hot_take`. The model appeared to blur that boundary.

The model also struggled to separate `lore_info` from other categories. This matters because my label definition treated factual explanation as different from interpretation. A comment that explains manga adaptation details or character background should be `lore_info`, while a comment that uses those facts to make an argument should be `analysis`. The fine-tuned model did not reliably learn this distinction, likely because the dataset was small and the boundary between facts and interpretation can be subtle.

Overall, the model captured that some Frieren comments involve strong claims, but it missed the broader goal of distinguishing different kinds of discourse. The zero-shot Groq baseline performed much better, which suggests that a larger language model was better able to apply my label definitions directly. The fine-tuned DistilBERT model likely needed more labeled examples, more balanced boundary cases, and possibly more training adjustments before it could become useful.

I would not deploy the current fine-tuned model as a real community tool because it overpredicts `hot_take` too often. A better version would need more examples of `analysis`, `reaction`, and `lore_info`, especially examples that sit near the boundary between labels. Still, the failure is informative: it shows that fine-tuning does not automatically improve performance, especially when the task involves subjective discourse labels and a small dataset.
