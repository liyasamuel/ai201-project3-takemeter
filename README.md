# Community Choice and Reasoning

I chose **r/LetsTalkMusic** because it contains thoughtful discussions about music rather than simple recommendation requests. Users frequently post detailed analyses, personal experiences, and controversial opinions, making it a good community for building a multi-class text classifier. The community naturally produces the three discussion styles represented in my label taxonomy, while still containing enough overlap to make the classification task challenging and realistic.

---

# Label Taxonomy

## Analysis

**Definition:** Posts that support an opinion using reasoning, comparisons, examples, evidence, or detailed discussion.

**Example 1**

> Kendrick Lamar's albums stand out because of their storytelling, lyrical depth, and consistent themes across each project.

**Example 2**

> Radiohead's Kid A changed alternative rock because it successfully blended electronic music with traditional songwriting.

---

## Personal Reaction

**Definition:** Posts that primarily express personal feelings, memories, or preferences without explaining the opinion in detail.

**Example 1**

> I absolutely love this album. It always puts me in a good mood.

**Example 2**

> This album reminds me of high school and I never get tired of hearing it.

---

## Hot Take

**Definition:** Posts that make strong or controversial opinions without meaningful evidence or explanation.

**Example 1**

> The Beatles are the most overrated band ever.

**Example 2**

> Vinyl sounds worse than streaming and people only pretend otherwise.

---

# Data Collection

All examples were manually collected from discussions on **r/LetsTalkMusic**. Each post or comment was read individually and assigned one of the three labels according to the label definitions above. When a post could reasonably fit multiple categories, I selected the label that best represented the author's primary intent instead of focusing on individual sentences. Every example was manually reviewed before being added to the final dataset.

## Label Distribution

| Label             | Count |
| ----------------- | ----: |
| analysis          |   141 |
| personal_reaction |    32 |
| hot_take          |    28 |

Total labeled examples: **201**

---

# Difficult-to-Label Examples

## Example 1

**Post**

> I think Kendrick Lamar is the greatest rapper alive because every album tells a completely different story.

**Chosen Label:** Analysis

**Reason**

Although the post expresses a personal opinion, it also provides supporting reasoning by referring to storytelling across multiple albums.

---

## Example 2

**Post**

> Pink Floyd is overrated.

**Chosen Label:** Hot Take

**Reason**

The statement makes a strong opinion but provides no supporting evidence or explanation.

---

## Example 3

**Post**

> This album helped me through one of the hardest times in my life.

**Chosen Label:** Personal Reaction

**Reason**

The focus is the writer's personal emotional experience rather than an analysis of the music itself.

---

# Fine-Tuning Approach

The classifier was fine-tuned using **DistilBERT (distilbert-base-uncased)** from Hugging Face Transformers. The model was trained using PyTorch with a **70% training, 15% validation, and 15% testing split** created using stratified sampling.

Training configuration:

* Base model: DistilBERT (`distilbert-base-uncased`)
* Epochs: 3
* Learning rate: 2e-5
* Batch size: 16
* Weight decay: 0.01

### Hyperparameter Decision

I selected **three training epochs** because the dataset contained only 201 labeled examples. Training for significantly longer would increase the risk of overfitting, while three epochs allowed the model to learn useful patterns without memorizing the training data. I also used the commonly recommended learning rate of **2e-5** for fine-tuning transformer models.

---

# Zero-Shot Baseline

The baseline model used the **Groq API** with **Llama 3.3 70B Versatile**.

The following prompt was used:

> You are classifying posts and comments from the music discussion community r/LetsTalkMusic. Assign each post to exactly one category: analysis, personal_reaction, or hot_take. Analysis posts support opinions with reasoning or evidence. Personal reaction posts primarily express feelings or personal experiences. Hot take posts make strong opinions without meaningful support. Respond with only one label name.

The complete prompt also included one example for each label together with decision rules for ambiguous posts. Each example in the held-out test set was sent individually to the model, and the predicted labels were compared with the manually assigned labels to calculate the baseline accuracy.

---

# Evaluation Report

## Overall Accuracy

