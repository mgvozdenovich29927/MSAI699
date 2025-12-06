# Notebooks

This directory contains all Jupyter notebooks for the capstone project, organized chronologically by project week.

## Notebook Overview

| Notebook | Week | Description | GPU Required | Runtime |
|----------|------|-------------|--------------|---------|
| [01_week3_baseline_implementation.ipynb](01_week3_baseline_implementation.ipynb) | 3 | Baseline GPT-2 fine-tuning, perplexity evaluation | Yes | ~2-4 hours |
| [02_week4_lora_experiments.ipynb](02_week4_lora_experiments.ipynb) | 4 | LoRA hyperparameter tuning, sampling strategies, attention visualization | Yes | ~3 hours |
| [03_week5_deployment.ipynb](03_week5_deployment.ipynb) | 5 | Gradio interface, toxicity detection, deployment testing | Optional | ~1 hour |
| [04_week6_ab_testing.ipynb](04_week6_ab_testing.ipynb) | 6 | Comprehensive A/B testing, statistical analysis, error analysis | Yes | ~2 hours |
| [05_capstone_demo_dashboard.ipynb](05_capstone_demo_dashboard.ipynb) | 8 | Interactive 4-tab dashboard, research showcase | Optional | ~30 min |

## Quick Start

### Running in Google Colab

Each notebook is designed to run in Google Colab:

1. Upload notebook to Google Colab
2. Runtime → Change runtime type → GPU (for notebooks requiring GPU)
3. Run cells sequentially

### Running Locally

```bash
# Install dependencies
pip install -r ../requirements.txt

# Start Jupyter
jupyter notebook

# Select and run notebook
```

## Key Results by Notebook

### Week 3: Baseline Implementation
- **Baseline Perplexity:** 42.52
- **Model:** GPT-2 Small (124M parameters)
- **Dataset:** WikiText-103 (10k training, 1k validation)
- **Training Time:** ~2-4 hours on Tesla T4

### Week 4: LoRA Experiments
- **Optimal Configuration:** r=8, alpha=16, lr=3e-5
- **Best Perplexity:** 31.89 (25% improvement)
- **Trainable Parameters:** 294,912 (0.24% of base model)
- **Best Sampling Strategy:** Nucleus (top-p=0.9)
- **Key Finding:** 71% toxicity reduction vs. greedy decoding

### Week 5: Deployment
- **Interface:** Gradio multi-tab dashboard
- **Average Generation Time:** 1.28 seconds
- **Toxicity Rate:** 0.0006 (100% safe outputs)
- **Features:** Real-time parameter adjustment, side-by-side comparison

### Week 6: A/B Testing
- **Total Outputs Tested:** 360 across 4 configurations
- **Test Prompts:** 30 diverse prompts × 12 outputs each
- **Toxicity Reduction:** 71% vs. baseline greedy
- **Statistical Significance:** p < 0.05 for all metrics
- **Key Discovery:** Conservative temperature (0.7) caused 56.7% repetition errors

### Capstone Demo
- **4 Interactive Tabs:** 
  - Text Generation Lab (side-by-side comparison)
  - Performance Analytics (visualizations)
  - Explainability (attention visualization)
  - Experiment Results (comprehensive tables)
- **Real-time Features:** Parameter adjustment, toxicity scoring
- **Deployment Ready:** Complete production-ready demonstration

## Common Issues and Solutions

### GPU Out of Memory
```python
# Reduce batch size
training_args = TrainingArguments(
    per_device_train_batch_size=4,  # Reduced from 8
    gradient_accumulation_steps=4,   # Maintain effective batch size
)
```

### Slow Training
- Use Google Colab Pro for faster GPUs (Tesla T4 → A100)
- Reduce dataset size for testing (1000 samples instead of 10000)
- Enable mixed precision training:
```python
training_args = TrainingArguments(
    fp16=True,  # Mixed precision
)
```

### Package Not Found
```bash
# Install missing packages
!pip install transformers datasets peft accelerate bertviz detoxify gradio

# Or install all requirements
!pip install -r ../requirements.txt
```

