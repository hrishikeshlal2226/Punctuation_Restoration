# Canva Presentation Slides Content
## Punctuation Restoration for Mental Health Conversations

---

## 📌 SLIDE 1: Title Slide

**Title:** Punctuation Restoration for Mental Health Conversations

**Subtitle:** Using T5 Transformer for Domain-Specific NLP

**Author:** Satvik Jain (22B0635)

**Date:** November 2025

**Visual Suggestion:** Use a clean, professional background with NLP/AI themed graphics

---

## 📌 SLIDE 2: Problem Statement & Objective

### 🎯 Problem Statement
Punctuation restoration is the task of predicting and adding missing punctuation marks (periods, commas, question marks, exclamation marks) to unpunctuated text.

### 🎯 Objectives
- ✅ Create synthetic dataset from mental health conversations
- ✅ Implement T5 Seq2Seq model for punctuation restoration
- ✅ Compare baseline vs fine-tuned model performance
- ✅ Evaluate effectiveness of domain-specific fine-tuning

### 📊 Dataset
**Source:** Kaggle NLP Mental Health Conversations
- 3,508 responses → 2,475 unique after cleaning
- Avg 176 words/response, 8.4 sentences/response
- Used "Response" column (psychologist responses)

**Visual Suggestion:** Use icons for each objective, show dataset source logo

---

## 📌 SLIDE 3: Approach & Methodology

### 🔧 Data Pipeline
```
Raw Text → Sentence Splitting → Punctuation Removal → Oversampling → Train/Val Split
```

### 🏗️ Model Architecture
| Component | Choice |
|-----------|--------|
| Model | T5-small (~60M params) |
| Approach | Seq2Seq (Text-to-Text) |
| Task Format | "restore punctuation: [input]" → "[output]" |
| Training | 4 epochs, lr=3e-4, batch=8 |

### ⚖️ Class Imbalance Solution: Oversampling
| Punctuation | Before | After | Change |
|-------------|--------|-------|--------|
| Period (.) | 9,020 | 11,195 | +24.1% |
| Comma (,) | 6,752 | 9,399 | +39.2% |
| Question (?) | 729 | 1,554 | **+113.2%** |
| Exclamation (!) | 361 | 2,542 | **+604.2%** |

**Visual Suggestion:** Flowchart with oversampling step highlighted, before/after bar chart

---

## 📌 SLIDE 4: Comprehensive Evaluation Metrics

### 📊 Metrics Used

| Category | Metrics |
|----------|---------|
| **Accuracy** | Character-level, Token-level, Exact Match |
| **Sequence** | ROUGE-1, ROUGE-2, ROUGE-L |
| **Similarity** | Hamming Distance |
| **Per-Punctuation** | Precision, Recall, F1 for (. , ? !) |
| **Aggregated** | Macro F1, Micro F1 |

### 🔍 Why These Metrics?
- **ROUGE:** Measures n-gram overlap between prediction and target
- **Hamming:** Measures character-level sequence differences
- **Per-Punctuation F1:** Shows model performance on each mark type
- **Macro/Micro:** Provides balanced and weighted averages

**Visual Suggestion:** Icons for each metric category, simple formulas if space allows

---

## 📌 SLIDE 5: Results & Comparison

### 📊 Baseline vs Fine-tuned Performance

| Metric | Baseline | Fine-tuned | % Change |
|--------|----------|------------|----------|
| Character Accuracy | 14.21% | 79.04% | +456% |
| Token Accuracy | 15.35% | 97.17% | +533% |
| Exact Match Rate | 1.00% | 73.00% | +7200% |
| ROUGE-1 | 0.5975 | 0.9972 | +67% |
| Macro F1 | 0.3359 | 0.9449 | +181% |

### 📊 Per-Punctuation F1 Improvement
| Punct | Baseline | Fine-tuned | Improvement |
|-------|----------|------------|-------------|
| . | 0.7820 | 0.9790 | +0.1971 |
| , | 0.1000 | 0.8817 | +0.7817 |
| ? | 0.4615 | 1.0000 | +0.5385 |
| ! | 0.0000 | 0.9189 | **+0.9189** |

### 📌 Key Finding
**Oversampling fixed `!` detection: F1 improved from 0.00 → 0.92**
- `!` sentences: 344 × 7 = +2,064 samples
- `?` sentences: 689 × 2 = +689 samples

**Visual Suggestion:** Bar chart comparing baseline vs fine-tuned, highlight '!' improvement

---

## 📌 SLIDE 6: Sample Predictions & Conclusions

### 📝 Sample Predictions (Fine-tuned Model)

| Input | Output |
|-------|--------|
| i think you should talk to your therapist about this | i think you should talk to your therapist about this. |
| how are you feeling today | how are you feeling today? |
| have you tried meditation or mindfulness exercises | have you tried meditation or mindfulness exercises? |
| anxiety is a normal response but it can be overwhelming | anxiety is a normal response, but it can be overwhelming. |

### 🎯 Key Takeaways
1. **T5 Seq2Seq** achieves 97.17% token accuracy, 73% exact match
2. **Oversampling** fixed `!` detection (F1: 0.00 → 0.92)
3. **Fine-tuning** improved exact match from 1% to 73%
4. **Sentence-level processing** handles avg 176 words/response

### 💡 Alternative Considered
- **Partial masking**: Mask some punctuation, predict rest
- **Why not used**: Problem statement required fully unpunctuated input

### 🚀 Future Work
- Larger models (T5-base), more punctuation types, API deployment

**Visual Suggestion:** Show sample predictions in a clean table format, conclusion icons
