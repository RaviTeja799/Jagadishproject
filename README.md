# AI-Powered Regulatory Compliance Checker

An intelligent system for analyzing contracts and legal documents for regulatory compliance using AI and NLP technologies.

## 🎯 Overview

This application automatically analyzes legal contracts and documents to identify compliance issues across multiple regulatory frameworks (GDPR, HIPAA, CCPA, SOX). It uses advanced NLP models to classify clauses, detect missing requirements, and generate AI-powered recommendations for compliance improvements.

## ✨ Key Features

### Document Processing
- **Multi-format Support**: PDF, DOCX, TXT, PNG, JPG
- **OCR Capability**: Extract text from scanned documents and images
- **Smart Segmentation**: Automatically identify and segment contract clauses
- **Google Sheets Integration**: Import contracts directly from Google Sheets

### AI-Powered Analysis
- **LegalBERT Classification**: Classify clauses using specialized legal language models
- **Semantic Analysis**: Generate embeddings for semantic similarity matching
- **Multi-Framework Compliance**: Check against GDPR, HIPAA, CCPA, and SOX simultaneously
- **Risk Assessment**: Automatic risk level assignment (High, Medium, Low)

### Intelligent Recommendations
- **Gap Detection**: Identify missing mandatory clauses
- **AI Suggestions**: Generate compliant clause text using LLaMA
- **Prioritized Actions**: Recommendations sorted by risk and impact
- **Regulatory References**: Include specific regulatory citations

### Interactive Interface
- **Visual Highlighting**: Color-coded risk highlighting in documents
- **Interactive Navigation**: Click clauses to see detailed analysis
- **Real-time Progress**: Track analysis progress with visual indicators
- **Comprehensive Dashboard**: View compliance metrics and trends

### Export & Reporting
- **Multiple Formats**: Export to JSON, CSV, and PDF
- **Detailed Reports**: Include executive summary and clause-level details
- **Compliance Charts**: Visual representation of compliance status

## 🚀 Quick Start

### Prerequisites

- Python 3.8 or higher
- pip package manager
- (Optional) Tesseract OCR for image processing
- (Optional) CUDA-capable GPU for faster model inference

### Installation

1. **Create virtual environment** (recommended)
```bash
python -m venv myenv
# Windows
myenv\Scripts\activate
# Linux/Mac
source myenv/bin/activate
```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```

3. **Install Tesseract OCR** (optional, for image processing)
   - Windows: Download from https://github.com/UB-Mannheim/tesseract/wiki
   - Linux: `sudo apt-get install tesseract-ocr`
   - Mac: `brew install tesseract`

4. **Download AI Models** (automatic on first run)
   - LegalBERT: `nlpaueb/legal-bert-base-uncased`
   - Sentence Transformer: `sentence-transformers/all-MiniLM-L6-v2`
   - Models will be downloaded automatically when first used

### Running the Application

```bash
streamlit run app.py
```

The application will open in your default browser at `http://localhost:8501`

## 📖 User Guide

### Analyzing a Contract

1. **Upload Document**
   - Click "File Upload" and select your contract (PDF, DOCX, TXT, or image)
   - Or paste contract text directly
   - Or provide a Google Sheets URL

2. **Select Regulatory Frameworks**
   - Check the frameworks you want to analyze against (GDPR, HIPAA, CCPA, SOX)
   - At least one framework must be selected

3. **Analyze**
   - Click "🚀 Analyze Contract"
   - Wait for the analysis to complete (typically 5-30 seconds)

4. **Review Results**
   - **Dashboard Tab**: View overall compliance score and metrics
   - **Clause Details Tab**: See highlighted document with risk-coded clauses
   - Click on any clause to see detailed analysis and recommendations

5. **Export Results**
   - Download JSON for programmatic access
   - Download CSV for spreadsheet analysis
   - Download PDF for formal reporting

### Understanding Results

#### Compliance Scores
- **90-100%**: Excellent compliance
- **75-89%**: Good compliance, minor issues
- **50-74%**: Moderate compliance, attention needed
- **Below 50%**: Low compliance, immediate action required

#### Risk Levels
- **🔴 High Risk**: Critical compliance issues, immediate action required
- **🟡 Medium Risk**: Moderate issues, should be addressed
- **🟢 Low Risk**: Minor issues or best practice recommendations

#### Compliance Status
- **✅ Compliant**: Clause meets regulatory requirements
- **❌ Non-Compliant**: Clause violates or lacks required elements
- **⚠️ Partial**: Clause partially meets requirements

## 🏗️ Project Structure

