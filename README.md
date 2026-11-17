# Medical Record Summarisation

A notebook-based NLP project that experiments with generating concise summaries from clinical notes. It compares a simple extractive baseline with pre-trained and fine-tuned sequence-to-sequence summarisation.

## Approach
- Load the `AGBonnet/augmented-clinical-notes` dataset
- Explore note and summary lengths and compression ratios
- Clean text and create reproducible train/validation/test splits
- Build a TF-IDF sentence-selection baseline
- Generate summaries using pre-trained `facebook/bart-large-cnn`
- Fine-tune BART-large-CNN and evaluate generated summaries

## Tech Stack
Python · PyTorch · Hugging Face Transformers · Datasets · scikit-learn · NLTK · ROUGE · BERTScore · Matplotlib · Seaborn

## Run
The experiment is provided as a Kaggle-compatible Jupyter notebook.

1. Open `NLP - Medical Record Summarisation/medical record summarisation.ipynb` in Kaggle or Jupyter.
2. Install the packages listed in the notebook.
3. Run the cells in order. A CUDA-enabled GPU is recommended for fine-tuning.

The notebook downloads the dataset and pretrained model from their respective Hugging Face resources, so internet access is required.

## Evaluation
The notebook calculates ROUGE-1, ROUGE-2, ROUGE-L, BERTScore, and METEOR for the summarisation approaches. It also discusses clinical entity preservation and factual consistency as relevant evaluation considerations.

## Repository Contents
- `NLP - Medical Record Summarisation/medical record summarisation.ipynb` – End-to-end experiment
- `NLP - Medical Record Summarisation/medical_nlp_paper.docx` – Project paper

> **Disclaimer:** This is an NLP research/learning project. Generated summaries require clinical review and must not be used as a standalone basis for medical decisions.
