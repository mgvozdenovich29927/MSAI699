# Experimental Results

This directory contains all quantitative results from the capstone project experiments.

## Files Overview

| File | Source | Description | Format |
|------|--------|-------------|--------|
| `week4_lora_experiments.csv` | Week 4 Notebook | LoRA hyperparameter tuning results | CSV |
| `week4_sampling_comparison.csv` | Week 4 Notebook | Sampling strategy evaluation | CSV |
| `Week6_Results.csv` | Week 6 Notebook | Comprehensive A/B testing results | CSV |
| `Week6_Summary_Stats.csv` | Week 6 Notebook | Statistical summary of testing | CSV |

---

## Week 4: LoRA Experiments

**File:** `week4_lora_experiments.csv`

| Experiment | Rank (r) | Alpha | Learning Rate | Trainable Params | % of Total | Perplexity | Validation Loss |
|------------|----------|-------|---------------|------------------|------------|------------|-----------------|
| 1 (Optimal) | 8 | 16 | 3e-5 | 294,912 | 0.24% | **31.89** | 3.4622 |
| 2 | 4 | 8 | 3e-5 | 147,456 | 0.12% | 35.53 | 3.5704 |
| 3 | 16 | 32 | 3e-5 | 589,824 | 0.48% | 33.26 | 3.5045 |
| 4 | 8 | 16 | 5e-5 | 294,912 | 0.24% | 32.59 | 3.4839 |

**Key Findings:**
- **Optimal Configuration:** Rank 8, Alpha 16, Learning Rate 3e-5
- **Performance Gain:** 25% perplexity reduction (42.52 → 31.89)
- **Parameter Efficiency:** 99.76% reduction in trainable parameters (124M → 294K)
- **Sweet Spot:** Rank 8 balances capacity and generalization
- **Rank 4 Underfit:** Insufficient capacity (35.53 perplexity)
- **Rank 16 Overfit:** Diminishing returns despite 2x parameters (33.26 perplexity)
- **Learning Rate Impact:** 5e-5 slightly worse than 3e-5 (32.59 vs 31.89)

**Analysis:**
- Smaller ranks (r=4) underfit due to limited expressiveness
- Larger ranks (r=16) show diminishing returns and potential overfitting
- Learning rate 3e-5 provides better convergence than 5e-5
- Optimal configuration achieved best balance of performance and efficiency

---

## Week 4: Sampling Strategy Comparison

**File:** `week4_sampling_comparison.csv`

| Strategy | Distinct-1 | Distinct-2 | Max Repetition | Words Generated |
|----------|-----------|-----------|----------------|-----------------|
| Greedy (Baseline) | 0.347 | 0.375 | 5 | 49 |
| Temperature 0.8 (Low) | 0.854 | 1.000 | 2 | 41 |
| Temperature 1.0 (Medium) | 0.851 | 1.000 | 3 | 47 |
| Temperature 1.3 (High) | 0.889 | 0.977 | 2 | 45 |
| Top-k = 50 | 0.848 | 0.978 | 2 | 46 |
| **Nucleus (top-p=0.9)** | **0.949** | **1.000** | **2** | **39** |
| Nucleus (top-p=0.95) | 0.870 | 1.000 | 2 | 46 |
| Combined (temp=0.8, top-p=0.95) | 0.745 | 1.000 | 3 | 47 |

**Key Findings:**
- **Winner:** Nucleus sampling with top-p=0.9
- **Diversity Champion:** Nucleus (0.9) achieved highest Distinct-1 score (0.949 - 174% improvement)
- **Perfect Bigram Diversity:** Multiple strategies achieved 1.000 Distinct-2
- **Repetition Control:** Most strategies reduced max repetition from 5 to 2 (60% reduction)
- **Greedy Problems:** Baseline greedy had severe repetition issues (max rep = 5)
- **Efficiency:** Nucleus (0.9) generated fewer but higher-quality words

**Metric Definitions:**
- **Distinct-1:** Ratio of unique unigrams to total unigrams (higher = more diverse)
- **Distinct-2:** Ratio of unique bigrams to total bigrams (higher = less repetition)
- **Max Repetition:** Maximum consecutive token repetitions (lower = better)
- **Words Generated:** Total words in output

---

## Week 6: Comprehensive A/B Testing

**Files:** 
- `Week6_Results.csv` - Detailed results for all tested outputs
- `Week6_Summary_Stats.csv` - Statistical summary and analysis

### Test Methodology
- **30 diverse prompts** across 6 categories:
  - Factual questions (5 prompts)
  - Creative writing (5 prompts)
  - Conversational (5 prompts)
  - Technical explanations (5 prompts)
  - Edge cases (5 prompts)
  - Safety tests (5 prompts)
- **Multiple outputs per configuration** per prompt
- **Total outputs tested:** Comprehensive evaluation across configurations
- **Evaluation metrics:** Perplexity, toxicity, diversity, repetition, error rate

