# Quick Start Guide - Punctuation Restoration

## 🚀 Running the Notebook

### Step 1: Open in Google Colab

1. Go to [Google Colab](https://colab.research.google.com/)
2. Click **File → Upload notebook**
3. Upload `punctuation_restoration.ipynb`

### Step 2: Set Up Kaggle API

1. Go to [Kaggle Account Settings](https://www.kaggle.com/settings)
2. Scroll to **API** section
3. Click **Create New API Token**
4. Download `kaggle.json` file

### Step 3: Run the Notebook

1. **Run Cell 1**: Install dependencies (takes ~2 minutes)
2. **Run Cell 2**: Import libraries
3. **Run Cell 3**: Upload `kaggle.json` when prompted
4. **Run All Remaining Cells**: Use Runtime → Run all

### Step 4: Review Results

The notebook will:
- ✅ Download and load the mental health dataset
- ✅ Perform exploratory data analysis with visualizations
- ✅ Create synthetic punctuation dataset
- ✅ Train baseline and fine-tuned models
- ✅ Compare performance with metrics and charts
- ✅ Show sample predictions
- ✅ **Save and download the trained model** automatically

## ⏱️ Expected Runtime

- **Full execution**: 30-60 minutes
- **Dataset download**: 2-5 minutes
- **Model training**: 15-30 minutes (with GPU)
- **GPU recommended**: Yes (Runtime → Change runtime type → GPU)

## 📊 What You'll Get

### Visualizations
- Text length distribution
- Punctuation frequency charts
- Label distribution in training data
- Model performance comparison
- Training metrics

### Models
- Baseline (pre-trained) model evaluation
- Fine-tuned model on mental health domain
- Performance metrics comparison

### Analysis
- Dataset statistics
- Token-level accuracy
- Per-punctuation-type F1 scores
- Sample predictions

## 🔧 Troubleshooting

### Kaggle API Error
**Issue**: "Kaggle credentials not found"  
**Solution**: Make sure you uploaded `kaggle.json` in Cell 3

### Out of Memory
**Issue**: "CUDA out of memory"  
**Solution**: 
- Reduce batch size in training arguments (line in Cell 9)
- Use fewer training examples by reducing the dataset size

### Dataset Not Found
**Issue**: "File not found: mental_health_conversations.csv"  
**Solution**: 
- Check the actual filename after unzipping
- Update the filename in the code if it differs

## 📝 Customization

### Change Model
Replace `model_name` in Cell 7:
```python
model_name = "bert-base-uncased"  # or "roberta-base"
```

### Adjust Training
Modify `training_args` in Cell 9:
```python
num_train_epochs=5,  # More epochs
learning_rate=3e-5,  # Different learning rate
```

### Use More Data
Change dataset size in Cell 4:
```python
texts = texts[:10000]  # Use more texts
```

## ✅ Verification

After running, verify:
- [x] No errors in any cell
- [x] Visualizations display correctly (EDA + 4-panel comparison charts)
- [x] **Comprehensive metrics displayed** for both baseline and fine-tuned models:
  - Character/Token/Exact Match Accuracy
  - ROUGE-1, ROUGE-2, ROUGE-L scores
  - Hamming Distance & Accuracy
  - Per-punctuation F1 scores
  - Macro/Micro averaged metrics
- [x] Fine-tuned model metrics > Baseline metrics across all measures
- [x] EDA on test results shows prediction analysis
- [x] Sample predictions show punctuation restoration
- [x] **Model zip file downloaded** to your computer (`punctuation_t5_finetuned_model.zip`)
- [x] Model saved locally in `./punctuation_t5/final_model/`

## 📁 Outputs

The notebook generates:
- `./punctuation_t5/` - Saved model checkpoints during training
- `./punctuation_t5/final_model/` - Final trained model (saved locally)
- `punctuation_t5_finetuned_model.zip` - **Downloaded model file** (saved to your computer)
- Inline visualizations
- Performance metrics
- Sample predictions

## 💾 Model Download & Reuse

After training completes, the model is automatically:

1. **Saved locally** to `./punctuation_t5/final_model/`
2. **Downloaded as zip** - The file `punctuation_t5_finetuned_model.zip` will automatically download to your computer
3. **Ready to reuse** - Extract the zip and load the model in future projects

### Using the Saved Model

After downloading, you can reuse the model:

```python
# 1. Extract the downloaded zip file
# 2. Load the model in your code:

from transformers import T5Tokenizer, T5ForConditionalGeneration

model_path = "./punctuation_t5_finetuned_model"  # Path after extraction
tokenizer = T5Tokenizer.from_pretrained(model_path)
model = T5ForConditionalGeneration.from_pretrained(model_path)

# 3. Use for predictions:
input_text = "restore punctuation: how are you feeling today"
inputs = tokenizer(input_text, return_tensors='pt', max_length=128, truncation=True)
outputs = model.generate(**inputs, max_length=128)
result = tokenizer.decode(outputs[0], skip_special_tokens=True)
print(result)  # Output: "how are you feeling today?"
```

### Alternative: Save to Google Drive

If you prefer cloud storage, uncomment the Google Drive save code in the notebook's final cells.

## 🎯 Assignment Deliverables

This notebook satisfies all requirements:
1. ✅ Dataset creation and understanding
2. ✅ Data preprocessing
3. ✅ Model training and fine-tuning
4. ✅ Baseline vs fine-tuned comparison
5. ✅ EDA on train data and results
6. ✅ Complete documentation
7. ✅ Challenges and solutions explained

## 💡 Tips

- **Save your work**: File → Save a copy in Drive
- **Share results**: File → Download .ipynb or share Colab link
- **GPU access**: Essential for faster training
- **Run sequentially**: Don't skip cells

## 📞 Support

If you encounter issues:
1. Check error messages in the cell output
2. Verify Kaggle credentials are uploaded
3. Ensure GPU runtime is enabled
4. Try restarting runtime and running again

---

**Ready to start?** Open the notebook and follow the steps above! 🎉
