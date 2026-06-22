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
