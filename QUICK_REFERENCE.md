# Quick Reference Card

## 🚀 Getting Started

### Installation (3 steps)
```bash
python -m venv myenv
myenv\Scripts\activate  # Windows: myenv\Scripts\activate
pip install -r requirements.txt
```

### Run Application
```bash
streamlit run app.py
```
Opens at: `http://localhost:8501`

## 📁 Supported File Formats

| Format | Extension | Notes |
|--------|-----------|-------|
| PDF | .pdf | Text-based or scanned |
| Word | .docx | Microsoft Word documents |
| Text | .txt | Plain text files |
| Images | .png, .jpg | Requires Tesseract OCR |
| Google Sheets | URL | Requires credentials |

## 🏛️ Regulatory Frameworks

- **GDPR**: General Data Protection Regulation
- **HIPAA**: Health Insurance Portability and Accountability Act
- **CCPA**: California Consumer Privacy Act
- **SOX**: Sarbanes-Oxley Act

## 📊 Compliance Scores

| Score | Status | Action |
|-------|--------|--------|
| 90-100% | ✅ Excellent | Minor review |
| 75-89% | ✅ Good | Address issues |
| 50-74% | ⚠️ Moderate | Attention needed |
| 0-49% | ❌ Low | Immediate action |

## 🎨 Risk Levels

| Level | Color | Meaning |
|-------|-------|---------|
| 🔴 High | Red | Critical issues |
| 🟡 Medium | Yellow | Moderate issues |
| 🟢 Low | Green | Minor issues |

## 💻 Basic Usage

### 1. Upload Document
```
File Upload → Select file → Wait for processing
```

### 2. Select Frameworks
```
Sidebar → Check GDPR, HIPAA, CCPA, SOX
```

### 3. Analyze
```
Click "🚀 Analyze Contract" → Wait 5-30 seconds
```

### 4. Review Results
```
Dashboard Tab → View metrics
Clause Details Tab → See highlighted document
```

### 5. Export
```
Sidebar → Download JSON/CSV/PDF
```

## 🔧 Common Commands

### Test Installation
```bash
python --version  # Check Python version
pip list  # List installed packages
```

### Run Tests
```bash
python test_error_handling.py
python test_document_processor.py
python test_nlp_analyzer.py
```

### Check GPU
```python
import torch
print(torch.cuda.is_available())
```

## 📝 Code Examples

### Process Document
```python
from services.document_processor import DocumentProcessor

processor = DocumentProcessor()
doc = processor.process_document("contract.pdf")
print(f"Extracted {doc.num_clauses} clauses")
```

### Analyze Clauses
```python
from services.nlp_analyzer import NLPAnalyzer

analyzer = NLPAnalyzer()
analyses = analyzer.analyze_clauses(doc.clauses)
```

### Check Compliance
```python
from services.compliance_checker import ComplianceChecker

checker = ComplianceChecker()
report = checker.check_compliance(analyses, ['GDPR', 'HIPAA'])
print(f"Score: {report.overall_score}%")
```

## 🐛 Troubleshooting

### Models Not Downloading
```
Check internet connection
Wait 5-10 minutes for first download
Models cache in ~/.cache/huggingface/
```

### OCR Not Working
```
Install Tesseract OCR
Update path in config/settings.py
Verify: tesseract --version
```

### Out of Memory
```
Close other applications
Reduce batch size in settings
Use smaller documents
Disable GPU: USE_GPU = False
```

### Slow Performance
```
Use GPU if available
Reduce number of frameworks
Process smaller documents
Check system resources
```

## 📚 Documentation Files

| File | Purpose |
|------|---------|
| README.md | Overview and quick start |
| SETUP_GUIDE.md | Detailed setup instructions |
| API_DOCUMENTATION.md | Complete API reference |
| ERROR_HANDLING_GUIDE.md | Error handling details |
| MODEL_DOWNLOAD_GUIDE.md | Model download info |
| QUICK_START.md | Quick start guide |
| STREAMLIT_APP_USAGE_GUIDE.md | UI usage guide |

## 🔑 Key Directories

```
App/
├── app.py              # Main application
├── services/           # Core business logic
├── models/             # Data models
├── utils/              # Utilities
├── config/             # Configuration
├── data/               # Regulatory requirements
└── logs/               # Application logs
```

## ⚙️ Configuration

### Edit config/settings.py
```python
# Model settings
LEGAL_BERT_MODEL = "nlpaueb/legal-bert-base-uncased"
SENTENCE_TRANSFORMER_MODEL = "sentence-transformers/all-MiniLM-L6-v2"

# Processing settings
MAX_FILE_SIZE_MB = 10
CONFIDENCE_THRESHOLD = 0.75

# GPU settings
USE_GPU = True

# OCR settings (Windows)
TESSERACT_PATH = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

## 📞 Getting Help

1. Check error message in UI
2. Review troubleshooting tips
3. Check logs in `logs/` directory
4. Review ERROR_HANDLING_GUIDE.md
5. Check documentation files

## ✅ Quick Checklist

Before first use:
- [ ] Python 3.8+ installed
- [ ] Virtual environment created
- [ ] Dependencies installed
- [ ] (Optional) Tesseract OCR installed
- [ ] Application starts without errors

For analysis:
- [ ] Document uploaded or text pasted
- [ ] At least one framework selected
- [ ] Analysis completes successfully
- [ ] Results display correctly
- [ ] Export works if needed

## 🎯 Performance Tips

1. **Use GPU**: 5-10x faster
2. **Batch Processing**: Enabled by default
3. **Model Caching**: Automatic
4. **Limit Frameworks**: Check only needed ones
5. **Text PDFs**: Faster than scanned

## 🔒 Security Notes

- ✅ Local processing only
- ✅ No data sent externally
- ✅ No permanent storage
- ✅ Sanitized logging
- ✅ Secure authentication

## 📊 Typical Workflow

```
Upload → Select Frameworks → Analyze → Review → Export
  ↓           ↓                ↓         ↓        ↓
5-10s      Instant          5-30s    Review   Instant
```

## 🎓 Learning Path

1. **Start**: README.md
2. **Setup**: SETUP_GUIDE.md
3. **Use**: QUICK_START.md
4. **Advanced**: API_DOCUMENTATION.md
5. **Troubleshoot**: ERROR_HANDLING_GUIDE.md

---

**Quick Help**: For detailed information, see the full documentation files.

**Version**: 1.0.0  
**Last Updated**: October 15, 2025
