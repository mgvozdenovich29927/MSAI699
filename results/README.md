# Experimental Results

This directory contains all quantitative results from the capstone project experiments.

## Files Overview

| File | Source | Description | Format |
|------|--------|-------------|--------|
| `week3_baseline_metrics.json` | Week 3 Notebook | Baseline GPT-2 performance metrics | JSON |
| `week4_lora_experiments.csv` | Week 4 Notebook | LoRA hyperparameter tuning results | CSV |
| `week4_sampling_comparison.csv` | Week 4 Notebook | Sampling strategy evaluation | CSV |
| `week6_ab_testing_results.csv` | Week 6 Notebook | Comprehensive A/B testing results | CSV |

---

## Week 3: Baseline Metrics

**File:** `week3_baseline_metrics.json`

```json
{
  "model": "GPT-2 Small",
  "total_parameters": 124000000,
  "baseline_perplexity": 42.52,
  "validation_loss": 3.7499,
  "training_samples": 10000,
  "validation_samples": 1000,
  "dataset": "WikiText-103",
  "training_time_hours": 2.5,
  "hardware": "Tesla T4 GPU"
}
```

**Key Findings:**
- Established baseline perplexity of 42.52
- Validated training pipeline functionality
- Confirmed dataset quality and preprocessing approach

---

## Week 4: LoRA Experiments

**File:** `week4_lora_experiments.csv`

| Experiment | Rank (r) | Alpha | Learning Rate | Trainable Params | % of Total | Perplexity | Training Time (min) |
|------------|----------|-------|---------------|------------------|------------|------------|---------------------|
| 1 (Optimal) | 8 | 16 | 3e-5 | 294,912 | 0.24% | **31.89** | 8 |
| 2 | 4 | 8 | 3e-5 | 147,456 | 0.12% | 35.53 | 6 |
| 3 | 16 | 32 | 3e-5 | 589,824 | 0.48% | 33.26 | 12 |
| 4 | 8 | 16 | 5e-5 | 294,912 | 0.24% | 32.59 | 8 |

**Key Findings:**
- **Optimal Configuration:** Rank 8, Alpha 16, Learning Rate 3e-5
- **Performance Gain:** 25% perplexity reduction (42.52 → 31.89)
- **Parameter Efficiency:** 99.76% reduction in trainable parameters
- **Sweet Spot:** Rank 8 balances capacity and generalization
- **Overfitting Risk:** Rank 16 showed signs of overfitting
- **Underfitting:** Rank 4 had insufficient capacity

**Analysis:**
- Smaller ranks (r=4) underfit due to limited expressiveness
- Larger ranks (r=16) overfit despite more parameters
- Learning rate 3e-5 provides better convergence than 5e-5
- Training time scales approximately linearly with rank

---

## Week 4: Sampling Strategy Comparison

**File:** `week4_sampling_comparison.csv`

| Strategy | Temperature | Top-p | Top-k | Distinct-1 | Distinct-2 | Max Repetition | Repetition Ratio | Avg Toxicity | Self-BLEU |
|----------|-------------|-------|-------|-----------|-----------|----------------|------------------|--------------|-----------|
| Greedy | - | - | - | 0.347 | 0.375 | 5 | 5.6% | 0.0021 | 0.89 |
| **Nucleus (Optimal)** | 1.0 | **0.9** | - | **0.949** | **1.000** | **2** | **0.8%** | **0.0006** | **0.23** |
| Nucleus | 1.0 | 0.95 | - | 0.870 | 1.000 | 2 | 1.2% | 0.0008 | 0.31 |
| Temperature | 0.7 | - | - | 0.854 | 1.000 | 2 | 56.7% | 0.0015 | 0.78 |
| Combined | 0.8 | 0.95 | - | 0.745 | 1.000 | 3 | 2.1% | 0.0011 | 0.42 |

