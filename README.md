# Punctuation Restoration for Mental Health Conversations

## 📋 Project Overview

This project implements an end-to-end punctuation restoration system for mental health conversations using transformer-based language models. The system takes unpunctuated text as input and accurately predicts and restores missing punctuation marks to enhance readability and natural language understanding.

## 🎯 Objectives

1. Create a synthetic punctuation restoration dataset from mental health conversations
2. Implement and train punctuation restoration models
3. Compare baseline (pre-trained) vs. fine-tuned models on domain-specific data
4. Evaluate effectiveness of fine-tuning for domain-specific NLP tasks
5. Perform comprehensive EDA and results analysis

## 📊 Dataset

**Source**: [NLP Mental Health Conversations Dataset](https://www.kaggle.com/datasets/thedevastator/nlp-mental-health-conversations/data)

### Dataset Structure

- **Original Dataset**: Conversations between users and psychologists
- **Key Column**: `Response` - contains psychologist responses with proper punctuation
- **Synthetic Dataset Creation**:
  - Input: Text with punctuation removed
  - Labels: Token-level punctuation predictions
  - Punctuation types: Period (.), Comma (,), Question (?), Exclamation (!)

### Dataset Statistics

The dataset will be analyzed for:
- Total number of conversations
- Average response length
- Punctuation distribution
- Vocabulary size
- Domain-specific terminology

## 🏗️ Methodology

### 1. Data Preprocessing

- **Text Cleaning**: Remove special characters, normalize whitespace
- **Tokenization**: WordPiece/BPE tokenization for transformer models
- **Label Creation**: Extract punctuation positions and create token-level labels
- **Train/Validation Split**: 80/20 split with stratification

### 2. Model Architecture

**Token Classification Approach**:
- Base Model: BERT/RoBERTa/DistilBERT
- Task: Multi-class classification for each token
- Output Classes: `O` (no punctuation), `PERIOD`, `COMMA`, `QUESTION`, `EXCLAMATION`

**Two Model Variants**:
1. **Baseline**: Pre-trained model without fine-tuning
2. **Fine-tuned**: Model trained on mental health conversations

### 3. Training Configuration

- **Optimizer**: AdamW
- **Learning Rate**: 2e-5 to 5e-5
- **Batch Size**: 8-16 (depending on GPU memory)
- **Epochs**: 3-5
- **Loss Function**: Cross-entropy loss

### 4. Evaluation Metrics

- **Token-level Accuracy**: Overall punctuation prediction accuracy
- **Per-class Metrics**: Precision, Recall, F1 for each punctuation type
- **Confusion Matrix**: Visualization of prediction patterns
- **Qualitative Analysis**: Sample predictions with human evaluation

## 🚀 Usage

### Running in Google Colab

1. Open the notebook: `punctuation_restoration.ipynb`
2. Upload your Kaggle API credentials (`kaggle.json`)
3. Run all cells sequentially
4. Review results and visualizations

### Local Setup

```bash
# Install dependencies
pip install -r requirements.txt

# Note: For local execution, ensure you have:
# - Python 3.8+
# - CUDA-capable GPU (recommended)
# - Kaggle API credentials configured
```

## 📈 Results

The notebook includes comprehensive analysis:

### Exploratory Data Analysis
- Token distribution visualizations
- Punctuation frequency analysis
- Sentence length statistics
- Domain-specific vocabulary insights

### Model Performance
- Baseline vs. Fine-tuned comparison
- Per-punctuation-type metrics
- Training curves and loss plots
- Sample predictions with ground truth

### Key Findings
- Effectiveness of fine-tuning on domain-specific text
- Challenges in punctuation restoration
- Areas for improvement

## 🔧 Technical Choices

### Why Token Classification?
- **Interpretability**: Clear understanding of which punctuation follows each token
- **Efficiency**: Direct prediction without sequence generation overhead
- **Evaluation**: Easier to compute token-level metrics

### Why BERT/RoBERTa?
- **Pre-trained Knowledge**: Strong language understanding from large corpora
- **Bidirectional Context**: Captures context from both directions
- **Fine-tuning Capability**: Excellent performance on downstream tasks

### Language Model Integration
- **Internal**: Transformer layers provide contextual embeddings
- **External**: Option to use external LM for perplexity-based reranking

## 🎓 Challenges & Solutions

### Challenge 1: Ambiguous Punctuation Placement
- **Problem**: Multiple valid punctuation positions
- **Solution**: Use context window and semantic understanding from fine-tuning

### Challenge 2: Class Imbalance
- **Problem**: Periods more common than question marks
- **Solution**: Class weighting and balanced sampling

### Challenge 3: Domain-Specific Language
- **Problem**: Mental health terminology differs from general text
- **Solution**: Fine-tuning on domain-specific conversations

### Challenge 4: Long Context Dependencies
- **Problem**: Some punctuation requires long-range context
- **Solution**: Use models with larger context windows (512 tokens)

## 📁 Project Structure

```
Augnito/
├── punctuation_restoration.ipynb   # Main Colab notebook
├── README.md                        # This file
├── requirements.txt                 # Python dependencies
├── problem_statement.md             # Original assignment
└── results/                         # Generated results (created during execution)
    ├── figures/                     # Visualizations
    ├── models/                      # Saved model checkpoints
    └── predictions/                 # Sample outputs
```

## 🔍 Code Documentation

The Jupyter notebook is extensively documented with:
- **Markdown cells**: Explaining each section's purpose
- **Code comments**: Rationale for implementation choices
- **Inline explanations**: Complex algorithm details
- **Visualization captions**: Interpreting results

## 📝 Assignment Compliance

This project addresses all assignment requirements:

✅ **Dataset Creation**: Synthetic dataset from mental health conversations  
✅ **Dataset Understanding**: Detailed structure description  
✅ **Data Preprocessing**: Cleaning, tokenization, splitting  
✅ **Training**: Fine-tuning on domain-specific data  
✅ **Language Model**: Integrated transformer architecture  
✅ **Comparison**: Baseline vs. fine-tuned evaluation  
✅ **EDA**: Comprehensive data and results analysis  
✅ **Documentation**: Code, algorithms, and reasoning explained  

## 🎬 Presentation

The notebook provides a complete walkthrough of:
1. **Approach**: Step-by-step methodology
2. **Challenges**: Issues encountered and solutions
3. **Results**: Quantitative and qualitative findings
4. **Insights**: Key takeaways and future improvements

## 📞 Platform

Designed for: **Google Colab** ([https://colab.google/](https://colab.google/))

## ⏱️ Time Duration

**Assignment Duration**: 3 days  
**Estimated Execution Time**: 2-3 hours (including training)

## 🙏 Acknowledgments

- Dataset: Kaggle NLP Mental Health Conversations
- Framework: Hugging Face Transformers
- Platform: Google Colab

---

**Note**: This project is completed as part of an NLP assignment focused on punctuation restoration in domain-specific text.
