# MSAI699
MSAI Capstone: Optimizing LLM text generation through LoRA fine-tuning and nucleus sampling 
# Optimizing Domain-Specific Text Generation with Large Language Models

**MSAI 699 Capstone Project**  
**Author:** Milica Gvozdenovich  
**Institution:** University of the Cumberlands  
**Program:** Master of Science in Artificial Intelligence

## Project Overview

This project investigates parameter-efficient fine-tuning of large language models (LLMs) for domain-specific text generation. Through systematic experimentation with Low-Rank Adaptation (LoRA) and comprehensive sampling strategy analysis, the research demonstrates that high-quality text generation can be achieved while reducing trainable parameters to just 0.24% of the base model.

### Key Findings

- **Optimal LoRA Configuration:** Rank r=8, alpha=16, learning rate 3e-5 achieved 31.89 perplexity
- **Best Sampling Strategy:** Nucleus sampling (top-p=0.9) achieved 71% toxicity reduction with high diversity
- **Parameter Efficiency:** Only 294,912 trainable parameters (0.24% of GPT-2 Small's 124M parameters)
- **Practical Deployment:** Interactive Gradio dashboard with real-time generation and safety filtering

## Research Questions

1. How does LoRA compare to full fine-tuning in computational requirements, convergence, and performance?
2. What relationships exist between sampling hyperparameters and generation quality metrics?
3. How do sampling strategies affect probability distributions and practical text quality?
4. Can explainability tools provide actionable insights for improving generation?

## Repository Structure

```
.
├── README.md                          # This file
├── requirements.txt                   # Python dependencies
│
├── notebooks/                         # Jupyter notebooks for experiments
│   ├── week3_baseline_implementation.ipynb
│   ├── week4_lora_experiments.ipynb
│   ├── week5_deployment.ipynb
│   ├── week6_ab_testing.ipynb
│   └── week8_dashboard_demo.ipynb
│
├── src/                              # Source code modules
│   ├── data/
│   │   └── preprocessing.py          # Dataset loading and preprocessing
│   ├── models/
│   │   ├── baseline.py               # Baseline GPT-2 model
│   │   └── lora_finetuning.py        # LoRA fine-tuning implementation
│   ├── evaluation/
│   │   ├── metrics.py                # Evaluation metrics (perplexity, diversity, toxicity)
│   │   └── visualization.py          # Attention visualization and plotting
│   └── deployment/
│       └── gradio_app.py             # Gradio dashboard application
│
├── data/                             # Dataset information
│   └── README.md                     # Dataset documentation
│
├── results/                          # Experimental results
│   ├── lora_experiments.csv          # LoRA hyperparameter results
│   ├── sampling_comparison.csv       # Sampling strategy comparison
│   └── ab_testing_results.csv        # A/B testing comprehensive results
│
├── docs/                             # Project documentation
│   ├── proposal.pdf                  # Project proposal
│   ├── literature_review.pdf         # Week 2 literature review
│   ├── week3_baseline_report.pdf     # Baseline implementation report
│   ├── week4_optimization_report.pdf # Optimization experiments report
│   ├── week5_deployment_report.pdf   # Deployment strategy report
│   ├── week6_testing_report.pdf      # Testing and evaluation report
│   ├── final_report.pdf              # Comprehensive final report
│   └── reflection_essay.pdf          # Personal reflection essay
│
└── presentation/                     # Presentation materials
    ├── slides.pptx                   # Project presentation slides
    └── demo_video.mp4                # Demo video (or link)
```

##  Quick Start

### Prerequisites

- Python 3.8 or higher
- CUDA-capable GPU (recommended) or Google Colab Pro
- 8GB+ RAM

### Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/llm-domain-text-generation.git
cd llm-domain-text-generation
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. (Optional) Set up Weights & Biases for experiment tracking:
```bash
wandb login
```

### Running the Code

#### 1. Baseline Model Training (Week 3)
```bash
# Open in Jupyter or Google Colab
jupyter notebook notebooks/week3_baseline_implementation.ipynb
```

#### 2. LoRA Experiments (Week 4)
```bash
# Run hyperparameter tuning experiments
jupyter notebook notebooks/week4_lora_experiments.ipynb
```

#### 3. Interactive Demo
```bash
# Launch Gradio dashboard
python src/deployment/gradio_app.py
```

The dashboard will be available at `http://localhost:7860`

## Experimental Results

### LoRA Hyperparameter Tuning

| Configuration | Rank (r) | Learning Rate | Trainable Params | Perplexity |
|--------------|----------|---------------|------------------|------------|
| **Optimal** | 8 | 3e-5 | 294,912 | **31.89** |
| Experiment 2 | 4 | 3e-5 | 147,456 | 35.53 |
| Experiment 3 | 16 | 3e-5 | 589,824 | 33.26 |
| Experiment 4 | 8 | 5e-5 | 294,912 | 32.59 |

### Sampling Strategy Comparison

| Strategy | Distinct-1 | Distinct-2 | Max Repetition | Avg Toxicity |
|----------|-----------|-----------|----------------|--------------|
| Greedy | 0.347 | 0.375 | 5 | 0.0021 |
| **Nucleus (p=0.9)** | **0.949** | **1.000** | **2** | **0.0006** |
| Nucleus (p=0.95) | 0.870 | 1.000 | 2 | 0.0008 |
| Temperature (0.7) | 0.854 | 1.000 | 2 | 0.0015 |

### Key Insights

- **Parameter Efficiency:** LoRA achieves 25% perplexity reduction with 99.76% fewer trainable parameters
- **Sampling Quality:** Nucleus sampling (top-p=0.9) provides optimal balance of diversity and coherence
- **Safety:** 71% toxicity reduction compared to baseline greedy decoding
- **Practical Impact:** Generation time averages 1.28 seconds with consistent safety filtering

##  Methodology

### Model Architecture
- **Base Model:** GPT-2 Small (124M parameters)
- **Fine-tuning Method:** Low-Rank Adaptation (LoRA)
- **Dataset:** WikiText-103 (10,000 training samples, 1,000 validation samples)

### Training Configuration
- **LoRA Rank:** 8
- **LoRA Alpha:** 16
- **Learning Rate:** 3e-5
- **Batch Size:** 8 (effective batch size 16 with gradient accumulation)
- **Epochs:** 3
- **Optimizer:** AdamW
- **Hardware:** Google Colab Pro (Tesla T4 GPU)

### Evaluation Metrics
- **Perplexity:** Model confidence and language modeling quality
- **Diversity Metrics:** Distinct-1, Distinct-2, repetition ratio
- **Toxicity Scores:** Detoxify toxicity classifier
- **Generation Quality:** Self-BLEU, human evaluation rubrics

### Explainability
- **Attention Visualization:** BertViz for token-level attention patterns
- **Layer Analysis:** Hierarchical pattern discovery across 12 transformer layers

## Interactive Dashboard Features

### Tab 1: Text Generation Lab
- Side-by-side comparison: Base GPT-2 vs. Fine-tuned model
- Real-time parameter adjustment (temperature, top-p, top-k)
- Multiple sampling strategies
- Live toxicity scoring

### Tab 2: Performance Analytics
- A/B testing results visualization
- Statistical significance charts
- Metric comparisons across configurations

### Tab 3: Explainability
- Token-level analysis
- Attention visualization
- Model behavior insights

### Tab 4: Experiment Results
- Comprehensive LoRA configuration tables
- Sampling strategy comparison data
- Full experimental methodology

## Key Technologies

- **Deep Learning Framework:** PyTorch
- **Transformers Library:** HuggingFace Transformers
- **Parameter-Efficient Training:** PEFT (Parameter-Efficient Fine-Tuning)
- **Experiment Tracking:** Weights & Biases
- **Deployment:** Gradio
- **Safety Tools:** Detoxify (toxicity detection)
- **Visualization:** BertViz, Matplotlib, Seaborn

## Academic References

1. Vaswani, A., et al. (2017). Attention is all you need. *Advances in Neural Information Processing Systems*, 30.
2. Radford, A., et al. (2019). Language models are unsupervised multitask learners. *OpenAI Technical Report*.
3. Hu, E. J., et al. (2021). LoRA: Low-rank adaptation of large language models. *arXiv preprint arXiv:2106.09685*.
4. Holtzman, A., et al. (2020). The curious case of neural text degeneration. *International Conference on Learning Representations*.
5. Bender, E. M., et al. (2021). On the dangers of stochastic parrots: Can language models be too big? *Proceedings of the 2021 ACM Conference on Fairness, Accountability, and Transparency*.

## Ethical Considerations

This project addresses several ethical concerns:

- **Bias Mitigation:** Fairness audits and demographic review
- **Toxicity Detection:** Real-time safety filtering via Detoxify
- **Explainability:** Attention visualization for transparency
- **Environmental Impact:** Parameter-efficient training reduces carbon footprint
- **Responsible Deployment:** Human-in-the-loop recommendations for production use

## Learning Outcomes

This project demonstrates mastery of:

1. **Complex AI Implementation:** Transformer architecture, LoRA, sampling strategies, explainability tools
2. **Advanced Analysis:** Optimization theory, probability distributions, statistical significance testing
3. **Research Methodology:** Literature synthesis, systematic experimentation, reproducible research
4. **Ethical AI:** Bias analysis, toxicity mitigation, transparency, environmental considerations
5. **Professional Communication:** Technical documentation, presentations, multi-format communication

##  Future Work

1. **Scaling Experiments:** Test LoRA patterns on GPT-2 Medium and Large
2. **Domain-Specific Fine-tuning:** Medical, legal, scientific text generation
3. **Advanced Safety:** Enhanced bias detection and mitigation strategies
4. **Human Evaluation:** Comprehensive user studies of sampling strategies
5. **Production Deployment:** Cloud-native architecture with API endpoints

##  Contact

**Milica Gvozdenovich**  
Master of Science in Artificial Intelligence  
University of the Cumberlands  
Email: mgvozdenovich29927@ucumberlands.edu

## License

This project is submitted as part of MSAI 699 Capstone requirements at the University of the Cumberlands. All rights reserved.

## Acknowledgments

- University of the Cumberlands MSAI Program
- HuggingFace for excellent libraries and documentation
- Google Colab for computational resources
- Open-source AI/ML community for foundational research

---

*Last Updated: December 2025*
