# Medical Record Summarisation

This project explores automatic summarisation of clinical notes using NLP. The notebook compares a TF-IDF extractive baseline, a pre-trained BART model, and a BART model fine-tuned on clinical-note data.

> This is a research/learning project. Generated summaries are not clinical advice and must not be used for medical decisions without qualified review.

## Dataset and Experiment

The notebook loads **`AGBonnet/augmented-clinical-notes`** from Hugging Face Datasets. It cleans the note/summary text, explores note lengths and compression ratios, and uses a reproducible 5,000-example sample.

| Split | Examples |
|---|---:|
| Training | 3,500 |
| Validation | 750 |
| Test | 750 |

The notebook runs the final metric comparison on a 50-example subset of the test split.

## Models Compared

1. **TF-IDF extractive baseline** — ranks sentences using TF-IDF similarity and selects the top five in their original order.
2. **Pre-trained BART** — `facebook/bart-large-cnn` used for summarisation without project-specific fine-tuning.
3. **Fine-tuned BART** — the same BART-large-CNN checkpoint fine-tuned on the sampled clinical notes and reference summaries.

### Fine-tuning Configuration

| Setting | Value |
|---|---|
| Maximum input / target length | 512 / 384 tokens |
| Training epochs | Up to 5, with early stopping (patience 2) |
| Learning rate | `3e-5` |
| Scheduler | Cosine with warm-up |
| Per-device batch size | 2 |
| Gradient accumulation | 4 (effective batch size 8) |
| Label smoothing | 0.1 |
| Inference decoding | Beam search, 4 beams |

Training uses mixed precision when CUDA is available.

## Evaluation

The notebook evaluates the three approaches using ROUGE, BERTScore, METEOR, medical-entity preservation, a custom factual-consistency score, and MEDCON.

<details>
<summary>Recorded results (50 test examples)</summary>

| Metric | TF-IDF | Pre-trained BART | Fine-tuned BART |
|---|---:|---:|---:|
| ROUGE-1 | 0.2918 | 0.1926 | **0.4922** |
| ROUGE-2 | 0.1751 | 0.1169 | **0.3381** |
| ROUGE-L | 0.1936 | 0.1384 | **0.3470** |
| BERTScore-F1 | 0.7291 | 0.7128 | **0.9187** |
| METEOR | 0.0684 | 0.0395 | **0.2546** |
| NER preservation | 0.2459 | 0.1814 | **0.5818** |
| Factual consistency | **1.0000** | **0.9995** | 0.5802 |
| MEDCON | 0.5143 | 0.4268 | **0.6531** |

</details>

The fine-tuned model leads on the overlap/semantic metrics and the notebook's entity-preservation and MEDCON scores. Its recorded factual-consistency score is lower than the two baselines, so these results should not be interpreted as proof that the generated summaries are factually reliable. The notebook run also reports that SciSpaCy was unavailable for medical NER and used a keyword-based fallback.

## Run the Notebook

The main experiment is:

`NLP - Medical Record Summarisation/medical record summarisation.ipynb`

Open it in Kaggle or Jupyter and execute the cells in order. The notebook installs its experiment dependencies, downloads the dataset and model, and saves plots such as `eda_plots.png`, `training_loss_curve.png`, and metric-comparison charts in the notebook working directory.

A CUDA-enabled GPU is recommended for fine-tuning; CPU execution may be significantly slower.

## Repository Contents

- `NLP - Medical Record Summarisation/medical record summarisation.ipynb` — data analysis, preprocessing, training, inference, and evaluation.
- `NLP - Medical Record Summarisation/medical_nlp_paper.docx` — accompanying project paper.

## Technology Stack

Python · PyTorch · Hugging Face Transformers · Hugging Face Datasets · scikit-learn · NLTK · ROUGE · BERTScore · Matplotlib · Seaborn
