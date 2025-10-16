# Error Handling and User Feedback Guide

## Overview

This document describes the comprehensive error handling and user feedback system implemented in the AI-Powered Regulatory Compliance Checker application.

## Architecture

The error handling system consists of three main components:

1. **Custom Exception Classes** (`utils/error_handler.py`)
2. **User Feedback Utilities** (`utils/user_feedback.py`)
3. **Integration in Services** (document_processor, nlp_analyzer, compliance_checker)

## Custom Exception Hierarchy

### Base Exception

```python
ComplianceCheckerError
├── DocumentProcessingError
│   ├── UnsupportedFormatError
│   ├── FileReadError
│   ├── OCRError
│   └── TextExtractionError
├── ModelError
│   ├── ModelLoadError
│   ├── InferenceError
│   └── LowConfidenceError
├── ComplianceCheckError
│   └── RegulatoryDataMissingError
├── ValidationError
│   ├── InvalidInputError
│   ├── FileSizeError
│   └── EmptyDocumentError
└── ExternalServiceError
    └── GoogleSheetsError
```

### Exception Features

Each custom exception includes:
- **Category**: Classification of error type (document_processing, model_inference, etc.)
- **Severity**: Error severity level (low, medium, high, critical)
- **User Message**: User-friendly error message
- **Details**: Additional context and debugging information

## Error Categories

### 1. Document Processing Errors

**UnsupportedFormatError**
- Raised when: File format is not supported
- User Message: "The uploaded file format is not supported. Please upload a PDF, DOCX, TXT, PNG, or JPG file."
- Troubleshooting Tips:
  - Ensure your file is in PDF, DOCX, TXT, PNG, or JPG format
  - Try converting your file to PDF format
  - Check that the file extension matches the actual file type

**FileReadError**
- Raised when: File cannot be read
- User Message: "Unable to read the uploaded file. The file may be corrupted or password-protected."
- Troubleshooting Tips:
  - Verify the file is not corrupted
  - If the file is password-protected, remove the password first
  - Try re-saving the file and uploading again

**OCRError**
- Raised when: OCR processing fails
- User Message: "Unable to extract text from the image. Please ensure the image is clear and contains readable text."
- Troubleshooting Tips:
  - Ensure the image is clear and high-resolution
  - Try scanning the document at a higher DPI (300+ recommended)
  - Verify the text in the image is readable
  - Consider converting the image to PDF first

**EmptyDocumentError**
- Raised when: Document has no content
- User Message: "The document appears to be empty or contains no readable text."
- Troubleshooting Tips:
  - Verify the document contains text
  - If it's a scanned document, ensure OCR is enabled
  - Try opening the file to confirm it has content

### 2. Model Inference Errors

**ModelLoadError**
- Raised when: ML model fails to load
- Severity: Critical
- User Message: "The AI model is temporarily unavailable. Please try again in a few moments."
- Troubleshooting Tips:
  - Wait a few moments and try again
  - Check your internet connection
  - Contact support if the issue persists

**InferenceError**
- Raised when: Model inference fails
- User Message: "An error occurred during analysis. Please try again or try with a different document."

**LowConfidenceError**
- Raised when: Model confidence is too low
- Severity: Low
- User Message: "The analysis has low confidence. Manual review is recommended for accurate results."

### 3. Validation Errors

**InvalidInputError**
- Raised when: User input is invalid
- User Message: "Invalid {field}. Please check your input and try again."

**FileSizeError**
- Raised when: File size exceeds limit
- User Message: "File size exceeds the maximum limit of {max_mb}MB. Please upload a smaller file."
- Troubleshooting Tips:
  - Compress the file to reduce its size
  - Split large documents into smaller sections
  - Remove unnecessary images or attachments

### 4. External Service Errors

**GoogleSheetsError**
- Raised when: Google Sheets integration fails
- User Message: "Unable to connect to Google Sheets. Please check the URL and ensure the sheet is shared correctly."
- Troubleshooting Tips:
  - Verify the Google Sheets URL is correct
  - Ensure the sheet is shared with the service account
  - Check that the sheet contains data in the specified range
  - Verify your Google API credentials are configured

