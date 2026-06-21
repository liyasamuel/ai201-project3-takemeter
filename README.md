Community Choice and Reasoning

I chose r/LetsTalkMusic because it contains a wide variety of discussion styles rather than simple questions or recommendations. Many posts include detailed musical analysis, personal experiences, and strong opinions, making it a good community for building a multi-class text classifier. The differences between these discussion styles are meaningful but sometimes subtle, making the classification task both interesting and challenging.

Label Taxonomy (Additional Examples)
Analysis

Example 1:

Radiohead's Kid A completely changed alternative rock because it incorporated electronic music without abandoning strong songwriting.

Example 2:

Fleetwood Mac's Rumours remains influential because each songwriter contributed a unique perspective while maintaining a consistent overall sound.

Personal Reaction

Example 1:

This album reminds me of high school and always makes me nostalgic.

Example 2:

I never skip this song because it instantly improves my mood.

Hot Take

Example 1:

Vinyl sounds worse than streaming and people only pretend otherwise.

Example 2:

Taylor Swift is the most overrated artist of this generation.

Data Collection and Labeling Process

Posts and comments were manually collected from r/LetsTalkMusic and labeled according to the three-category taxonomy. Each example was reviewed individually and assigned the label that best represented the primary intent of the text. When a post contained multiple characteristics, the label was determined by identifying the dominant communication style rather than isolated phrases. This manual labeling process helped create a consistent supervised learning dataset.

Difficult-to-Label Examples
Example 1

Post:

I think Kendrick is the greatest rapper alive because every album tells a different story.

Chosen Label: Analysis

Reason:

Although the post expresses an opinion, it also provides supporting reasoning by discussing storytelling across albums.

Example 2

Post:

Pink Floyd is overrated.

Chosen Label: Hot Take

Reason:

The statement makes a strong opinion without offering any supporting evidence.

Example 3

Post:

This album helped me through a difficult time in my life.

Chosen Label: Personal Reaction

Reason:

The post focuses on a personal emotional experience rather than analyzing the music itself.

Fine-Tuning Decisions

The DistilBERT model was fine-tuned for three epochs using a learning rate of 2e-5. Three epochs were selected because the dataset contained only 201 labeled examples, reducing the risk of overfitting while still allowing the model to learn meaningful patterns. The learning rate of 2e-5 follows common best practices for fine-tuning transformer-based language models.

Baseline Prompt

The zero-shot baseline used the Groq API with the Llama 3.3 70B Versatile model. The prompt described the three labels, provided one example for each category, included decision rules for ambiguous cases, and instructed the model to respond using only one valid label.

Per-Class Evaluation
Zero-Shot Baseline
Label	Precision	Recall	F1
analysis	1.00	0.59	0.74
personal_reaction	0.33	1.00	0.50
hot_take	1.00	0.75	0.86

Overall Accuracy: 67.7%

Fine-Tuned DistilBERT
Label	Precision	Recall	F1
analysis	0.71	1.00	0.83
personal_reaction	0.00	0.00	0.00
hot_take	0.00	0.00	0.00

Overall Accuracy: 71.0%

Confusion Matrix (Markdown)
Actual \ Predicted	Analysis	Personal Reaction	Hot Take
Analysis	22	0	0
Personal Reaction	5	0	0
Hot Take	4	0	0
Incorrect Predictions
Example 1

Actual: Personal Reaction

Predicted: Analysis

The model focused on descriptive language rather than recognizing that the post primarily expressed personal feelings.

Example 2

Actual: Hot Take

Predicted: Analysis

The model interpreted the opinion as analytical despite the lack of supporting evidence.

Example 3

Actual: Personal Reaction

Predicted: Analysis

The post contained music-related discussion, causing the classifier to favor the majority class.

Sample Classifications
Example	Predicted Label	Confidence	Correct
"Kendrick's albums tell better stories than most modern rappers."	Analysis	0.94	✅
"This album always makes me emotional."	Analysis	0.72	❌
"The Beatles are overrated."	Analysis	0.68	❌
"Radiohead changed alternative music forever because..."	Analysis	0.96	✅
Reflection

The fine-tuned DistilBERT model improved overall accuracy from 67.7% to 71.0%, demonstrating that supervised training on a domain-specific dataset can outperform a zero-shot baseline. However, the model heavily favored the majority class, indicating that the limited and imbalanced dataset prevented it from learning strong decision boundaries for the minority classes. With additional labeled examples and a more balanced dataset, the classifier would likely achieve better performance across all three categories.

Spec Reflection

The project specification helped define a structured workflow by separating planning, data collection, baseline evaluation, fine-tuning, and analysis into distinct stages. During implementation, the workflow diverged slightly because additional examples were added to the dataset after the initial planning phase to improve class balance and overall model performance.

AI Usage

Artificial intelligence tools were used throughout the project as development assistants rather than replacements for manual work.

AI was used to help debug Python, Git, Google Colab, and Hugging Face errors encountered during implementation. All code was reviewed, executed, and tested before being included in the final project.
AI was used to improve documentation, including refining the README structure, grammar, and explanations. The dataset labels, evaluation results, and final analysis were based on the actual outputs produced during training.

Annotation assistance: AI helped brainstorm additional example posts, but every dataset entry was manually reviewed and assigned a final label before being included in the training data.