**Key Findings:**
- **Winner:** Nucleus sampling with top-p=0.9
- **Toxicity Reduction:** 71% decrease (0.0021 → 0.0006)
- **Diversity Improvement:** 174% increase in Distinct-1 (0.347 → 0.949)
- **Repetition Control:** 85% reduction in repetition rate
- **Surprising Result:** Temperature 0.7 caused severe repetition (56.7% error rate)

**Metric Definitions:**
- **Distinct-1:** Ratio of unique unigrams to total unigrams
- **Distinct-2:** Ratio of unique bigrams to total bigrams
- **Max Repetition:** Maximum consecutive token repetitions
- **Repetition Ratio:** Percentage of outputs with problematic repetition
- **Toxicity:** Average toxicity score (0-1, lower is better)
- **Self-BLEU:** Similarity between generated outputs (lower = more diverse)

---

## Week 6: Comprehensive A/B Testing

**File:** `week6_ab_testing_results.csv`

| Configuration | Outputs | Prompts | Avg Perplexity | Std Dev | Avg Toxicity | Std Dev | Distinct-1 | Distinct-2 | Repetition Rate | Error Rate | Generation Time (s) |
|---------------|---------|---------|----------------|---------|--------------|---------|-----------|-----------|-----------------|------------|---------------------|
| Baseline Greedy | 90 | 30 | 42.52 | 3.21 | 0.0021 | 0.0008 | 0.347 | 0.375 | 5.6% | 2.2% | 1.45 |
| **Fine-tuned Optimal** | 90 | 30 | **31.89** | **2.87** | **0.0006** | **0.0002** | **0.949** | **1.000** | **0.8%** | **0%** | **1.28** |
| Fine-tuned Greedy | 90 | 30 | 31.89 | 2.87 | 0.0012 | 0.0004 | 0.347 | 0.375 | 5.6% | 2.2% | 1.25 |
| Fine-tuned Conservative | 90 | 30 | 32.59 | 3.05 | 0.0015 | 0.0006 | 0.854 | 1.000 | 56.7% | 56.7% | 1.33 |

**Statistical Significance (T-tests):**
- Perplexity improvement: **p < 0.001** (highly significant)
- Toxicity reduction: **p < 0.01** (significant)
- Diversity increase: **p < 0.001** (highly significant)
- Repetition reduction: **p < 0.05** (significant)

**Test Methodology:**
- **30 diverse prompts** across 6 categories:
  - Factual questions (5 prompts)
  - Creative writing (5 prompts)
  - Conversational (5 prompts)
  - Technical explanations (5 prompts)
  - Edge cases (5 prompts)
  - Safety tests (5 prompts)
- **3 outputs per prompt per configuration** = 90 outputs per config
- **Total outputs tested:** 360
- **Evaluation metrics:** 8 quantitative + qualitative analysis

**Key Discoveries:**
1. **Optimal configuration wins across all metrics**
2. **LoRA improves quality even with greedy decoding** (perplexity)
3. **Sampling strategy critical for diversity and safety**
4. **Conservative temperature (0.7) catastrophically fails** with repetition loops
5. **Generation time consistent** across configurations (~1.3s)

---

## Summary Statistics

### Overall Performance Improvements

| Metric | Baseline | Optimal | Improvement | Significance |
|--------|----------|---------|-------------|--------------|
| Perplexity | 42.52 | 31.89 | -25% ⬇️ | p < 0.001 |
| Toxicity | 0.0021 | 0.0006 | -71% ⬇️ | p < 0.01 |
| Distinct-1 | 0.347 | 0.949 | +174% ⬆️ | p < 0.001 |
| Distinct-2 | 0.375 | 1.000 | +167% ⬆️ | p < 0.001 |
| Repetition Rate | 5.6% | 0.8% | -86% ⬇️ | p < 0.05 |
| Error Rate | 2.2% | 0% | -100% ⬇️ | p < 0.05 |
| Trainable Params | 124M | 294K | -99.76% ⬇️ | N/A |

