# API Documentation

This document provides detailed API documentation for all services and models in the Compliance Checker application.

## Table of Contents

1. [Document Processing](#document-processing)
2. [NLP Analysis](#nlp-analysis)
3. [Compliance Checking](#compliance-checking)
4. [Recommendations](#recommendations)
5. [Export Services](#export-services)
6. [Data Models](#data-models)
7. [Utilities](#utilities)

---

## Document Processing

### DocumentProcessor

Main orchestrator for document processing pipeline.

```python
from services.document_processor import DocumentProcessor

processor = DocumentProcessor()
```

#### Methods

**process_document(file_path, file_type=None, use_ocr=False)**

Process a document through the complete pipeline.

Parameters:
- `file_path` (str): Path to the document file
- `file_type` (str, optional): File type override ('pdf', 'image', 'text', 'docx')
- `use_ocr` (bool): Force OCR extraction even for text-based PDFs

Returns:
- `ProcessedDocument`: Document with extracted text and segmented clauses

Raises:
- `DocumentProcessingError`: If processing fails
- `UnsupportedFormatError`: If file format is not supported
- `EmptyDocumentError`: If document has no content

Example:
```python
doc = processor.process_document("contract.pdf")
print(f"Extracted {doc.num_clauses} clauses")
```

**process_text(text, filename="pasted_text.txt")**

Process raw text input.

Parameters:
- `text` (str): Raw contract text
- `filename` (str): Optional filename for reference

Returns:
- `ProcessedDocument`: Document with segmented clauses

Example:
```python
text = "This is a contract..."
doc = processor.process_text(text)
```

**process_google_sheet(url, sheet_name=None, cell_range=None)**

Process contract text from Google Sheets.

Parameters:
- `url` (str): Google Sheets URL
- `sheet_name` (str, optional): Sheet name (defaults to first sheet)
- `cell_range` (str, optional): Cell range (e.g., 'A1:B10')

Returns:
- `ProcessedDocument`: Document with segmented clauses

Example:
```python
url = "https://docs.google.com/spreadsheets/d/abc123/edit"
doc = processor.process_google_sheet(url)
```

---

## NLP Analysis

### NLPAnalyzer

Orchestrator for NLP analysis including classification and embedding.

```python
from services.nlp_analyzer import NLPAnalyzer

analyzer = NLPAnalyzer(confidence_threshold=0.75)
```

#### Methods

**analyze_clause(clause)**

Analyze a single clause.

Parameters:
- `clause` (Clause): Clause to analyze

Returns:
- `ClauseAnalysis`: Analysis with classification and embedding

Example:
```python
analysis = analyzer.analyze_clause(clause)
print(f"Type: {analysis.clause_type}, Confidence: {analysis.confidence_score}")
```

**analyze_clauses(clauses, batch_size=32)**

Analyze multiple clauses with batch processing.

Parameters:
- `clauses` (List[Clause]): List of clauses to analyze
- `batch_size` (int): Batch size for embedding generation

Returns:
- `List[ClauseAnalysis]`: List of analysis results

Example:
```python
analyses = analyzer.analyze_clauses(doc.clauses)
```

**get_low_confidence_clauses(analyses)**

Filter clauses with confidence below threshold.

Parameters:
- `analyses` (List[ClauseAnalysis]): List of clause analyses

Returns:
- `List[ClauseAnalysis]`: Low confidence analyses

**get_analysis_summary(analyses)**

Get summary statistics for analyses.

Parameters:
- `analyses` (List[ClauseAnalysis]): List of clause analyses

Returns:
- `dict`: Summary statistics

Example:
```python
summary = analyzer.get_analysis_summary(analyses)
print(f"Average confidence: {summary['avg_confidence']}")
```

---

## Compliance Checking

### ComplianceChecker

Main orchestrator for compliance checking.

```python
from services.compliance_checker import ComplianceChecker

checker = ComplianceChecker()
```

#### Methods

**check_compliance(clauses, frameworks, document_id="unknown")**

Check compliance against multiple regulatory frameworks.

Parameters:
- `clauses` (List[ClauseAnalysis]): Analyzed clauses with embeddings
- `frameworks` (List[str]): Frameworks to check (e.g., ['GDPR', 'HIPAA'])
- `document_id` (str): Document identifier

Returns:
- `ComplianceReport`: Comprehensive compliance analysis

Example:
```python
report = checker.check_compliance(
    clauses=analyses,
    frameworks=['GDPR', 'HIPAA'],
    document_id="contract_001"
)
print(f"Overall score: {report.overall_score}%")
```

**check_single_framework(clauses, framework, document_id="unknown")**

Check compliance against a single framework.

Parameters:
- `clauses` (List[ClauseAnalysis]): Analyzed clauses
- `framework` (str): Framework to check
- `document_id` (str): Document identifier

Returns:
- `ComplianceReport`: Compliance report for the framework

**quick_check(clauses, frameworks)**

Perform quick compliance check returning scores only.

Parameters:
- `clauses` (List[ClauseAnalysis]): Analyzed clauses
- `frameworks` (List[str]): Frameworks to check

Returns:
- `Dict[str, float]`: Framework names mapped to scores

Example:
```python
scores = checker.quick_check(analyses, ['GDPR', 'HIPAA'])
print(f"GDPR: {scores['GDPR']}%")
```

**get_missing_requirements_for_framework(clauses, framework)**

Get missing requirements for a specific framework.

Parameters:
- `clauses` (List[ClauseAnalysis]): Analyzed clauses
- `framework` (str): Framework to check

Returns:
- `List[RegulatoryRequirement]`: Missing mandatory requirements

---

## Recommendations

### RecommendationEngine

Orchestrator for generating AI-powered recommendations.

```python
from services.recommendation_engine import RecommendationEngine

engine = RecommendationEngine(use_llama=False)
```

#### Methods

**generate_recommendations(compliance_report)**

Generate recommendations based on compliance report.

Parameters:
- `compliance_report` (ComplianceReport): Compliance analysis results

Returns:
- `List[Recommendation]`: Prioritized recommendations

Example:
```python
recommendations = engine.generate_recommendations(report)
for rec in recommendations:
    print(f"{rec.priority}: {rec.title}")
```

**generate_clause_text(requirement, context="")**

Generate compliant clause text for a requirement.

Parameters:
- `requirement` (RegulatoryRequirement): Requirement to generate clause for
- `context` (str): Additional context

Returns:
- `str`: Generated clause text

Example:
```python
clause_text = engine.generate_clause_text(requirement)
```

---

## Export Services

### ExportService

Service for exporting compliance reports.

```python
from services.export_service import ExportService

exporter = ExportService()
```

#### Methods

**export_to_json(compliance_report, recommendations)**

Export to JSON format.

Parameters:
- `compliance_report` (ComplianceReport): Compliance report
- `recommendations` (List[Recommendation]): Recommendations

Returns:
- `str`: JSON string

Example:
```python
json_data = exporter.export_to_json(report, recommendations)
```

**export_to_csv(compliance_report, recommendations)**

Export to CSV format.

Parameters:
- `compliance_report` (ComplianceReport): Compliance report
- `recommendations` (List[Recommendation]): Recommendations

Returns:
- `str`: CSV string

**export_to_pdf(compliance_report, recommendations)**

Export to PDF format.

Parameters:
- `compliance_report` (ComplianceReport): Compliance report
- `recommendations` (List[Recommendation]): Recommendations

Returns:
- `bytes`: PDF binary data

---

## Data Models

### Clause

Represents a contract clause.

```python
from models.clause import Clause

clause = Clause(
    clause_id="clause_1",
    text="This agreement shall...",
    position=0,
    section="1.1"
)
```

Attributes:
- `clause_id` (str): Unique identifier
- `text` (str): Clause text
- `position` (int): Position in document
- `section` (str, optional): Section number

### ProcessedDocument

Represents a processed document.

```python
from models.processed_document import ProcessedDocument

doc = ProcessedDocument(
    document_id="doc_001",
    original_filename="contract.pdf",
    extracted_text="Full text...",
    clauses=[clause1, clause2],
    metadata={},
    processing_time=2.5
)
```

Attributes:
- `document_id` (str): Unique identifier
- `original_filename` (str): Original file name
- `extracted_text` (str): Full extracted text
- `clauses` (List[Clause]): Segmented clauses
- `metadata` (dict): Processing metadata
- `processing_time` (float): Processing time in seconds

Properties:
- `num_clauses` (int): Number of clauses
- `total_words` (int): Total word count

### ClauseAnalysis

Represents NLP analysis results for a clause.

```python
from models.clause_analysis import ClauseAnalysis

analysis = ClauseAnalysis(
    clause_id="clause_1",
    clause_text="Text...",
    clause_type="Data Processing",
    confidence_score=0.95,
    embeddings=[0.1, 0.2, ...],
    alternative_types=[("Other", 0.05)]
)
```

Attributes:
- `clause_id` (str): Clause identifier
- `clause_text` (str): Clause text
- `clause_type` (str): Classified type
- `confidence_score` (float): Classification confidence (0-1)
- `embeddings` (List[float]): Semantic embedding vector
- `alternative_types` (List[Tuple[str, float]]): Alternative classifications

### ComplianceReport

Represents comprehensive compliance analysis.

```python
from models.regulatory_requirement import ComplianceReport

report = ComplianceReport(
    document_id="doc_001",
    frameworks_checked=['GDPR', 'HIPAA'],
    overall_score=85.5,
    clause_results=[...],
    missing_requirements=[...],
    high_risk_items=[...],
    summary=summary_obj
)
```

Attributes:
- `document_id` (str): Document identifier
- `frameworks_checked` (List[str]): Frameworks analyzed
- `overall_score` (float): Overall compliance score (0-100)
- `clause_results` (List[ClauseComplianceResult]): Clause-level results
- `missing_requirements` (List[RegulatoryRequirement]): Missing requirements
- `high_risk_items` (List[ClauseComplianceResult]): High-risk items
- `summary` (ComplianceSummary): Summary statistics

### Recommendation

Represents an AI-generated recommendation.

```python
from models.recommendation import Recommendation, ActionType, Priority

rec = Recommendation(
    recommendation_id="rec_001",
    clause_id="clause_1",
    action_type=ActionType.MODIFY,
    priority=Priority.HIGH,
    title="Update data processing clause",
    description="The clause lacks...",
    suggested_text="Updated clause text...",
    regulatory_references=["GDPR Art. 28"],
    rationale="This change is needed because..."
)
```

Attributes:
- `recommendation_id` (str): Unique identifier
- `clause_id` (str, optional): Related clause ID
- `action_type` (ActionType): Type of action (ADD, MODIFY, REMOVE, REVIEW)
- `priority` (Priority): Priority level (HIGH, MEDIUM, LOW)
- `title` (str): Recommendation title
- `description` (str): Detailed description
- `suggested_text` (str, optional): Suggested clause text
- `regulatory_references` (List[str]): Regulatory citations
- `rationale` (str): Explanation of recommendation

---

## Utilities

### Logger

Logging utility with sensitive data sanitization.

```python
from utils.logger import get_logger

logger = get_logger(__name__)
logger.info("Processing document")
logger.error("Error occurred", exc_info=True)
```

### Error Handler

Comprehensive error handling system.

```python
from utils.error_handler import (
    InputValidator,
    FallbackHandler,
    ErrorMessageFormatter
)

# Validate input
InputValidator.validate_file_size(file_size, max_size_mb=10)

# Create fallback
fallback = FallbackHandler.create_fallback_clause_analysis(clause_id, text)

# Format error for UI
formatted = ErrorMessageFormatter.format_for_ui(exception)
```

### User Feedback

User feedback utilities for Streamlit.

```python
from utils.user_feedback import UserFeedback, ProgressTracker

# Show messages
UserFeedback.show_success("Operation completed")
UserFeedback.show_error("Error occurred", error=e, troubleshooting_tips=tips)

# Track progress
tracker = ProgressTracker(total_steps=3, description="Analyzing")
tracker.update(1, "Step 1...")
tracker.complete("Done!")
```

---

## Complete Example

Here's a complete example using the API:

```python
from services.document_processor import DocumentProcessor
from services.nlp_analyzer import NLPAnalyzer
from services.compliance_checker import ComplianceChecker
from services.recommendation_engine import RecommendationEngine
from services.export_service import ExportService

# Initialize services
doc_processor = DocumentProcessor()
nlp_analyzer = NLPAnalyzer()
compliance_checker = ComplianceChecker()
recommendation_engine = RecommendationEngine()
export_service = ExportService()

# Process document
processed_doc = doc_processor.process_document("contract.pdf")
print(f"Extracted {processed_doc.num_clauses} clauses")

# Analyze clauses
analyses = nlp_analyzer.analyze_clauses(processed_doc.clauses)
print(f"Analyzed {len(analyses)} clauses")

# Check compliance
report = compliance_checker.check_compliance(
    clauses=analyses,
    frameworks=['GDPR', 'HIPAA'],
    document_id=processed_doc.document_id
)
print(f"Compliance score: {report.overall_score}%")

# Generate recommendations
recommendations = recommendation_engine.generate_recommendations(report)
print(f"Generated {len(recommendations)} recommendations")

# Export results
json_data = export_service.export_to_json(report, recommendations)
pdf_data = export_service.export_to_pdf(report, recommendations)

print("Analysis complete!")
```

---

## Error Handling

All services raise specific exceptions for different error conditions:

```python
from utils.error_handler import (
    DocumentProcessingError,
    UnsupportedFormatError,
    ModelError,
    ComplianceCheckError
)

try:
    doc = processor.process_document("file.xyz")
except UnsupportedFormatError as e:
    print(f"Unsupported format: {e.user_message}")
except DocumentProcessingError as e:
    print(f"Processing error: {e.user_message}")
```

See `ERROR_HANDLING_GUIDE.md` for comprehensive error documentation.

---

## Performance Considerations

### Batch Processing

Process multiple clauses together for better performance:

```python
# Good: Batch processing
analyses = analyzer.analyze_clauses(all_clauses, batch_size=32)

# Avoid: Individual processing
analyses = [analyzer.analyze_clause(c) for c in all_clauses]
```

### Caching

Models and embeddings are automatically cached:

```python
# First call: Downloads and caches model
analyzer1 = NLPAnalyzer()

# Subsequent calls: Uses cached model
analyzer2 = NLPAnalyzer()  # Fast!
```

### GPU Acceleration

Enable GPU for faster inference:

```python
# In config/settings.py
USE_GPU = True

# Check if GPU is being used
import torch
print(f"Using GPU: {torch.cuda.is_available()}")
```

---

For more information, see:
- `README.md`: General overview
- `SETUP_GUIDE.md`: Setup instructions
- `ERROR_HANDLING_GUIDE.md`: Error handling details
- `MODEL_DOWNLOAD_GUIDE.md`: Model download information
