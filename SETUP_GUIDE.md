# Complete Setup Guide

This guide provides detailed step-by-step instructions for setting up the AI-Powered Regulatory Compliance Checker.

## Table of Contents

1. [System Requirements](#system-requirements)
2. [Python Environment Setup](#python-environment-setup)
3. [Dependency Installation](#dependency-installation)
4. [Model Download](#model-download)
5. [Optional Components](#optional-components)
6. [Configuration](#configuration)
7. [Verification](#verification)
8. [Troubleshooting](#troubleshooting)

## System Requirements

### Minimum Requirements
- **OS**: Windows 10/11, macOS 10.14+, or Linux (Ubuntu 18.04+)
- **Python**: 3.8 or higher
- **RAM**: 8 GB
- **Storage**: 5 GB free space (for models and dependencies)
- **Internet**: Required for initial model download

### Recommended Requirements
- **RAM**: 16 GB or more
- **GPU**: NVIDIA GPU with CUDA support (for faster processing)
- **Storage**: 10 GB free space

## Python Environment Setup

### Step 1: Check Python Version

```bash
python --version
```

You should see Python 3.8 or higher. If not, download from [python.org](https://www.python.org/downloads/).

### Step 2: Create Virtual Environment

Creating a virtual environment is **highly recommended** to avoid dependency conflicts.

**Windows:**
```bash
python -m venv myenv
myenv\Scripts\activate
```

**Linux/Mac:**
```bash
python3 -m venv myenv
source myenv/bin/activate
```

You should see `(myenv)` in your terminal prompt, indicating the environment is active.

### Step 3: Upgrade pip

```bash
python -m pip install --upgrade pip
```

## Dependency Installation

### Step 1: Navigate to Project Directory

```bash
cd path/to/App
```

### Step 2: Install Requirements

```bash
pip install -r requirements.txt
```

This will install all required packages:
- **streamlit**: Web interface framework
- **transformers**: Hugging Face transformers for LegalBERT
- **sentence-transformers**: Semantic embeddings
- **torch**: PyTorch for model inference
- **pandas**: Data manipulation
- **numpy**: Numerical operations
- **plotly**: Interactive charts
- **PyPDF2**: PDF text extraction
- **pdfplumber**: Advanced PDF processing
- **pytesseract**: OCR interface
- **Pillow**: Image processing
- **opencv-python**: Image preprocessing
- **python-docx**: DOCX file processing
- **reportlab**: PDF report generation
- **google-api-python-client**: Google Sheets integration
- **google-auth**: Google authentication

### Step 3: Verify Installation

```bash
pip list
```

Check that all packages are installed without errors.

## Model Download

### Automatic Download (Recommended)

Models will download automatically when you first run the application:

1. **LegalBERT** (~400 MB): Downloads on first clause classification
2. **Sentence Transformer** (~80 MB): Downloads on first embedding generation

The first run will take 5-10 minutes to download models.

### Manual Download (Optional)

If you prefer to download models manually:

```python
from transformers import AutoTokenizer, AutoModel
from sentence_transformers import SentenceTransformer

# Download LegalBERT
tokenizer = AutoTokenizer.from_pretrained("nlpaueb/legal-bert-base-uncased")
model = AutoModel.from_pretrained("nlpaueb/legal-bert-base-uncased")

# Download Sentence Transformer
embedder = SentenceTransformer('sentence-transformers/all-MiniLM-L6-v2')
```

### Model Storage Location

Models are stored in:
- **Windows**: `C:\Users\<username>\.cache\huggingface\`
- **Linux/Mac**: `~/.cache/huggingface/`

## Optional Components

### Tesseract OCR (for Image Processing)

Required only if you want to process scanned documents or images.

#### Windows

1. Download installer from: https://github.com/UB-Mannheim/tesseract/wiki
2. Run installer (recommended path: `C:\Program Files\Tesseract-OCR`)
3. Update `config/settings.py`:
   ```python
   TESSERACT_PATH = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
   ```

#### Linux (Ubuntu/Debian)

```bash
sudo apt-get update
sudo apt-get install tesseract-ocr
```

#### macOS

```bash
brew install tesseract
```

#### Verify Installation

```bash
tesseract --version
```

### CUDA for GPU Acceleration (Optional)

For faster model inference, install CUDA if you have an NVIDIA GPU.

1. Check GPU compatibility: https://developer.nvidia.com/cuda-gpus
2. Download CUDA Toolkit: https://developer.nvidia.com/cuda-downloads
3. Install PyTorch with CUDA:
   ```bash
   pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
   ```

Verify GPU is available:
```python
import torch
print(torch.cuda.is_available())  # Should print True
```

### Google Sheets Integration (Optional)

Required only if you want to import contracts from Google Sheets.

#### Step 1: Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project
3. Enable Google Sheets API

#### Step 2: Create Service Account

1. Go to "IAM & Admin" > "Service Accounts"
2. Click "Create Service Account"
3. Give it a name (e.g., "compliance-checker")
4. Grant "Editor" role
5. Click "Done"

#### Step 3: Create Credentials

1. Click on the service account you created
2. Go to "Keys" tab
3. Click "Add Key" > "Create new key"
4. Choose "JSON" format
5. Download the JSON file

#### Step 4: Configure Application

1. Rename downloaded file to `google_credentials.json`
2. Place in `App/config/` directory
3. Note the service account email (e.g., `compliance-checker@project.iam.gserviceaccount.com`)

#### Step 5: Share Your Sheet

1. Open your Google Sheet
2. Click "Share"
3. Add the service account email
4. Grant "Viewer" access

See `config/GOOGLE_SHEETS_SETUP.md` for detailed instructions.

## Configuration

### Basic Configuration

Edit `config/settings.py` to customize application behavior:

```python
# Debug mode
DEBUG = False

# Logging
LOG_LEVEL = "INFO"  # DEBUG, INFO, WARNING, ERROR, CRITICAL

# Model paths
LEGAL_BERT_MODEL = "nlpaueb/legal-bert-base-uncased"
SENTENCE_TRANSFORMER_MODEL = "sentence-transformers/all-MiniLM-L6-v2"

# Processing settings
MAX_FILE_SIZE_MB = 10
MIN_TEXT_LENGTH = 100
CONFIDENCE_THRESHOLD = 0.75

# GPU settings
USE_GPU = True  # Set to False to force CPU

# OCR settings (Windows example)
TESSERACT_PATH = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

### Advanced Configuration

For advanced users, you can modify:
- Batch sizes for processing
- Model inference parameters
- Similarity thresholds
- Risk level thresholds

## Verification

### Step 1: Run Application

```bash
streamlit run app.py
```

You should see:
```
You can now view your Streamlit app in your browser.
Local URL: http://localhost:8501
```

### Step 2: Test Basic Functionality

1. Open browser to `http://localhost:8501`
2. Upload a sample contract (or use text input)
3. Select at least one regulatory framework
4. Click "Analyze Contract"
5. Wait for analysis to complete

### Step 3: Verify Components

Check that all components work:
- ✅ Document upload and processing
- ✅ Clause segmentation
- ✅ NLP analysis
- ✅ Compliance checking
- ✅ Dashboard display
- ✅ Export functionality

### Step 4: Run Tests

```bash
# Test document processing
python test_document_processor.py

# Test NLP analysis
python test_nlp_analyzer.py

# Test compliance checking
python test_compliance_checker.py

# Test error handling
python test_error_handling.py
```

All tests should pass with "✅" indicators.

## Troubleshooting

### Issue: Python version too old

**Error**: `Python 3.8 or higher is required`

**Solution**:
1. Download Python 3.8+ from python.org
2. Install and verify: `python --version`
3. Recreate virtual environment

### Issue: pip install fails

**Error**: `Could not find a version that satisfies the requirement...`

**Solution**:
1. Upgrade pip: `python -m pip install --upgrade pip`
2. Try installing packages individually
3. Check internet connection

### Issue: Models not downloading

**Error**: `Connection timeout` or `Failed to download model`

**Solution**:
1. Check internet connection
2. Try manual download (see Model Download section)
3. Check firewall settings
4. Use VPN if behind corporate firewall

### Issue: Out of memory

**Error**: `CUDA out of memory` or `RuntimeError: out of memory`

**Solution**:
1. Close other applications
2. Reduce batch size in settings
3. Use smaller documents
4. Disable GPU: Set `USE_GPU = False` in settings

### Issue: Tesseract not found

**Error**: `TesseractNotFoundError`

**Solution**:
1. Install Tesseract OCR (see Optional Components)
2. Update `TESSERACT_PATH` in `config/settings.py`
3. Verify installation: `tesseract --version`

### Issue: Google Sheets connection fails

**Error**: `GoogleSheetsError: Authentication failed`

**Solution**:
1. Verify `google_credentials.json` is in `config/` directory
2. Check service account email is correct
3. Ensure sheet is shared with service account
4. Verify Google Sheets API is enabled

### Issue: Slow performance

**Symptoms**: Analysis takes very long

**Solution**:
1. Use GPU if available
2. Reduce number of frameworks
3. Process smaller documents
4. Check system resources (RAM, CPU)
5. Close other applications

### Issue: Streamlit won't start

**Error**: `ModuleNotFoundError: No module named 'streamlit'`

**Solution**:
1. Verify virtual environment is activated
2. Reinstall requirements: `pip install -r requirements.txt`
3. Check Python version

### Issue: Import errors

**Error**: `ImportError: cannot import name...`

**Solution**:
1. Verify all files are present
2. Check directory structure matches documentation
3. Reinstall dependencies
4. Check for typos in import statements

## Getting Help

If you encounter issues not covered here:

1. **Check Logs**: Look in `logs/` directory for detailed error messages
2. **Error Guide**: Review `ERROR_HANDLING_GUIDE.md` for common errors
3. **Documentation**: Check other documentation files
4. **Test Files**: Run test files to identify specific issues

## Next Steps

After successful setup:

1. Read `QUICK_START.md` for usage guide
2. Review `STREAMLIT_APP_USAGE_GUIDE.md` for UI details
3. Check `ERROR_HANDLING_GUIDE.md` for error information
4. Explore sample contracts in `samples/` (if available)

---

**Setup Complete!** You're ready to start analyzing contracts for compliance.