```
App/
├── app.py                      # Main Streamlit application
├── requirements.txt            # Python dependencies
├── README.md                   # This file
│
├── services/                   # Core business logic
│   ├── document_processor.py  # Document processing orchestrator
│   ├── pdf_extractor.py       # PDF text extraction
│   ├── ocr_extractor.py       # OCR for images
│   ├── clause_segmenter.py    # Clause segmentation
│   ├── nlp_analyzer.py        # NLP analysis orchestrator
│   ├── legal_bert_classifier.py # LegalBERT classification
│   ├── embedding_generator.py # Semantic embeddings
│   ├── compliance_checker.py  # Compliance checking orchestrator
│   ├── regulatory_knowledge_base.py # Regulatory requirements
│   ├── compliance_rule_engine.py # Compliance rules
│   ├── compliance_assessor.py # Clause assessment
│   ├── compliance_scorer.py   # Scoring algorithms
│   ├── recommendation_engine.py # Recommendation orchestrator
│   ├── legal_llama.py         # LLaMA integration
│   ├── prompt_builder.py      # Prompt templates
│   ├── clause_generator.py    # Clause text generation
│   ├── recommendation_generator.py # Recommendation generation
│   ├── export_service.py      # Export functionality
│   ├── google_sheets_service.py # Google Sheets integration
│   └── document_viewer.py     # Document visualization
│
├── models/                     # Data models
│   ├── clause.py              # Clause data model
│   ├── processed_document.py  # Processed document model
│   ├── clause_analysis.py     # Analysis result model
│   ├── regulatory_requirement.py # Compliance models
│   └── recommendation.py      # Recommendation model
│
├── utils/                      # Utility functions
│   ├── logger.py              # Logging utility
│   ├── error_handler.py       # Error handling system
│   └── user_feedback.py       # User feedback utilities
│
├── config/                     # Configuration
│   └── settings.py            # Application settings
│
├── data/                       # Regulatory requirements
│   ├── gdpr_requirements.py   # GDPR requirements
│   ├── hipaa_requirements.py  # HIPAA requirements
│   ├── ccpa_requirements.py   # CCPA requirements
│   └── sox_requirements.py    # SOX requirements
│
└── logs/                       # Application logs
```

## 🔧 Configuration

### Application Settings

Edit `config/settings.py` to customize:

```python
# Model settings
LEGAL_BERT_MODEL = "nlpaueb/legal-bert-base-uncased"
SENTENCE_TRANSFORMER_MODEL = "sentence-transformers/all-MiniLM-L6-v2"

# Processing settings
MAX_FILE_SIZE_MB = 10
MIN_TEXT_LENGTH = 100
CONFIDENCE_THRESHOLD = 0.75

# OCR settings (Windows example)
TESSERACT_PATH = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

### Google Sheets Integration

1. Create a Google Cloud Project
2. Enable Google Sheets API
3. Create service account credentials
4. Download credentials JSON
5. Place in `config/google_credentials.json`
6. Share your sheet with the service account email

See `config/GOOGLE_SHEETS_SETUP.md` for detailed instructions.

## 🧪 Testing

Run tests to verify functionality:

```bash
# Test document processing
python test_document_processor.py

# Test NLP analysis
python test_nlp_analyzer.py

# Test compliance checking
python test_compliance_checker.py

# Test error handling
python test_error_handling.py

# Test export functionality
python test_export_service.py
```

## 🐛 Troubleshooting

### Common Issues

**Issue**: Models not downloading
- **Solution**: Check internet connection, models download automatically on first use

**Issue**: OCR not working
- **Solution**: Install Tesseract OCR and update path in `config/settings.py`

**Issue**: Out of memory errors
- **Solution**: Reduce batch size in settings or use smaller documents

**Issue**: Slow performance
- **Solution**: Use GPU if available, reduce number of frameworks, or process smaller documents

**Issue**: Google Sheets connection fails
- **Solution**: Verify credentials, check sheet sharing, ensure API is enabled

### Error Messages

The application provides detailed error messages with troubleshooting tips. Check:
- Error message displayed in the UI
- Expandable troubleshooting tips section
- Log files in `logs/` directory
- `ERROR_HANDLING_GUIDE.md` for comprehensive error documentation

## 📊 Performance

### Typical Processing Times

- **Small contract** (5-10 pages): 5-10 seconds
- **Medium contract** (10-30 pages): 10-30 seconds
- **Large contract** (30+ pages): 30-60 seconds

### Optimization Tips

1. **Use GPU**: Significantly faster model inference
2. **Batch Processing**: Process multiple clauses together
3. **Cache Models**: Models are cached after first load
4. **Limit Frameworks**: Check only needed frameworks
5. **Pre-process Documents**: Use text-based PDFs when possible

## 🔒 Security & Privacy

- **Local Processing**: All analysis runs locally, no data sent to external servers
- **No Data Storage**: Documents are not permanently stored
- **Secure Logging**: Sensitive data is sanitized in logs
- **API Security**: Google Sheets uses service account authentication

## 📚 Additional Documentation

- **ERROR_HANDLING_GUIDE.md**: Comprehensive error handling documentation
- **STREAMLIT_APP_USAGE_GUIDE.md**: Detailed UI usage guide
- **MULTI_SOURCE_INPUT_GUIDE.md**: Multi-source input documentation
- **config/GOOGLE_SHEETS_SETUP.md**: Google Sheets integration setup
- **QUICK_START.md**: Quick start guide for new users

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## 📝 License

Copyright © 2025 AI Compliance Checker

## 🙏 Acknowledgments

- **LegalBERT**: nlpaueb/legal-bert-base-uncased
- **Sentence Transformers**: sentence-transformers library
- **Streamlit**: Interactive web framework
- **Hugging Face**: Model hosting and transformers library

## 📧 Support

For issues, questions, or suggestions:
- Check the documentation files
- Review error handling guide
- Check application logs in `logs/` directory

## 🗺️ Roadmap

Future enhancements:
- [ ] Additional regulatory frameworks (PCI-DSS, ISO 27001)
- [ ] Multi-language support
- [ ] Contract comparison features
- [ ] Automated compliance monitoring
- [ ] API for programmatic access
- [ ] Cloud deployment options

---

**Version**: 1.0.0  
**Last Updated**: October 15, 2025
#   I n f o s y s - P r o j e c t  
 #   J a g a d i s h p r o j e c t  
 #   J a g a d i s h p r o j e c t  
 #   J a g a d i s h p r o j e c t  
 