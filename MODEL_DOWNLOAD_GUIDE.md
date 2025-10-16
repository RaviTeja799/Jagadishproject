# Model Download Guide

This guide explains how AI models are downloaded and managed in the Compliance Checker application.

## Overview

The application uses two main AI models:
1. **LegalBERT**: For clause classification (~400 MB)
2. **Sentence Transformer**: For semantic embeddings (~80 MB)

## Automatic Download (Recommended)

### How It Works

Models download automatically when first needed:
- **LegalBERT** downloads when you first analyze a contract
- **Sentence Transformer** downloads during the same first analysis

### First Run Process

1. Start the application: `streamlit run app.py`
2. Upload a contract and click "Analyze"
3. You'll see messages like:
   ```
   Downloading model: nlpaueb/legal-bert-base-uncased
   This may take a few minutes...
   ```
4. Wait for download to complete (5-10 minutes)
5. Models are cached for future use

### Download Location

Models are stored in your user cache directory:

**Windows:**
```
C:\Users\<username>\.cache\huggingface\hub\
```

**Linux/Mac:**
```
~/.cache/huggingface/hub/
```

## Manual Download (Advanced)

### Why Manual Download?

- Pre-download models before first use
- Download on a machine with better internet
- Transfer models to offline machine
- Troubleshoot download issues

### Method 1: Python Script

Create a file `download_models.py`:

```python
"""
Download all required models for the Compliance Checker.
Run this script before first use to pre-download models.
"""
from transformers import AutoTokenizer, AutoModel
from sentence_transformers import SentenceTransformer
import torch

print("=" * 60)
print("Downloading AI Models for Compliance Checker")
print("=" * 60)

# Download LegalBERT
print("\n1. Downloading LegalBERT...")
print("   Model: nlpaueb/legal-bert-base-uncased")
print("   Size: ~400 MB")
print("   Purpose: Clause classification")

try:
    tokenizer = AutoTokenizer.from_pretrained("nlpaueb/legal-bert-base-uncased")
    model = AutoModel.from_pretrained("nlpaueb/legal-bert-base-uncased")
    print("   ✅ LegalBERT downloaded successfully")
except Exception as e:
    print(f"   ❌ Error downloading LegalBERT: {e}")

# Download Sentence Transformer
print("\n2. Downloading Sentence Transformer...")
print("   Model: sentence-transformers/all-MiniLM-L6-v2")
print("   Size: ~80 MB")
print("   Purpose: Semantic embeddings")

try:
    embedder = SentenceTransformer('sentence-transformers/all-MiniLM-L6-v2')
    print("   ✅ Sentence Transformer downloaded successfully")
except Exception as e:
    print(f"   ❌ Error downloading Sentence Transformer: {e}")

# Verify GPU availability
print("\n3. Checking GPU availability...")
if torch.cuda.is_available():
    print(f"   ✅ GPU available: {torch.cuda.get_device_name(0)}")
    print(f"   Memory: {torch.cuda.get_device_properties(0).total_memory / 1e9:.2f} GB")
else:
    print("   ℹ️  No GPU detected, will use CPU")

print("\n" + "=" * 60)
print("Model download complete!")
print("=" * 60)
print("\nYou can now run the application:")
print("  streamlit run app.py")
```

Run the script:
```bash
python download_models.py
```

### Method 2: Interactive Python

```python
# Start Python interpreter
python

# Download LegalBERT
from transformers import AutoTokenizer, AutoModel
tokenizer = AutoTokenizer.from_pretrained("nlpaueb/legal-bert-base-uncased")
model = AutoModel.from_pretrained("nlpaueb/legal-bert-base-uncased")

# Download Sentence Transformer
from sentence_transformers import SentenceTransformer
embedder = SentenceTransformer('sentence-transformers/all-MiniLM-L6-v2')

# Exit Python
exit()
```

### Method 3: Hugging Face CLI

Install Hugging Face CLI:
```bash
pip install huggingface-hub
```

Download models:
```bash
# Download LegalBERT
huggingface-cli download nlpaueb/legal-bert-base-uncased

# Download Sentence Transformer
huggingface-cli download sentence-transformers/all-MiniLM-L6-v2
```

## Transferring Models to Offline Machine

### Step 1: Download on Online Machine

Use any manual download method above on a machine with internet.

### Step 2: Locate Downloaded Models

Find models in cache directory:
```bash
# Windows
dir "C:\Users\<username>\.cache\huggingface\hub"

# Linux/Mac
ls ~/.cache/huggingface/hub/
```

You'll see directories like:
- `models--nlpaueb--legal-bert-base-uncased`
- `models--sentence-transformers--all-MiniLM-L6-v2`

### Step 3: Copy to Offline Machine

1. Copy entire `huggingface` directory
2. Transfer to offline machine (USB, network, etc.)
3. Place in same cache location on offline machine

### Step 4: Verify on Offline Machine

```python
from transformers import AutoModel
from sentence_transformers import SentenceTransformer

# Should load from cache without internet
model = AutoModel.from_pretrained("nlpaueb/legal-bert-base-uncased")
embedder = SentenceTransformer('sentence-transformers/all-MiniLM-L6-v2')
```

## Model Information

### LegalBERT

**Full Name**: nlpaueb/legal-bert-base-uncased

**Description**: BERT model pre-trained on legal documents

**Size**: ~400 MB