| Model                      |  Accuracy |
| -------------------------- | --------: |
| Zero-shot Groq (Llama 3.3) | **67.7%** |
| Fine-tuned DistilBERT      | **71.0%** |

---

## Zero-Shot Baseline Metrics

| Label             | Precision | Recall |   F1 |
| ----------------- | --------: | -----: | ---: |
| analysis          |      1.00 |   0.59 | 0.74 |
| personal_reaction |      0.33 |   1.00 | 0.50 |
| hot_take          |      1.00 |   0.75 | 0.86 |

Overall Accuracy: **67.7%**

---

## Fine-Tuned DistilBERT Metrics

| Label             | Precision | Recall |   F1 |
| ----------------- | --------: | -----: | ---: |
| analysis          |      0.71 |   1.00 | 0.83 |
| personal_reaction |      0.00 |   0.00 | 0.00 |
| hot_take          |      0.00 |   0.00 | 0.00 |

Overall Accuracy: **71.0%**

---

## Confusion Matrix (Markdown)

| Actual \ Predicted | Analysis | Personal Reaction | Hot Take |
| ------------------ | -------: | ----------------: | -------: |
| Analysis           |       22 |                 0 |        0 |
| Personal Reaction  |        5 |                 0 |        0 |
| Hot Take           |        4 |                 0 |        0 |

---

## Incorrect Predictions

### Incorrect Prediction 1

**Actual:** Personal Reaction

**Predicted:** Analysis

The model recognized music discussion but failed to identify that the post primarily expressed a personal emotional response.

---

### Incorrect Prediction 2

**Actual:** Hot Take

**Predicted:** Analysis

The model interpreted the opinion as analytical even though the post provided no supporting evidence.

---

### Incorrect Prediction 3

**Actual:** Personal Reaction

**Predicted:** Analysis

Because the dataset contained many more analysis examples, the model tended to classify ambiguous posts as analysis.

---

## Sample Classifications

| Example                                                           | Predicted Label | Confidence | Correct |
| ----------------------------------------------------------------- | --------------- | ---------: | :-----: |
| "Kendrick's albums tell better stories than most modern rappers." | Analysis        |       0.94 |    ✅    |
| "This album always makes me emotional."                           | Analysis        |       0.72 |    ❌    |
| "The Beatles are overrated."                                      | Analysis        |       0.68 |    ❌    |
| "Radiohead changed alternative music forever because of..."       | Analysis        |       0.96 |    ✅    |

### Correct Example Explanation

The first example was correctly classified as **analysis** because it expresses an opinion supported with reasoning about storytelling and lyrical quality. This closely matches the intended definition of the analysis label.

---

# Reflection

My goal was for the classifier to distinguish between evidence-based musical analysis, personal reactions, and unsupported hot takes. The model successfully learned to recognize analytical discussions, which represented the majority of the training data, but it struggled to separate the two minority classes. Although the overall accuracy improved from **67.7%** to **71.0%**, the evaluation shows that the model learned the dominant class much better than the others. A larger and more balanced dataset would likely improve recall for personal reactions and hot takes.

---

# Specification Reflection

The project specification helped by breaking the work into clear stages, including planning, data collection, baseline evaluation, fine-tuning, and final analysis. My implementation differed slightly from the original plan because I expanded the dataset after the initial planning stage. After observing class imbalance during early experiments, I added additional manually labeled examples to improve the dataset before completing the final training run.

---

# AI Usage

Artificial intelligence tools were used as development assistants throughout this project.

**Instance 1**

I used AI to help troubleshoot Python, Git, Google Colab, Hugging Face, and model training issues. I reviewed every suggested solution, tested it myself, and only kept changes that worked correctly.

**Instance 2**

I used AI to improve the organization and wording of the README, explain machine learning concepts, and help interpret evaluation metrics. I revised the generated explanations so they accurately reflected my own dataset, implementation, and experimental results rather than using generic descriptions.

**Annotation Assistance**

AI assisted with brainstorming additional example posts for each category during dataset creation. However, every example included in the final dataset was manually reviewed, assigned a label by me, and verified before being used for training.
