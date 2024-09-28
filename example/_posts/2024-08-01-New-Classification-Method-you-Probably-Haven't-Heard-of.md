---
layout: post
title: New Classification Method you Probably Haven't Heard of
image:
  path: /assets/img/blog/Sustainability.jpg
description: >
  New Approach to Classification Based on Transformers, LLM, and Embedding
sitemap: false
---

## New Classification Method you Probably Haven't Heard of

You've probably heard of many classification models for tabular data, from SVMs and logistic regression, to decision tree-based models such as Bagging or Boosting classifiers, to stacking models... This guide will introduce you to many other unconventional approaches, which you've probably never heard of, and which may offer new perspectives or lead to unexpected performance gains.

Deep learning trend from Google Trend

<img src="/assets/img/blog/deeplearningtrend.jpg" alt="drawing" width="500"/>

The representation of tabular data in image form via radar diagrams is a novelty or another method that remains to be explored and researched. The Vision Transformer [ViT] (ViT) is already used for image classification, so the aim is to see whether transforming tabular data into images can add value.

<img src="/assets/img/blog/vit_architecture.jpg" alt="drawing" width="600"/>

Radar plots (also known as spider plots or radar charts) are a popular data visualisation tool that allows us to compare datasets by displaying multiple variables simultaneously on a 2-dimensional plot. Radar charts provide an excellent way to visualize one or more groups of values (depends if it is a binary classification or not) over multiple variables (features of the dataset) (see the image below).

<img src="/assets/img/blog/radarimage.jpg" alt="drawing" width="400"/>

This could be an example of a method for transforming tabular data into images. Other methods involve representing each pixel of the image by the normalized value of the feature.

## Strengths of the approach (ViT on radar diagrams)

1. Capture complex information: Radar diagrams can highlight relationships between features visually, and ViT can capture these relationships through self-attention. This makes it possible to model global dependencies and uncover complex patterns that conventional methods might miss. This approach can be particularly useful if the relationships between features are non-linear or complex, but is probably less suited to data with independent features.

## Potential challenges of the approach

1. Loss of information: Transforming numerical data into 2D graphs could lead to a loss of information, whereas classical models better preserve the details of the raw data.

2. Complexity, resources and interpretability: Training a ViT requires more resources than classical models, which can offer similar results with less complexity and may be easier to interpret (especially for use cases such as telecom churn).

3. Data efficiency : ViTs often require large data sets, whereas classical models are more efficient on smaller sets.

## Conclusion

This approach, along with other DNN techniques for tabular data (such as image transformation or the use of transformers without image transformation), can be useful in some cases, but not always. In particular, it is slower in prediction, which can pose problems in solutions where speed is essential.

Experimentation with different techniques and rigorous evaluation are always essential to identify the best approach for each specific use case.

Future research could focus on optimizing image generation techniques (such as bar charts, for example) and exploring hybrid models that combine the strengths of transformers and traditional approaches. See this article about converting tabular data into prompts with LLMs that produce contextual embeddings for each row, enabling it to identify subtle correlations in the data. For churn prediction, these embeddings are then tabulated and integrated into a CatBoost model. [LLM for classification of tabular data]

<img src="/assets/img/blog/optionclassification.jpg" alt="drawing"/>

Links for reashers focusing on this field:

[Link1]

[Link2]

[Link3]

[Link4]

[Link5]

[Link1]: https://www.nature.com/articles/s41598-021-90923-y
[Link3]: https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0295598
[Link2]: https://proceedings.neurips.cc/paper_files/paper/2021/file/9d86d83f925f2149e9edb0ac3b49229c-Paper.pdf
[Link4]: https://www.sciencedirect.com/science/article/pii/S0167739X24003510
[Link5]: https://arxiv.org/pdf/2401.15238
[ViT]: https://huggingface.co/docs/transformers/model_doc/vit
[LLM for classification of tabular data]: https://www.irjmets.com/uploadedfiles/paper/issue_5_may_2024/57236/final/fin_irjmets1718378031.pdf