## Input Validation

The `InputValidator` class provides validation for common inputs:

### File Size Validation
```python
InputValidator.validate_file_size(file_size, max_size_mb=10)
```

### Text Input Validation
```python
InputValidator.validate_text_input(text, min_length=100)
```

### Framework Validation
```python
InputValidator.validate_frameworks(['GDPR', 'HIPAA'])
```

### Confidence Threshold Validation
```python
InputValidator.validate_confidence_threshold(0.75)
```

### Google Sheets URL Validation
```python
InputValidator.validate_google_sheets_url(url)
```

## Fallback Mechanisms

The `FallbackHandler` class provides safe fallback values when operations fail:

### Fallback Clause Analysis
```python
analysis = FallbackHandler.create_fallback_clause_analysis(
    clause_id="clause_1",
    clause_text="Sample text"
)
```

### Fallback Compliance Report
```python
report = FallbackHandler.create_fallback_compliance_report(
    document_id="doc_1",
    frameworks=["GDPR", "HIPAA"]
)
```

### Fallback Processed Document
```python
doc = FallbackHandler.create_fallback_processed_document(
    filename="test.pdf",
    error_msg="Processing failed"
)
```

## Error Recovery Strategies

### Retry with Exponential Backoff
```python
result = ErrorRecoveryStrategy.retry_with_backoff(
    func=my_function,
    max_retries=3,
    initial_delay=1.0,
    backoff_factor=2.0
)
```

### Fallback Chain
```python
result = ErrorRecoveryStrategy.fallback_chain(
    funcs=[primary_func, secondary_func, tertiary_func],
    default_value="fallback_value"
)
```

## User Feedback System

The `UserFeedback` class provides consistent UI feedback:

### Success Messages
```python
UserFeedback.show_success(
    message="Document processed successfully",
    details="Extracted 25 clauses in 2.3 seconds"
)
```

### Error Messages with Troubleshooting
```python
UserFeedback.show_error(
    message="Failed to process document",
    error=exception_object,
    troubleshooting_tips=[
        "Ensure the file is not corrupted",
        "Try a different file format",
        "Contact support if issue persists"
    ]
)
```

### Warning Messages
```python
UserFeedback.show_warning(
    message="Low confidence detected",
    action="Manual review recommended"
)
```

### Progress Tracking
```python
tracker = ProgressTracker(total_steps=3, description="Analyzing")
tracker.update(1, "Classifying clauses...")
tracker.update(2, "Checking compliance...")
tracker.update(3, "Generating recommendations...")
tracker.complete("Analysis complete!")
```

### Validation Feedback
```python
UserFeedback.show_validation_error(
    field="File Size",
    message="File exceeds 10MB limit"
)
```

### Low Confidence Warning
```python
UserFeedback.show_low_confidence_warning(
    clause_count=5,
    threshold=0.75
)
```

## Integration Examples

### Document Processor
```python
def process_document(self, file_path: str) -> ProcessedDocument:
    try:
        # Validate file size
        file_size = Path(file_path).stat().st_size
        InputValidator.validate_file_size(file_size)
        
        # Process document
        text = self._extract_text(file_path)
        
        # Validate extracted text
        if not text or not text.strip():
            raise EmptyDocumentError(
                "No text could be extracted",
                details={'file_name': file_path}
            )
        
        return processed_doc
        
    except EmptyDocumentError:
        raise
    except Exception as e:
        raise DocumentProcessingError(
            f"Failed to process document: {e}",
            details={'file_name': file_path}
        )
```

