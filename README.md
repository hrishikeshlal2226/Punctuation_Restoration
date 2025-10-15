# Punctuation Restoration

Punctuation Restoration is a small NLP project that restores missing punctuation in informal text using a transformer-based sequence-to-sequence model. The focus is mental-health conversation data, where punctuation can improve readability, flow, and downstream understanding.

## What it does

- Builds sentence-level training examples from psychologist responses in a Kaggle mental-health conversations dataset
- Compares a baseline model against a fine-tuned model
- Evaluates results with accuracy, ROUGE, Hamming distance, and per-punctuation F1 scores
- Generates sample predictions and exploratory analysis in the notebook

## Repository contents

- [punctuation_restoration.ipynb](punctuation_restoration.ipynb) - main notebook
- [README.md](README.md) - project overview and run instructions
- [requirements.txt](requirements.txt) - Python dependencies

## Dataset

Source: [NLP Mental Health Conversations Dataset](https://www.kaggle.com/datasets/thedevastator/nlp-mental-health-conversations/data)

The notebook uses the `Response` column to create synthetic pairs:
- input: unpunctuated, lowercased sentence
- target: original punctuated sentence

Supported punctuation marks:
- period `.`
- comma `,`
- question mark `?`
- exclamation mark `!`

## Model and evaluation

The notebook uses a T5-small text-to-text setup for punctuation restoration.

Training setup:
- optimizer: AdamW
- learning rate: 3e-4
- batch size: 8
- epochs: 4
- max length: 128 tokens

Metrics reported:
- character accuracy
- token accuracy
- exact match rate
- ROUGE-1, ROUGE-2, ROUGE-L
- Hamming distance
- precision, recall, and F1 per punctuation mark
- macro and micro F1

## How to run

The notebook is designed for Google Colab.

1. Open [punctuation_restoration.ipynb](punctuation_restoration.ipynb) in Colab.
2. Upload your `kaggle.json` file when prompted.
3. Run the cells from top to bottom.
4. Review the generated charts, metrics, and sample outputs.

Local install:

```bash
pip install -r requirements.txt
```

Recommended environment:
- Python 3.8 or newer
- GPU runtime if available
- Kaggle API credentials

## Outputs

When the notebook finishes, it saves and packages the fine-tuned model locally and also downloads a zip file for reuse.

Typical outputs:
- `./punctuation_t5/final_model/`
- `punctuation_t5_finetuned_model.zip`
- notebook charts and prediction examples

## Notes

This repository is intentionally minimal: the notebook is the main artifact, and the README is the main entry point.