**Purpose**: 
- Classify contract clauses into types
- Understand legal language context
- Generate clause representations

**Source**: https://huggingface.co/nlpaueb/legal-bert-base-uncased

**Citation**:
```
@inproceedings{chalkidis-etal-2020-legal,
    title = "{LEGAL}-{BERT}: The Muppets straight out of Law School",
    author = "Chalkidis, Ilias and Fergadiotis, Manos and Malakasiotis, Prodromos and Aletras, Nikolaos and Androutsopoulos, Ion",
    booktitle = "Findings of EMNLP 2020",
    year = "2020"
}
```

### Sentence Transformer

**Full Name**: sentence-transformers/all-MiniLM-L6-v2

**Description**: Efficient sentence embedding model

**Size**: ~80 MB

**Purpose**:
- Generate semantic embeddings for clauses
- Calculate similarity between clauses and requirements
- Enable semantic search

**Source**: https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2

**Performance**: 
- Speed: Very fast
- Quality: Good for general purpose
- Dimensions: 384

## Troubleshooting

### Download Fails

**Issue**: Connection timeout or network error

**Solutions**:
1. Check internet connection
2. Try again later (Hugging Face servers may be busy)
3. Use VPN if behind firewall
4. Download manually and transfer
5. Check disk space (need ~1 GB free)

### Slow Download

**Issue**: Download taking very long

**Solutions**:
1. Check internet speed
2. Download during off-peak hours
3. Use wired connection instead of WiFi
4. Close other applications using bandwidth

### Model Not Found

**Issue**: `Model not found` error

**Solutions**:
1. Check model name spelling
2. Verify internet connection
3. Clear cache and re-download:
   ```bash
   # Windows
   rmdir /s "C:\Users\<username>\.cache\huggingface"
   
   # Linux/Mac
   rm -rf ~/.cache/huggingface
   ```

### Corrupted Download

**Issue**: Model loads but gives errors

**Solutions**:
1. Delete cached model
2. Re-download:
   ```python
   from transformers import AutoModel
   model = AutoModel.from_pretrained(
       "nlpaueb/legal-bert-base-uncased",
       force_download=True
   )
   ```

### Disk Space Issues

**Issue**: Not enough space for models

**Solutions**:
1. Free up disk space (need ~1 GB)
2. Change cache location:
   ```python
   import os
   os.environ['TRANSFORMERS_CACHE'] = '/path/to/new/cache'
   ```

## Verifying Models

### Check if Models are Downloaded

```python
import os
from pathlib import Path

# Get cache directory
cache_dir = Path.home() / '.cache' / 'huggingface' / 'hub'

# Check for LegalBERT
legal_bert = list(cache_dir.glob('models--nlpaueb--legal-bert*'))
print(f"LegalBERT: {'✅ Found' if legal_bert else '❌ Not found'}")

# Check for Sentence Transformer
sent_trans = list(cache_dir.glob('models--sentence-transformers--all-MiniLM*'))
print(f"Sentence Transformer: {'✅ Found' if sent_trans else '❌ Not found'}")
```

### Test Model Loading

```python
from transformers import AutoModel
from sentence_transformers import SentenceTransformer
import time

# Test LegalBERT
print("Testing LegalBERT...")
start = time.time()
model = AutoModel.from_pretrained("nlpaueb/legal-bert-base-uncased")
print(f"✅ Loaded in {time.time() - start:.2f}s")

# Test Sentence Transformer
print("\nTesting Sentence Transformer...")
start = time.time()
embedder = SentenceTransformer('sentence-transformers/all-MiniLM-L6-v2')
print(f"✅ Loaded in {time.time() - start:.2f}s")

# Test inference
print("\nTesting inference...")
embedding = embedder.encode("This is a test sentence")
print(f"✅ Generated embedding of shape {embedding.shape}")
```

## Model Updates

### Checking for Updates

Models are versioned. To check for updates:

```python
from huggingface_hub import model_info

# Check LegalBERT
info = model_info("nlpaueb/legal-bert-base-uncased")
print(f"Last modified: {info.lastModified}")

# Check Sentence Transformer
info = model_info("sentence-transformers/all-MiniLM-L6-v2")
print(f"Last modified: {info.lastModified}")
```

### Updating Models

To force re-download latest version:

```python
from transformers import AutoModel

model = AutoModel.from_pretrained(
    "nlpaueb/legal-bert-base-uncased",
    force_download=True
)
```

## Alternative Models

### Using Different Models

You can use alternative models by editing `config/settings.py`:

```python
# Use a different BERT model
LEGAL_BERT_MODEL = "bert-base-uncased"  # General BERT

# Use a different sentence transformer
SENTENCE_TRANSFORMER_MODEL = "sentence-transformers/all-mpnet-base-v2"  # Higher quality
```

### Recommended Alternatives

**For Better Quality** (slower, larger):
- `nlpaueb/legal-bert-large-uncased` (1.3 GB)
- `sentence-transformers/all-mpnet-base-v2` (420 MB)

**For Faster Speed** (lower quality, smaller):
- `bert-base-uncased` (440 MB)
- `sentence-transformers/all-MiniLM-L12-v2` (120 MB)

## Summary

- Models download automatically on first use
- Total download size: ~500 MB
- Download time: 5-10 minutes (depends on internet speed)
- Models are cached for future use
- Manual download available for offline use
- Models can be transferred between machines

For issues, see Troubleshooting section or check application logs.
