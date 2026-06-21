# TakeMeter Planning Document

## Community

**Chosen Community:** r/LetsTalkMusic

I chose r/LetsTalkMusic because it contains thoughtful discussions about artists, albums, genres, and music history. Members regularly share opinions, reviews, and debates, making it a strong community for building a text classification model. The variety of discussions provides enough different writing styles and viewpoints to create a meaningful dataset for training and evaluating a classifier.

## Labels

### 1. Analysis

**Definition:** A post that supports its opinion with reasoning, comparisons, examples, or evidence related to music.

**Example 1:**
"Pink Floyd's *The Dark Side of the Moon* remains influential because its production techniques and conceptual storytelling changed progressive rock."

**Example 2:**
"Kendrick Lamar's albums stand out because of their lyrical depth, storytelling, and consistent themes across each project."

---

### 2. Personal Reaction (personal_reaction)

**Definition:** A post that mainly expresses personal feelings, preferences, or emotions without explaining the opinion in detail.

**Example 1:**
"I absolutely love this album. I never get tired of listening to it."

**Example 2:**
"This song always makes me happy whenever it comes on."

---

### 3. Hot Take (hot_take)

**Definition:** A post that makes a strong or controversial opinion about music without providing meaningful evidence or reasoning.

**Example 1:**
"The Beatles are the most overrated band ever."

**Example 2:**
"Modern pop music has no creativity anymore."

## Hard Edge Cases

One of the most difficult cases is when a post begins with a bold opinion but also includes some reasoning.

**Example:**

"Taylor Swift is overrated because many of her recent songs sound too similar."

This could be labeled as either **hot_take** or **analysis**.

To keep my labeling consistent, I will use the following rule:

- If the post provides meaningful reasoning, examples, or evidence that supports the opinion, I will label it **analysis**.
- If the post mainly makes a bold or controversial statement without meaningful support, I will label it **hot_take**.

This decision rule will be used throughout the annotation process to ensure consistency. I will apply this rule consistently throughout the dataset so that similar posts always receive the same label.

## Data Collection Plan

I will collect public posts and comments from r/LetsTalkMusic. My goal is to create a dataset containing at least 200 labeled examples. Each example will be manually reviewed and assigned one of the three labels: analysis, personal_reaction, or hot_take.

I will aim for a balanced dataset so that no single label dominates the training data. If one label has significantly fewer examples than the others, I will continue collecting additional posts that fit that category until the distribution is more balanced.

## Evaluation Metrics

I will evaluate my model using overall accuracy, precision, recall, F1-score, and a confusion matrix.

Overall accuracy will show how many predictions are correct across the entire test set. Precision and recall will help measure how well the model performs for each individual label, especially if some labels are more difficult to predict than others. The F1-score provides a balance between precision and recall, making it useful for comparing performance across labels. The confusion matrix will help identify which labels the model confuses most often so I can better understand its weaknesses.

## Definition of Success

I will consider this project successful if my fine-tuned model performs noticeably better than the zero-shot baseline on the same test set. I would like the model to achieve at least 80% overall accuracy while also maintaining balanced precision, recall, and F1-scores across all three labels. A successful classifier should correctly identify most music discussions without consistently favoring one label over the others.

## AI Tool Plan

I will use AI tools to improve my project in three ways.

First, I will use ChatGPT to test my label definitions by generating examples that are difficult to classify. This will help me identify unclear label boundaries before I begin annotating data.

Second, I may use ChatGPT to suggest labels for a small batch of examples, but I will manually review every prediction before adding it to my dataset. All final labels will be my own decisions.

Finally, after evaluating my model, I will use ChatGPT to analyze incorrect predictions and identify common error patterns. I will verify these patterns myself before including them in my evaluation report.