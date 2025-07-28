# A Comparative Study of Fine-Tuning Scenarios for Transformer Models in Indonesian Sentiment Analysis

This repository contains the code and notebooks for a research project (mainly for KP). Full report can be accessed [here](https://repository.its.ac.id/120632)
 
## Abstract

This research conducts a comparative study to evaluate three fine-tuning scenarios—Standard Fine-Tuning, Gradual Unfreezing, and Differential Learning Rates—across three model architectures: IndoBERT-base, IndoBERTweet, and RoBERTa. The experiments were performed on two datasets from different domains: app reviews (BBM Dataset) and political comments (Pemilu Dataset), using F1-Score as the primary evaluation metric. The results show that **IndoBERTweet** consistently emerged as the top-performing model, while the **Standard Fine-Tuning** strategy with an optimized learning rate proved to be superior to the other two, more complex techniques.

## 🔑 Key Findings

1.  **IndoBERTweet is the Best Model:** This model consistently outperformed other architectures. In its best-case scenario (Standard Fine-Tuning), IndoBERTweet achieved an F1-Score of **0.9218** on the BBM Dataset and **0.7431** on the Pemilu Dataset.
2.  **Hyperparameter Tuning is the Best Strategy:** Scenario 1, with an optimized learning rate, yielded peak performance. For instance, IndoBERTweet on the BBM Dataset reached an F1-Score of **0.9218** with this method, surpassing the results from Scenario 3 (Differential LR, 0.9159) and Scenario 2 (Gradual Unfreezing, 0.9042).
3.  **Differential LR is Superior to Gradual Unfreezing:** Among the two advanced techniques, Differential Learning Rates (S3) proved to be far more robust. On the RoBERTa model with the BBM Dataset, Scenario 3 achieved an F1-Score of **0.8530**, whereas Scenario 2 suffered a performance failure with a score of only **0.4890**, indicating a massive difference in effectiveness.
4.  **Data Quality Has a Major Impact:** Model performance varied drastically between datasets. The highest F1-Score on the BBM Dataset (**0.9218**) was nearly **25% higher** than the highest score on the Pemilu Dataset (**0.7431**). This disparity was also reflected in the validation loss, where the lowest loss on the Pemilu Dataset (~0.64) was significantly higher than on the BBM Dataset (~0.27), suggesting that **domain complexity, noise, and data ambiguity** are major challenges in political sentiment analysis.

---

## 📁 Project Structure

```
kp-penelitian/
│
├── data/
│   ├── dataset_bbm.csv         # BBM app review dataset
│   └── dataset_pemilu.csv      # General Election comments dataset
│
├── notebooks/
│   ├── 1_Skenario_Fine_Tuning.ipynb
│   ├── 2_Skenario_Gradual_Unfreezing.ipynb
│   └── 3_Skenario_Differential_LR.ipynb
│
├── .gitignore
└── README.md
```

---

## 🔬 Experimental Methodology

The experiment was conducted by comparing three models across three training scenarios:

#### Models Used:
- **IndoBERT-base** (`indobenchmark/indobert-base-p1`)
- **IndoBERTweet** (`indobenchmark/indobertweet-base-p1`)
- **RoBERTa** (`roberta-base` or other Indonesian variants)

#### Experimental Scenarios:
1.  **Scenario 1: Standard Fine-Tuning & LR Optimization:** Training the entire model simultaneously while searching for the best learning rate via cross-validation.
2.  **Scenario 2: Gradual Unfreezing:** Training the model layer by layer, starting with the classifier head and progressively unfreezing the layers beneath it.
3.  **Scenario 3: Differential Learning Rates:** Training the entire model simultaneously but applying different learning rates to distinct layer groups.

## 🚀 How to Run the Experiment

To reproduce the results of this research, follow these steps:

#### 1. Prerequisites
- Python 3.8+
- `pip` and `venv` (recommended)
- GPU with sufficient VRAM (at least 8GB recommended) for training the models.

#### 2. Clone the Repository
```bash
git clone https://github.com/rifqimaruf/kp-penelitian.git
cd kp-penelitian
```

#### 3. Set up a Virtual Environment (Recommended)
```bash
# Create a virtual environment
python -m venv venv

# Activate the environment (Windows)
.\venv\Scripts\activate

# Activate the environment (macOS/Linux)
source venv/bin/activate
```

## 📊 Results
Detailed results from each scenario, including comparison tables, F1-Scores, and loss curves for each model and dataset, can be found in the final research report. In summary, the best combination found was:
- **Model:** `IndoBERTweet`
- **Strategy:** `Standard Fine-Tuning`
- **Optimal Learning Rate:** `3e-05` (for both datasets)

## Researcher's Notes
According to the researcher, there are three reasons why Scenarios 2 and 3 failed to improve upon the baseline performance:
1.  The baseline was already tested with several learning rates and validated using 3-Fold Cross-Validation, setting a very high benchmark.
2.  The determination of which layers to freeze in the Gradual Unfreezing scenario was too rigid and should have been further adapted to each model's specific architecture.
3.  The combination of learning rates in the Differential Learning Rates scenario was not yet optimized.
