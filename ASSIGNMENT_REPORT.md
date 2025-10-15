# Punctuation Restoration for Mental Health Conversations
## Assignment Report

---

## 1. Introduction

### 1.1 Problem Statement
Punctuation restoration is a Natural Language Processing (NLP) task that involves predicting and adding missing punctuation marks (such as periods, commas, question marks, and exclamation marks) to text that lacks proper punctuation. This task is crucial for enhancing readability and aiding in natural language understanding.

### 1.2 Objective
The goal of this assignment is to create a punctuation restoration system that:
- Takes unpunctuated text as input
- Accurately predicts and restores missing punctuation marks
- Forms coherent, properly punctuated sentences

### 1.3 Dataset
**Source:** [NLP Mental Health Conversations Dataset](https://www.kaggle.com/datasets/thedevastator/nlp-mental-health-conversations/data)

The dataset contains conversations between users and experienced psychologists related to mental health topics. As per the assignment requirements, we utilized the **Response column** (psychologist responses) to create our synthetic punctuation restoration dataset.

---

## 2. Methodology

### 2.1 Data Preprocessing

| Step | Action |
|------|--------|
| Text Cleaning | Remove special chars, normalize whitespace, filter <30 char texts |
| Missing Values | `dropna()` on Response column |
| Deduplication | Remove duplicate responses (3,508 → 2,479 unique) |
| Tokenization | NLTK `sent_tokenize()` for sentence-level splitting |
| Dataset Creation | Remove punctuation from input, keep original as output |
| Train/Val Split | 85/15 split, random seed 42 |

**Example:**
```
Input:  "i think you should talk to your therapist about this"
Output: "i think you should talk to your therapist about this."
```

### 2.2 Class Imbalance Problem

**Original Distribution (before oversampling):**
| Punctuation | Count | % of Total |
|-------------|-------|------------|
| Period (.) | 9,020 | 53.4% |
| Comma (,) | 6,752 | 40.0% |
| Question (?) | 729 | 4.3% |
| Exclamation (!) | 361 | **2.1%** |

**Problem:** Exclamation and question marks severely underrepresented → low recall for `!` and `?`

### 2.3 Solution: Oversampling Rare Punctuation

We duplicated sentences containing `!` and `?` to balance the dataset:

| Punctuation | Before | After | Change |
|-------------|--------|-------|--------|
| Period (.) | 9,020 | 11,195 | +24.1% |
| Comma (,) | 6,752 | 9,399 | +39.2% |
| Question (?) | 729 | 1,554 | **+113.2%** |
| Exclamation (!) | 361 | 2,542 | **+604.2%** |

**Implementation:**
- Oversampled `!` sentences: 344 × 7 = +2,064 samples
- Oversampled `?` sentences: 689 × 2 = +689 samples

**Note:** Period and comma counts also increased because sentences with `!` or `?` contain multiple punctuation types.

### 2.4 Final Dataset Statistics

| Metric | Value |
|--------|-------|
| Total Responses | 2,475 unique (after cleaning) |
| Total Sentence Pairs | 10,535 (after oversampling) |
| Training Samples | 8,954 |
| Validation Samples | 1,581 |
| Punctuation Types | 4 (. , ? !) |
| Avg Words/Response | 176.3 |
| Avg Sentences/Response | 8.4 |

### 2.5 Training Set Punctuation Distribution (After Oversampling)

| Punctuation | Count | % of Total |
|-------------|-------|------------|
| Period (.) | 9,532 | 45.5% |
| Comma (,) | 7,944 | 37.9% |
| Question (?) | 1,313 | 6.3% |
| Exclamation (!) | 2,165 | 10.3% |

---

## 3. Model Architecture

### 3.1 Why T5 Seq2Seq?

| Reason | Benefit |
|--------|--------|
| Text-to-Text Framework | Natural fit for punctuation restoration |
| Pre-trained Knowledge | Strong language understanding |
| Flexibility | Variable-length outputs, no alignment issues |
| Efficiency | T5-small (60M params) runs on Colab GPU |

### 3.2 Configuration

| Parameter | Value |
|-----------|-------|
| Model | T5-small (~60M params) |
| Max Length | 128 tokens |
| Task Prefix | "restore punctuation: " |
| Learning Rate | 3e-4 |
| Batch Size | 8 |
| Epochs | 4 |
| Optimizer | AdamW |
| Generation | Beam search (num_beams=2) |

---

## 4. Evaluation Metrics

| Category | Metrics |
|----------|---------|
| **Accuracy** | Character-level, Token-level, Exact Match Rate |
| **Sequence (ROUGE)** | ROUGE-1 (unigram), ROUGE-2 (bigram), ROUGE-L (LCS) |
| **Similarity** | Hamming Distance, Hamming Accuracy |
| **Per-Punctuation** | Precision, Recall, F1 for each (. , ? !) |
| **Aggregated** | Macro F1, Micro F1, Micro Precision, Micro Recall |

---

## 5. Results

### 5.1 Baseline vs Fine-tuned Performance

| Metric | Baseline | Fine-tuned | Improvement | % Change |
|--------|----------|------------|-------------|----------|
| Character Accuracy (%) | 14.21 | 79.04 | +64.84 | +456.3% |
| Token Accuracy (%) | 15.35 | 97.17 | +81.82 | +532.8% |
| Exact Match Rate (%) | 1.00 | 73.00 | +72.00 | +7200.0% |
| Hamming Accuracy (%) | 16.74 | 86.03 | +69.28 | +413.8% |
| ROUGE-1 | 0.5975 | 0.9972 | +0.3997 | +66.9% |
| ROUGE-2 | 0.5641 | 0.9958 | +0.4316 | +76.5% |
| ROUGE-L | 0.5929 | 0.9972 | +0.4043 | +68.2% |
| Macro F1 | 0.3359 | 0.9449 | +0.6090 | +181.3% |
| Micro F1 | 0.5126 | 0.9457 | +0.4331 | +84.5% |
| Micro Precision | 0.7625 | 0.9548 | +0.1923 | +25.2% |
| Micro Recall | 0.3861 | 0.9367 | +0.5506 | +142.6% |

### 5.2 Per-Punctuation F1 Scores

| Punctuation | Baseline F1 | Fine-tuned F1 | Improvement |
|-------------|-------------|---------------|-------------|
| Period (.) | 0.7820 | 0.9790 | +0.1971 |
| Comma (,) | 0.1000 | 0.8817 | +0.7817 |
| Question (?) | 0.4615 | 1.0000 | +0.5385 |
| Exclamation (!) | 0.0000 | 0.9189 | +0.9189 |

**Key Insight:** Oversampling dramatically improved `!` detection from F1=0.00 (baseline) to F1=0.92 (fine-tuned).

### 5.3 Sample Predictions (Fine-tuned Model)

| Input | Output |
|-------|--------|
| i think you should talk to your therapist about this | i think you should talk to your therapist about this. |
| how are you feeling today | how are you feeling today? |
| its important to take care of your mental health | its important to take care of your mental health. |
| have you tried meditation or mindfulness exercises | have you tried meditation or mindfulness exercises? |
| anxiety is a normal response but it can be overwhelming | anxiety is a normal response, but it can be overwhelming. |

---

## 6. EDA Summary

### 6.1 Raw Dataset Statistics
| Metric | Value |
|--------|-------|
| Total Responses | 3,508 |
| Unique Responses | 2,479 |
| Duplicates Removed | 1,029 |
| After Cleaning | 2,475 |
| Avg Words/Response | 176.3 |
| Avg Sentences/Response | 8.4 |
| Total Punctuation Marks | 44,300 |

### 6.2 Raw Punctuation Distribution
| Punctuation | Count | % |
|-------------|-------|---|
| Period (.) | 23,878 | 53.9% |
| Comma (,) | 17,346 | 39.2% |
| Question (?) | 2,105 | 4.8% |
| Exclamation (!) | 971 | 2.2% |

---

## 7. Alternative Approach Considered

### Partial Punctuation Masking

**Idea:** Instead of removing all punctuation, randomly mask only some punctuation marks and train the model to predict the masked ones.

**Potential Benefits:**
- Model learns from context of existing punctuation
- More realistic scenario (some text may have partial punctuation)
- Could improve accuracy for ambiguous cases

**Why We Didn't Implement It:**

The problem statement explicitly states:
> *"takes unpunctuated text as input and accurately predicts and restores the missing punctuation marks"*

This requires handling **fully unpunctuated** text, not partially punctuated. Therefore, we followed the specification and removed all punctuation from inputs.

---

## 8. Challenges and Solutions

| Challenge | Problem | Solution |
|-----------|---------|----------|
| **Long Responses** | Avg 176 words/response, exceed 128 token limit | Sentence-level splitting |
| **Class Imbalance** | `!` only 2.1% of data, F1=0.00 baseline | **Oversampling** (7x for `!`, 2x for `?`) |
| **Ambiguous Placement** | Multiple valid options (. vs ?) | T5's bidirectional context |
| **Domain-Specific Language** | Mental health terminology | Fine-tuning on domain data |
| **Evaluation Complexity** | Simple accuracy insufficient | 11 metrics (ROUGE, Hamming, F1) |

---

## 9. Algorithm Details

### Dataset Creation
```
1. Split texts into sentences (NLTK sent_tokenize)
2. For each sentence: remove punctuation → input, original → output
3. Track sentences with ! and ?
4. Oversample: ! sentences (7x), ? sentences (2x)
5. Shuffle dataset
```

### Evaluation
```
1. Generate predictions using beam search
2. Calculate: char accuracy, token accuracy, exact match
3. Compute ROUGE-1/2/L and Hamming distance
4. Track TP/FP/FN for each punctuation type
5. Calculate precision, recall, F1 per punctuation
6. Compute macro/micro averaged metrics
```

### Inference
```
1. Prepend "restore punctuation: " to input
2. Tokenize and pass to model
3. Generate with beam search (num_beams=2)
4. Decode and return punctuated text
```

---

## 10. Design Choices

| Choice | Why |
|--------|-----|
| **T5 over BERT** | Seq2Seq handles variable output naturally, no token alignment |
| **T5-small** | Efficient for Colab GPU, 60M params, good performance |
| **Sentence-level** | Avoids truncation (avg 176 words/response too long) |
| **Oversampling** | `!` F1 improved from 0.00 → 0.92, `?` from 0.46 → 1.00 |
| **Beam search** | Better generation quality than greedy decoding |

---

## 11. Conclusions

### 11.1 Achievements

| Requirement | Status | Result |
|-------------|--------|--------|
| Dataset Creation | ✅ | 10,535 sentence pairs |
| Model Implementation | ✅ | T5-small Seq2Seq |
| Comprehensive Evaluation | ✅ | 11 metrics computed |
| Fine-tuning Success | ✅ | 73% exact match rate |
| Class Imbalance Handling | ✅ | `!` F1: 0.00 → 0.92 |

### 11.2 Key Takeaways

1. **Oversampling** dramatically improved rare punctuation detection (`!` from 0.00 to 0.92 F1)
2. **T5 Seq2Seq** achieves 97.17% token accuracy after fine-tuning
3. **Domain-specific fine-tuning** improved exact match from 1% to 73%
4. **Sentence-level processing** avoids truncation of long responses (avg 176 words)

### 11.3 Future Work

- Larger models (T5-base/large)
- Additional punctuation types (semicolons, colons)
- Partial masking approach for different use cases
- API deployment for real-time use

---

## 12. References

1. Raffel, C., et al. (2020). "Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer." JMLR.

2. Kaggle Dataset: NLP Mental Health Conversations
   https://www.kaggle.com/datasets/thedevastator/nlp-mental-health-conversations/data

3. Hugging Face Transformers Library
   https://huggingface.co/transformers/

4. ROUGE Score Implementation
   https://github.com/google-research/google-research/tree/master/rouge

---

## 13. Appendix

### A. Code Structure

```
punctuation_restoration.ipynb
├── 1. Setup & Installation
├── 2. Dataset Acquisition
├── 3. Dataset Loading & Understanding
├── 4. Data Preprocessing & Quality Analysis
├── 5. Comprehensive EDA
├── 6. Sentence-Level Dataset Creation (with Oversampling)
├── 7. Train/Validation Split
├── 8. T5 Seq2Seq Approach (Model + Metrics Functions)
├── 9. Baseline Evaluation (Pre-trained T5)
├── 10. Fine-tuning on Mental Health Domain
├── 11. Fine-tuned Model Evaluation
├── 12. Comprehensive Model Comparison
├── 13. Save Fine-tuned Model
└── 14. Interactive Testing
```

### B. Dependencies

```
transformers>=4.30.0
torch>=2.0.0
datasets>=2.12.0
pandas>=1.5.0
numpy>=1.23.0
matplotlib>=3.7.0
seaborn>=0.12.0
scikit-learn>=1.2.0
nltk>=3.8.0
rouge-score>=0.1.2
sentencepiece>=0.1.99
```

### C. Model Persistence

The fine-tuned model is saved to:
- Local path: `./punctuation_t5/final_model/`
- Download: `punctuation_t5_finetuned_model.zip`

---

**Report Prepared By:** Satvik Jain (22B0635)  
**Date:** November 2025  
**Platform:** Google Colab  
**Assignment Duration:** 3 Days