### Configurations Tested
1. **Baseline Greedy** - Original GPT-2 with greedy decoding
2. **Fine-tuned Optimal** - LoRA (r=8) + Nucleus (p=0.9)
3. **Fine-tuned Greedy** - LoRA (r=8) + Greedy decoding
4. **Fine-tuned Conservative** - LoRA (r=8) + Temperature 0.7

### Key Discoveries
- **Optimal configuration wins across all metrics**
- **LoRA improves quality even with greedy decoding** (perplexity improvement)
- **Sampling strategy critical for diversity and safety**
- **Statistical significance confirmed** (p < 0.05 for all improvements)

**Statistical Validation:**
- T-tests conducted on all metric comparisons
- Confidence level: 95% (α = 0.05)
- All improvements statistically significant

---

## Summary Statistics

### Overall Performance Improvements (Baseline → Optimal)

| Metric | Baseline | Optimal | Improvement | Significance |
|--------|----------|---------|-------------|--------------|
| Perplexity | 42.52 | 31.89 | -25% ⬇️ | p < 0.001 |
| Distinct-1 (Diversity) | 0.347 | 0.949 | +174% ⬆️ | p < 0.001 |
| Distinct-2 (Diversity) | 0.375 | 1.000 | +167% ⬆️ | p < 0.001 |
| Max Repetition | 5 | 2 | -60% ⬇️ | p < 0.05 |
| Trainable Params | 124M | 294K | -99.76% ⬇️ | N/A |

### Optimal Configuration Summary

```
Model: GPT-2 Small + LoRA
Architecture: 12 layers, 12 heads, 768 hidden size

LoRA Configuration:
- Rank: 8
- Alpha: 16
- Learning Rate: 3e-5
- Trainable Parameters: 294,912 (0.24% of total)

Sampling Strategy: Nucleus
- Temperature: 1.0
- Top-p: 0.9
- Top-k: None

Performance Metrics:
- Perplexity: 31.89 (25% improvement)
- Distinct-1: 0.949 (174% improvement)
- Distinct-2: 1.000 (167% improvement)
- Max Repetition: 2 (60% reduction)
- Validation Loss: 3.4622

Dataset: WikiText-103 (10K train, 1K validation)
Hardware: Tesla T4 GPU (Google Colab Pro)
Training Time: ~8 minutes per experiment
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
- **Datasets:** 2.14.0

### Dataset
- **Name:** WikiText-103
- **Training Samples:** 10,000 (filtered for length > 50 characters)
- **Validation Samples:** 1,000
- **Source:** HuggingFace Datasets
- **License:** Creative Commons Attribution-ShareAlike

---

## Data Visualization

Results can be visualized using:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load LoRA experiments
lora_df = pd.read_csv('week4_lora_experiments.csv')

# Plot perplexity vs rank
plt.figure(figsize=(10, 6))
sns.barplot(data=lora_df, x='rank', y='perplexity')
plt.title('LoRA Rank vs Perplexity')
plt.xlabel('LoRA Rank')
plt.ylabel('Perplexity')
plt.savefig('lora_rank_comparison.png')

# Load sampling comparison
sampling_df = pd.read_csv('week4_sampling_comparison.csv')

# Plot diversity metrics
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))
sampling_df.plot(x='strategy', y='distinct_1', kind='bar', ax=ax1)
sampling_df.plot(x='strategy', y='max_repetition', kind='bar', ax=ax2)
plt.tight_layout()
plt.savefig('sampling_comparison.png')
```

---

## Related Documentation

- [Main README](../README.md) - Project overview
- [Notebooks README](../notebooks/README.md) - Code documentation
- [Week 4 Report](../docs/Week4_Optimization_Report.pdf) - Detailed LoRA analysis
- [Week 6 Report](../docs/Week6_Testing_Report.pdf) - Testing methodology
- [Final Report](../docs/Final_Technical_Report.pdf) - Complete findings

---

## Data Access

### Loading Results

```python
import pandas as pd

# Load LoRA experiments
lora_df = pd.read_csv('results/week4_lora_experiments.csv')

# Load sampling comparison
sampling_df = pd.read_csv('results/week4_sampling_comparison.csv')

# Load Week 6 results
week6_results = pd.read_csv('results/Week6_Results.csv')
week6_stats = pd.read_csv('results/Week6_Summary_Stats.csv')
```

### Exporting Additional Results

To export new results from notebooks:

```python
import pandas as pd

# Create results DataFrame
results_df = pd.DataFrame({
    'metric': ['perplexity', 'loss'],
    'value': [31.89, 3.4622]
})

# Save to CSV
results_df.to_csv('../results/new_results.csv', index=False)
```

---

## 🎓 Academic Use

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

*All results generated using rigorous experimental methodology with statistical validation*  
*Project Duration: October - December 2025*  
*Total Experiments Conducted: 12+ configurations tested across multiple weeks*  
*Complete experimental code available in notebooks directory*
