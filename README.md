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
- **Synthetic Dataset Creation** (Sentence-level):
  - Input: Sentence with punctuation removed (lowercase)
  - Output: Original punctuated sentence
  - Sentence-level processing avoids truncation issues
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

**Sequence-to-Sequence (Seq2Seq) Approach with T5**:
- Base Model: T5-small (~60M parameters)
- Task: Text-to-text generation ("restore punctuation: [input]" → "[punctuated output]")
- Sentence-level processing to avoid truncation issues
- Punctuation types handled: Period (.), Comma (,), Question (?), Exclamation (!)

**Two Model Variants**:
1. **Baseline**: Pre-trained T5 without domain fine-tuning
2. **Fine-tuned**: T5 trained on mental health conversations

### 3. Training Configuration

- **Optimizer**: AdamW
- **Learning Rate**: 3e-4
- **Batch Size**: 8
- **Epochs**: 4
- **Max Length**: 128 tokens
- **Loss Function**: Cross-entropy (Seq2Seq)
- **Strategy**: Load best model at end

### 4. Evaluation Metrics

**Comprehensive Evaluation Suite:**
- **Character-level Accuracy**: Percentage of characters correctly predicted
- **Token-level Accuracy**: Percentage of tokens with correct punctuation
- **Exact Match Rate**: Percentage of perfectly restored sentences
- **ROUGE Scores**: ROUGE-1, ROUGE-2, ROUGE-L for n-gram overlap
- **Hamming Distance**: Positional differences in character sequences
- **Per-punctuation Metrics**: Precision, Recall, F1 for each mark (. , ? !)
- **Macro/Micro F1**: Averaged performance metrics
- **Qualitative Analysis**: Sample predictions with ground truth comparison

## 🚀 Usage

### Running in Google Colab

1. Open the notebook: `punctuation_restoration.ipynb`
2. Upload your Kaggle API credentials (`kaggle.json`)
3. Run all cells sequentially
4. Review results and visualizations
5. **Model Download**: The trained model is automatically saved and downloaded as a zip file at the end

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
- **Comprehensive Baseline vs. Fine-tuned comparison** with all metrics
- Character, Token, and Exact Match accuracy
- ROUGE-1, ROUGE-2, ROUGE-L sequence-level metrics
- Hamming distance for sequence similarity
- Per-punctuation F1 scores (. , ? !)
- Macro and Micro averaged precision, recall, F1
- Training curves and loss plots
- Sample predictions with ground truth
- **Model persistence**: Trained model saved and downloadable for future use

### Key Findings
- Effectiveness of fine-tuning on domain-specific text
- Challenges in punctuation restoration
- Areas for improvement

## 🔧 Technical Choices

### Why Seq2Seq (T5)?
- **Natural Generation**: Direct text-to-text transformation
- **Flexibility**: Handles multiple punctuation marks per sentence
- **Context**: Encoder-decoder captures full sentence context
- **Simplicity**: No token alignment issues

### Why T5 Seq2Seq?
- **Text-to-Text**: Natural fit for punctuation restoration
- **Flexibility**: Handles variable length outputs naturally
- **Pre-trained**: Strong language understanding from large corpora
- **Efficiency**: T5-small balances performance and resource usage
- **Fine-tuning**: Excellent adaptation to domain-specific tasks

### Language Model Integration
- **Internal**: T5 encoder-decoder provides contextual understanding
- **External**: Option to use beam search for better generation quality

## 🎓 Challenges & Solutions

### Challenge 1: Ambiguous Punctuation Placement
- **Problem**: Multiple valid punctuation positions
- **Solution**: T5's encoder-decoder with attention captures semantic context

### Challenge 2: Long Responses
- **Problem**: Full responses may exceed model's max length
- **Solution**: Sentence-level splitting for balanced examples without truncation

### Challenge 3: Domain-Specific Language
- **Problem**: Mental health terminology differs from general text
- **Solution**: Fine-tuning on domain-specific conversations

### Challenge 4: Evaluation Complexity
- **Problem**: Need comprehensive metrics beyond simple accuracy
- **Solution**: Multi-metric evaluation (ROUGE, Hamming, F1, etc.)

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

After execution, the trained model is saved to:
- Local: ./punctuation_t5/final_model/
- Download: punctuation_t5_finetuned_model.zip (automatically downloaded)
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

## 💾 Model Persistence

After training completes, the fine-tuned model is automatically:

1. **Saved locally** to `./punctuation_t5/final_model/` directory
2. **Downloaded as zip** file (`punctuation_t5_finetuned_model.zip`) to your local machine
3. **Optional Google Drive save** - Code provided for saving to Google Drive

### Reusing the Model

To reuse the trained model in future projects:

```python
from transformers import T5Tokenizer, T5ForConditionalGeneration

# Load the saved model
model_path = "./punctuation_t5_finetuned_model"  # After extracting zip
tokenizer = T5Tokenizer.from_pretrained(model_path)
model = T5ForConditionalGeneration.from_pretrained(model_path)

# Use for punctuation restoration
input_text = "restore punctuation: your text here"
inputs = tokenizer(input_text, return_tensors='pt', max_length=128, truncation=True)
outputs = model.generate(**inputs, max_length=128)
result = tokenizer.decode(outputs[0], skip_special_tokens=True)
```

## 🙏 Acknowledgments

- Dataset: Kaggle NLP Mental Health Conversations
- Framework: Hugging Face Transformers
- Platform: Google Colab

---

**Note**: This project is completed as part of an NLP assignment focused on punctuation restoration in domain-specific text.
