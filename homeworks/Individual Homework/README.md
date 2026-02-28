User Behavior Prediction Reproduction (SoMe Benchmark)

This project reproduces the **User Behavior Prediction (UBP)** task from the **SoMe benchmark** and evaluates different prompting strategies using a large language model (LLM).

The goal of the task is to **predict whether a user will like a specific social media post** based on the available information.

We reproduce the original setup and implement three different inference methods to analyze how additional context affects prediction performance.

---

# Project Overview

Social media platforms generate massive amounts of user interaction data. Predicting user behaviors such as **likes, comments, and reposts** is important for applications like:

* recommendation systems
* user profiling
* personalized content delivery

The **SoMe benchmark** proposes a set of tasks for evaluating LLMs on social media understanding.

In this project, we focus on the **User Behavior Prediction** task and evaluate whether LLMs can infer if a user would like a given post.

---

# Reproduction Target

We reproduce the **User Behavior Prediction** task described in the SoMe benchmark paper.

The model is given:

* a **user ID**
* a **social media post**
* optional **additional context**

The model must output:

```
是 (Yes)
否 (No)
```

indicating whether the user would like the post.

---

# Methods

We implemented three different methods for comparison.

## 1. Baseline

The baseline method only provides the **target post** to the model.

The model predicts the user behavior based solely on the content of the post.

Input:

```
User ID + Target Post
```

This represents the **simplest inference setting**.

---

## 2. History Method

The history method provides the model with **a small number of posts that the user liked previously**.

Input:

```
User liked posts history + Target Post
```

The idea is that the model can infer **user preference patterns** from past behavior.

---

## 3. Retrieval Method

The retrieval method introduces **retrieved posts similar to the target post**.

Input:

```
Target Post + Top similar posts
```

This simulates a **retrieval-augmented reasoning process** where the model compares the target post with similar content.

---

# Evaluation Metrics

Two metrics are used to evaluate the results.

### TCR (Task Completion Rate)

Measures whether the model produced a valid output.

```
TCR = valid_predictions / total_samples
```

---

### ACC (Accuracy)

Measures how often the predicted behavior matches the ground truth.

```
ACC = correct_predictions / valid_predictions
```

Predictions are compared against the **ground truth labels** in the dataset.

---

# Experimental Setup

### Model

Gemini-3-Flash

### Dataset

SoMe benchmark dataset

Task:

```
user_behavior_prediction
```

---

### Sample Sizes

Two evaluation settings were used.

| Dataset Size | Purpose                     |
| ------------ | --------------------------- |
| 30 samples   | quick debugging and testing |
| 200 samples  | full evaluation             |

---

# Results

### 30 Sample Evaluation

| Method    | Accuracy |
| --------- | -------- |
| Baseline  | ~0.52    |
| History   | ~0.43    |
| Retrieval | ~0.47    |

---

### 200 Sample Evaluation

| Method    | Accuracy    |
| --------- | ----------- |
| Baseline  | **0.45226** |
| History   | 0.40000     |
| Retrieval | 0.32000     |

---

# Key Findings

1. The **baseline method performs the best** in our experiments.

2. Providing additional context does **not always improve prediction performance**.

3. Retrieval-based context may introduce **noise** when the retrieved posts are not strongly related to the user's preferences.

4. This suggests that **LLMs may rely more on post-level heuristics rather than modeling user behavior deeply**.

---

# Project Structure

```
project_root
│
├── data
│   ├── ground_truth.json
│   └── all_posts.json
│
├── outputs
│   ├── baseline_results.json
│   ├── history_results.json
│   └── retrieval_results.json
│
├── ubp_baseline.py
├── ubp_history.py
├── ubp_retrieval.py
└── evaluation.py
```

---

# Setup

Install dependencies

```
pip install openai
pip install tqdm
```

---

Set the API key in Colab or environment variables

```
export GEMINI_API_KEY=your_api_key
```

---

# Running Experiments

Run the baseline method

```
python run_baseline.py
```

Run the history method

```
python run_history.py
```

Run the retrieval method

```
python run_retrieval.py
```

---

# Limitations

Some limitations observed during the reproduction process:

* The dataset provides **very limited historical user behavior**
* Many users only have **one labeled interaction**
* This makes it difficult to truly model user preferences

Additionally, LLM predictions may rely heavily on **post content rather than user-level patterns**.

---

# Lessons Learned

During reproduction we observed several practical challenges:

1. dataset structure inconsistencies
2. missing files in the original repository
3. API timeouts during large-scale inference
4. handling long-running experiments

We implemented:

* timeout control
* intermediate result saving
* smaller debugging subsets

to make the experiments stable.

---

# Reference

SoMe Benchmark Paper

[https://arxiv.org/abs/2512.14720](https://arxiv.org/abs/2512.14720)


