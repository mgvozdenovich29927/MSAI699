# Dataset Information

## WikiText-103

This project uses the **WikiText-103** dataset for training and evaluation.

### Overview

- **Name:** WikiText-103
- **Source:** HuggingFace Datasets
- **Size:** ~103 million tokens
- **Domain:** General English text from Wikipedia articles
- **License:** Creative Commons Attribution-ShareAlike

### Dataset Statistics

- **Total Articles:** ~28,000 Wikipedia articles
- **Training Samples Used:** 10,000 (filtered for quality)
- **Validation Samples Used:** 1,000
- **Average Token Length:** Variable (filtered for >50 characters)
- **Vocabulary Size:** ~267,000 tokens

### Why WikiText-103?

1. **Quality:** Professionally written, edited Wikipedia content
2. **Diversity:** Covers wide range of topics and writing styles
3. **Size:** Large enough for effective fine-tuning, manageable for academic project
4. **Accessibility:** Freely available via HuggingFace Datasets
5. **Benchmark:** Widely used in NLP research for language modeling

### Loading the Dataset

```python
from datasets import load_dataset

# Load full dataset
dataset = load_dataset("wikitext", "wikitext-103-v1")

# Access splits
train_data = dataset["train"]
validation_data = dataset["validation"]
test_data = dataset["test"]
```

### Preprocessing Steps

1. **Filtering:** Remove samples with <50 characters
2. **Cleaning:** Basic text normalization
3. **Tokenization:** GPT-2 tokenizer (byte-pair encoding)
4. **Chunking:** Split long sequences to max_length=512 tokens

### Data Directory Structure

```
data/
├── README.md          # This file
├── raw/               # Original downloaded data (not in repo)
├── processed/         # Preprocessed data (not in repo)
└── sample_outputs/    # Example model generations
```

### Sample Data

Example from WikiText-103:

```
The Valkyrian Dynasty is a series of console role-playing games created 
by Sega. The series began with Valkyria Chronicles in 2008, which was 
developed by Sega's internal development studio and published by Sega...
```

### Citation

If using this dataset in research, please cite:

```bibtex
@article{merity2016pointer,
  title={Pointer sentinel mixture models},
  author={Merity, Stephen and Xiong, Caiming and Bradbury, James and Socher, Richard},
  journal={arXiv preprint arXiv:1609.07843},
  year={2016}
}
```

### Alternative Datasets Considered

- **PubMed:** Medical/scientific domain (requires domain expertise)
- **Legal Corpora:** Legal documents (complex preprocessing)
- **OpenWebText:** Larger scale (higher computational requirements)

### Data Storage Notes

 **Note:** Raw and processed data files are not included in this repository due to size constraints. The dataset can be automatically downloaded using the HuggingFace `datasets` library as shown in the loading example above.

### Privacy and Ethics

- WikiText-103 consists of publicly available Wikipedia articles
- All content follows Wikipedia's Creative Commons licensing
- No personal or sensitive information
- Articles are curated and edited by Wikipedia community