### NLP Analyzer
```python
def analyze_clauses(self, clauses: List[Clause]) -> List[ClauseAnalysis]:
    try:
        analyses = []
        errors = 0
        
        for clause in clauses:
            try:
                analysis = self.analyze_clause(clause)
                analyses.append(analysis)
            except InferenceError as e:
                logger.error(f"Analysis failed: {e}")
                analyses.append(
                    FallbackHandler.create_fallback_clause_analysis(
                        clause.clause_id,
                        clause.text
                    )
                )
                errors += 1
        
        # Warn if too many errors
        if errors > len(clauses) * 0.5:
            logger.warning(f"High error rate: {errors}/{len(clauses)}")
        
        return analyses
        
    except Exception as e:
        logger.error(f"Critical error: {e}")
        return [
            FallbackHandler.create_fallback_clause_analysis(c.clause_id, c.text)
            for c in clauses
        ]
```

### Compliance Checker
```python
def check_compliance(
    self,
    clauses: List[ClauseAnalysis],
    frameworks: List[str],
    document_id: str
) -> ComplianceReport:
    try:
        # Validate inputs
        InputValidator.validate_frameworks(frameworks)
        
        if not clauses:
            return self._create_empty_report(document_id, frameworks)
        
        # Perform compliance checking
        results = self._check_all_frameworks(clauses, frameworks)
        
        return self._generate_report(results)
        
    except InvalidInputError:
        raise
    except Exception as e:
        logger.error(f"Compliance check failed: {e}")
        return FallbackHandler.create_fallback_compliance_report(
            document_id,
            frameworks
        )
```

## Best Practices

### 1. Always Validate Inputs
```python
# Validate before processing
InputValidator.validate_file_size(file_size)
InputValidator.validate_text_input(text)
InputValidator.validate_frameworks(frameworks)
```

### 2. Use Specific Exceptions
```python
# Good
raise UnsupportedFormatError("Invalid format", details={'format': 'xyz'})

# Avoid
raise Exception("Invalid format")
```

### 3. Provide User-Friendly Messages
```python
# Good
raise FileSizeError(
    "File too large",
    size_mb=15,
    max_mb=10
)
# User sees: "File size exceeds the maximum limit of 10MB"

# Avoid
raise Exception("File size 15728640 bytes exceeds 10485760 bytes")
```

### 4. Include Troubleshooting Tips
```python
UserFeedback.show_error(
    message="OCR extraction failed",
    error=ocr_error,
    troubleshooting_tips=ErrorMessageFormatter.get_troubleshooting_tips(ocr_error)
)
```

### 5. Log Errors Appropriately
```python
try:
    result = process_document(file_path)
except DocumentProcessingError as e:
    logger.error(f"Processing failed: {e}", extra=e.to_dict())
    raise
except Exception as e:
    logger.exception(f"Unexpected error: {e}")
    raise
```

### 6. Use Fallbacks for Non-Critical Failures
```python
try:
    embedding = generate_embedding(text)
except Exception as e:
    logger.warning(f"Embedding generation failed: {e}")
    embedding = None  # Continue without embedding
```

### 7. Provide Progress Feedback
```python
tracker = ProgressTracker(total_steps=3, description="Processing")
tracker.update(1, "Extracting text...")
tracker.update(2, "Segmenting clauses...")
tracker.update(3, "Analyzing content...")
tracker.complete("Processing complete!")
```

## Testing

Run the error handling tests:
```bash
python App/test_error_handling.py
```

Expected output:
```
✓ Custom exceptions test passed
✓ Error message formatter test passed
✓ Troubleshooting tips test passed
✓ Input validators test passed
✓ Fallback handler test passed
✓ Error recovery retry test passed
✓ Error recovery fallback chain test passed
✓ Exception serialization test passed

✅ All error handling tests passed!
```

## Summary

The error handling system provides:

1. **Comprehensive Exception Hierarchy**: Specific exceptions for different error types
2. **User-Friendly Messages**: Clear, actionable error messages for users
3. **Troubleshooting Guidance**: Helpful tips for resolving common issues
4. **Input Validation**: Prevent errors before they occur
5. **Fallback Mechanisms**: Graceful degradation when operations fail
6. **Error Recovery**: Retry strategies and fallback chains
7. **Progress Feedback**: Keep users informed during long operations
8. **Consistent UI**: Standardized error display across the application

This system ensures that users receive helpful feedback when errors occur and that the application degrades gracefully rather than failing completely.