### Weights & Biases Login Issues
```python
# Disable W&B logging if not needed
import os
os.environ['WANDB_DISABLED'] = 'true'

# Or configure to not prompt
training_args = TrainingArguments(
    report_to=[],  # Disable all reporting
)
```

## Execution Order

For first-time users, run notebooks in order:

1. **Week 3** → Establishes baseline metrics and training pipeline
2. **Week 4** → Optimizes with LoRA and tests sampling strategies  
3. **Week 5** → Deploys best model with Gradio interface
4. **Week 6** → Conducts comprehensive evaluation and statistical testing
5. **Demo** → Showcases complete system with interactive dashboard

Each notebook is self-contained but builds on previous weeks' results.

## Saving Results

### Automatic Saves
Results are automatically saved to `../results/` directory:
- `week3_baseline_metrics.json` - Baseline performance
- `week4_lora_experiments.csv` - LoRA hyperparameter results
- `week4_sampling_comparison.csv` - Sampling strategy comparison
- `week6_ab_testing_results.csv` - Comprehensive A/B test results

### Google Drive Integration
```python
# Mount Google Drive for persistent storage
from google.colab import drive
drive.mount('/content/drive')

# Save model checkpoints
model.save_pretrained('/content/drive/MyDrive/Capstone_Models/week4_optimal')
tokenizer.save_pretrained('/content/drive/MyDrive/Capstone_Models/week4_optimal')
```

## Visualization Outputs

Notebooks generate various visualizations:
- **Week 4:** Attention heatmaps (12 layers, 144 heads)
- **Week 6:** Box plots for metric comparisons
- **Week 6:** Statistical significance charts
- **Demo:** Interactive Gradio interface

## Reproducibility

### Random Seeds
All experiments use fixed random seeds for reproducibility:
```python
import random
import numpy as np
import torch

random.seed(42)
np.random.seed(42)
torch.manual_seed(42)
```

### Environment
- **Python:** 3.9+
- **PyTorch:** 2.0+
- **Transformers:** 4.35+
- **Hardware:** Tesla T4 GPU (Google Colab Pro)
- **CUDA:** 11.8

### Dataset
- **Name:** WikiText-103
- **Training Samples:** 10,000 (filtered)
- **Validation Samples:** 1,000
- **Preprocessing:** Minimum 50 characters per sample

## Related Documentation

- [Main README](../README.md) - Complete project overview
- [Setup Guide](../SETUP.md) - Installation and environment setup
- [Data Documentation](../data/README.md) - Dataset information
- [Results Documentation](../results/README.md) - Experiment results

## Notebook Structure

Each notebook follows a consistent structure:

1. **Setup & Installation**
   - Package installation
   - Library imports
   - Environment configuration

2. **Data Loading & Preprocessing**
   - Dataset loading
   - Tokenization
   - Train/validation split

3. **Model Configuration**
   - Model initialization
   - LoRA configuration (if applicable)
   - Training arguments

4. **Training/Experimentation**
   - Model training
   - Hyperparameter experiments
   - Progress tracking

5. **Evaluation**
   - Metric calculation
   - Results visualization
   - Statistical analysis

6. **Saving Results**
   - Export metrics
   - Save model checkpoints
   - Generate reports

## Support

For questions about running the notebooks:
- Check [SETUP.md](../SETUP.md) for environment setup
- Review inline comments in each notebook
- Refer to [HuggingFace documentation](https://huggingface.co/docs/transformers)
- Visit [HuggingFace forums](https://discuss.huggingface.co/)

## Educational Value

These notebooks demonstrate:
- **End-to-end ML pipeline:** From baseline to production
- **Parameter-efficient fine-tuning:** LoRA implementation
- **Systematic experimentation:** Hyperparameter tuning methodology
- **Comprehensive evaluation:** Multiple metrics, statistical testing
- **Deployment practices:** Production-ready interface development
- **Responsible AI:** Safety filtering, explainability, ethics

## Citation

If you use or reference these notebooks, please cite:

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

*All notebooks developed and tested in Google Colab Pro with Tesla T4 GPU*  
*Total development time: 8 weeks (October - December 2025)*  
*Combined execution time: ~10-12 hours*