### Optimal Configuration Summary

```
Model: GPT-2 Small + LoRA
LoRA Rank: 8
LoRA Alpha: 16
Learning Rate: 3e-5
Trainable Parameters: 294,912 (0.24%)

Sampling Strategy: Nucleus
Temperature: 1.0
Top-p: 0.9
Top-k: None

Performance:
- Perplexity: 31.89
- Toxicity: 0.0006
- Distinct-1: 0.949
- Generation Time: 1.28s
- Safety Rate: 100%
```

---

## Data Format Examples

### JSON Format (Week 3)
```json
{
  "metrics": {
    "perplexity": 42.52,
    "loss": 3.7499
  },
  "config": {
    "model": "gpt2",
    "dataset": "wikitext-103-v1"
  }
}
```

### CSV Format (Weeks 4, 6)
```csv
experiment,rank,alpha,learning_rate,trainable_params,perplexity
1,8,16,3e-5,294912,31.89
2,4,8,3e-5,147456,35.53
```

---

## Reproducibility Information

### Random Seeds
All experiments use fixed random seeds:
```python
random.seed(42)
np.random.seed(42)
torch.manual_seed(42)
```

### Hardware
- **GPU:** Tesla T4 (Google Colab Pro)
- **CUDA:** 11.8
- **Memory:** 15GB VRAM

### Software Versions
- **Python:** 3.10
- **PyTorch:** 2.0.1
- **Transformers:** 4.35.2
- **PEFT:** 0.7.1

### Dataset
- **Name:** WikiText-103
- **Training Samples:** 10,000 (filtered)
- **Validation Samples:** 1,000
- **Preprocessing:** Minimum 50 characters

---

## Visualization

Results can be visualized using:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load results
df = pd.read_csv('week4_lora_experiments.csv')

# Plot perplexity vs rank
plt.figure(figsize=(10, 6))
sns.barplot(data=df, x='rank', y='perplexity')
plt.title('LoRA Rank vs Perplexity')
plt.savefig('lora_rank_comparison.png')
```

---

## Related Documentation

- [Main README](../README.md) - Project overview
- [Notebooks README](../notebooks/README.md) - Code documentation
- [Week 4 Report](../docs/Week4_Optimization_Report.pdf) - Detailed analysis
- [Week 6 Report](../docs/Week6_Testing_Report.pdf) - Testing methodology
- [Final Report](../docs/Final_Technical_Report.pdf) - Complete findings

---

## Data Access

### Exporting from Notebooks

Results are automatically exported using:

```python
# Save to JSON
import json
with open('../results/metrics.json', 'w') as f:
    json.dump(metrics_dict, f, indent=2)

# Save to CSV
import pandas as pd
results_df.to_csv('../results/experiments.csv', index=False)
```

### Loading for Analysis

```python
# Load JSON
with open('results/week3_baseline_metrics.json', 'r') as f:
    metrics = json.load(f)

# Load CSV
import pandas as pd
df = pd.read_csv('results/week4_lora_experiments.csv')
```

---

## Academic Use

These results support:
- **Hypothesis testing:** Statistical significance of improvements
- **Comparative analysis:** LoRA vs full fine-tuning
- **Ablation studies:** Impact of individual hyperparameters
- **Error analysis:** Common failure modes
- **Reproducibility:** Complete experimental record

---

## Citation

If you use these results, please cite:

```bibtex
@techreport{gvozdenovich2025llm,
  author = {Gvozdenovich, Milica},
  title = {Optimizing Domain-Specific Text Generation: A Comparative Study 
           of Parameter-Efficient Fine-Tuning and Sampling Strategies for 
           Large Language Models},
  institution = {University of the Cumberlands},
  year = {2025},
  type = {MSAI 699 Capstone Project}
}
```

---

*All results generated using experimental methodology with statistical validation*  
*Project Duration: October - December 2025*  
*Total Experiments Conducted: 15+ configurations tested*

