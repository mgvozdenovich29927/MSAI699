# Setup Guide

Complete instructions for setting up and running this project.

## Table of Contents
1. [Environment Setup](#environment-setup)
2. [Installation](#installation)
3. [Google Colab Setup](#google-colab-setup)
4. [Local Setup](#local-setup)
5. [Verification](#verification)
6. [Troubleshooting](#troubleshooting)

## Environment Setup

### Option 1: Google Colab (Recommended for Beginners)

Google Colab provides free GPU access and requires no local setup.

1. Go to [Google Colab](https://colab.research.google.com/)
2. Sign in with your Google account
3. Upload the notebook files from the `notebooks/` directory
4. Install Colab Pro ($10/month) for:
   - Faster GPUs (Tesla T4 or better)
   - Longer runtime limits
   - More memory

### Option 2: Local Setup

#### System Requirements

**Minimum:**
- Python 3.8 or higher
- 8GB RAM
- 10GB free disk space
- CPU training possible but slow

**Recommended:**
- Python 3.9+
- 16GB+ RAM
- NVIDIA GPU with 8GB+ VRAM (RTX 3060 or better)
- CUDA 11.8 or higher
- 20GB+ free disk space

#### Check Your GPU

```bash
# Check if NVIDIA GPU is available
nvidia-smi

# Check CUDA version
nvcc --version
```

## Installation

### Step 1: Clone Repository

```bash
git clone https://github.com/yourusername/llm-domain-text-generation.git
cd llm-domain-text-generation
```

### Step 2: Create Virtual Environment

```bash
# Using venv
python -m venv venv

# Activate on Windows
venv\Scripts\activate

# Activate on macOS/Linux
source venv/bin/activate
```

Or using conda:

```bash
# Create environment
conda create -n llm-project python=3.9

# Activate environment
conda activate llm-project
```

### Step 3: Install Dependencies

```bash
# Install all requirements
pip install -r requirements.txt

# If you have GPU with CUDA
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# If you only have CPU
pip install torch torchvision torchaudio
```

### Step 4: Download NLTK Data (for evaluation)

```python
import nltk
nltk.download('punkt')
```

## Google Colab Setup

### Installing Libraries in Colab

Add this cell at the start of each notebook:

```python
# Install required packages
!pip install transformers datasets peft accelerate bertviz detoxify gradio wandb

# Import libraries
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM, TrainingArguments, Trainer
from datasets import load_dataset
from peft import LoraConfig, get_peft_model
```

### Mounting Google Drive (Optional)

To save models and results:

```python
from google.colab import drive
drive.mount('/content/drive')

# Save model to Drive
model.save_pretrained('/content/drive/MyDrive/Capstone_Models/week4_optimal')
```

### GPU Verification in Colab

```python
# Check GPU availability
print(torch.cuda.is_available())
print(torch.cuda.get_device_name(0))
```

## Local Setup

### Installing Jupyter

```bash
pip install jupyter notebook

# Start Jupyter
jupyter notebook
```

### Setting up Weights & Biases (Optional)

```bash
# Install wandb
pip install wandb

# Login
wandb login

# Enter your API key from https://wandb.ai/authorize
```

## Verification

### Test Installation

Create and run this test script:

```python
# test_setup.py
import torch
import transformers
from datasets import load_dataset
from peft import LoraConfig

print("=" * 50)
print("Testing Installation")
print("=" * 50)

# Check PyTorch
print(f"PyTorch version: {torch.__version__}")
print(f"CUDA available: {torch.cuda.is_available()}")
if torch.cuda.is_available():
    print(f"CUDA version: {torch.version.cuda}")
    print(f"GPU: {torch.cuda.get_device_name(0)}")

# Check Transformers
print(f"Transformers version: {transformers.__version__}")

# Test dataset loading
print("\nTesting dataset loading...")
dataset = load_dataset("wikitext", "wikitext-103-v1", split="train[:10]")
print(f"Loaded {len(dataset)} samples")

# Test model loading
print("\nTesting model loading...")
from transformers import AutoTokenizer
tokenizer = AutoTokenizer.from_pretrained("gpt2")
print(f"Tokenizer loaded: {tokenizer.name_or_path}")

print("\n" + "=" * 50)
print("All tests passed! ✓")
print("=" * 50)
```

Run the test:

```bash
python test_setup.py
```

## Troubleshooting

### Common Issues

#### 1. CUDA Out of Memory

**Solution:**
- Reduce batch size in training arguments
- Enable gradient accumulation
- Use mixed precision training (FP16)

```python
training_args = TrainingArguments(
    per_device_train_batch_size=4,  # Reduce from 8
    gradient_accumulation_steps=4,   # Accumulate over 4 steps
    fp16=True,                       # Use mixed precision
)
```

#### 2. Slow Training on CPU

**Solution:**
- Use Google Colab with GPU
- Reduce dataset size for testing
- Use smaller model (GPT-2 small instead of medium)

#### 3. Import Errors

**Solution:**
```bash
# Reinstall specific package
pip install --upgrade transformers

# Or reinstall all
pip install -r requirements.txt --force-reinstall
```

#### 4. Tokenizer Warnings

**Solution:**
```python
import os
os.environ["TOKENIZERS_PARALLELISM"] = "false"
```

#### 5. Weights & Biases Login Issues

**Solution:**
```python
import os
os.environ['WANDB_DISABLED'] = 'true'  # Disable wandb if not needed
```

### Getting Help

If you encounter issues:

1. Check [HuggingFace Documentation](https://huggingface.co/docs)
2. Search [Stack Overflow](https://stackoverflow.com/questions/tagged/transformers)
3. Visit [HuggingFace Forums](https://discuss.huggingface.co/)
4. Review project issues on GitHub

## Project Structure After Setup

```
llm-domain-text-generation/
├── venv/                  # Virtual environment
├── notebooks/             # Jupyter notebooks
├── src/                   # Source code
├── data/                  # Data directory
├── results/               # Results will be saved here
├── checkpoints/           # Model checkpoints (created during training)
└── wandb/                 # W&B logs (if using)
```

## Next Steps

After setup is complete:

1. **Week 3:** Start with `notebooks/week3_baseline_implementation.ipynb`
2. **Week 4:** Run LoRA experiments in `notebooks/week4_lora_experiments.ipynb`
3. **Week 5:** Build deployment in `notebooks/week5_deployment.ipynb`
4. **Week 6:** Conduct testing in `notebooks/week6_ab_testing.ipynb`
5. **Week 8:** Launch demo with `notebooks/week8_dashboard_demo.ipynb`

## Performance Expectations

**Training Times (Google Colab Pro - Tesla T4):**
- Baseline GPT-2 fine-tuning: 2-4 hours
- LoRA experiments (4 configs): ~8 minutes per experiment
- Full Week 4 work session: ~3 hours
- Sampling strategy testing: 30-60 minutes

**Storage Requirements:**
- Base GPT-2 model: ~500MB
- Fine-tuned checkpoints: ~500MB each
- Datasets (cached): ~1GB
- Total project: ~5-10GB

---

*For additional support, refer to the main README.md or project documentation.*
