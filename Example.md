# PII Detection and Redaction - Examples and Usage Guide

This guide provides comprehensive examples and instructions for using the PII Detection and Redaction service powered by Azure AI Language Services. This solution can be deployed as a container for enhanced data privacy and security, including disconnected scenarios.

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Example 1: Basic PII Redaction](#example-1-basic-pii-redaction)
- [Example 2: Selective PII Category Redaction](#example-2-selective-pii-category-redaction)
- [Example 3: Using cURL](#example-3-using-curl)
- [Example 4: Azure Container Deployment](#example-4-azure-container-deployment)
- [Understanding the Response](#understanding-the-response)
- [Advanced Configuration](#advanced-configuration)
- [Troubleshooting](#troubleshooting)
- [Additional Resources](#additional-resources)

## Overview

The PII Detection and Redaction service helps you:
- **Mask sensitive information** to avoid data leaks, inappropriate handling, or data bias
- **Auto-label and classify documents** based on sensitivity levels
- **Preprocess data** before sending to Large Language Models (LLMs)
- **Comply with data protection regulations** by identifying and redacting PII

### What is PII?

Personally Identifiable Information (PII) includes data that can identify an individual, such as:
- Names, addresses, phone numbers
- Email addresses
- Social Security Numbers
- Credit card numbers
- IP addresses
- Medical record numbers
- And many more...

## Prerequisites

Before you begin, ensure you have:

1. **Azure Subscription**: An active Azure subscription
2. **Azure AI Language Service**: A deployed Language Service resource
   - Endpoint URL
   - API Key (Subscription Key)
3. **Development Tools** (for testing):
   - cURL or an HTTP client (Postman, Insomnia, Bruno, etc.)
   - Azure CLI (for container deployment)
   - Visual Studio Code (optional but recommended)

## Quick Start

### Step 1: Create Azure AI Language Service

1. Log in to the [Azure Portal](https://portal.azure.com)
2. Create a new **Language Service** resource
3. Note down your:
   - **Endpoint**: `https://<your-resource>.cognitiveservices.azure.com/`
   - **API Key**: Found in "Keys and Endpoint" section

### Step 2: Test the Service

Send a POST request to the Language Service endpoint:

```bash
curl -X POST "https://<YOUR_ENDPOINT>/language/:analyze-text?api-version=2024-11-01" \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <YOUR_API_KEY>" \
-d '{
  "kind": "PiiEntityRecognition",
  "parameters": {
    "modelVersion": "latest"
  },
  "analysisInput": {
    "documents": [
      {
        "id": "1",
        "language": "en",
        "text": "Call our office at 312-555-1234"
      }
    ]
  }
}'
```

## Example 1: Basic PII Redaction

This example demonstrates detecting and redacting all types of PII from a simple text.

### Input Request

Create a file named `request1.json`:

```json
{
    "kind": "PiiEntityRecognition",
    "parameters": {
        "modelVersion": "latest"
    },
    "analysisInput": {
        "documents": [
            {
                "id": "1",
                "language": "en",
                "text": "Call our office at 312-555-1234, or send an email to support@contoso.com"
            }
        ]
    }
}
```

### Key Fields Explained

- **kind**: Type of analysis (always `"PiiEntityRecognition"` for PII detection)
- **modelVersion**: Version of the model to use (`"latest"` recommended)
- **id**: Unique identifier for each document
- **language**: ISO 639-1 language code (e.g., "en" for English)
- **text**: The content to analyze

### Send the Request

```bash
curl -i -X POST "https://<YOUR_ENDPOINT>/language/:analyze-text?api-version=2024-11-01" \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <YOUR_API_KEY>" \
-d @request1.json
```

### Expected Response

```json
{
  "kind": "PiiEntityRecognitionResults",
  "results": {
    "documents": [
      {
        "redactedText": "Call our office at ***********, or send an email to *********",
        "id": "1",
        "entities": [
          {
            "text": "312-555-1234",
            "category": "PhoneNumber",
            "offset": 19,
            "length": 12,
            "confidenceScore": 0.8
          },
          {
            "text": "support@contoso.com",
            "category": "Email",
            "offset": 53,
            "length": 19,
            "confidenceScore": 0.8
          }
        ]
      }
    ]
  }
}
```

### What Happened?

The service identified:
1. **Phone Number**: `312-555-1234` - Redacted with asterisks
2. **Email Address**: `support@contoso.com` - Redacted with asterisks

## Example 2: Selective PII Category Redaction

Sometimes you only want to redact specific types of PII. This example shows how to target specific categories.

### Input Request

Create a file named `request2.json`:

```json
{
    "kind": "PiiEntityRecognition",
    "parameters": {
        "modelVersion": "latest",
        "piiCategories": [
            "Person"
        ]
    },
    "analysisInput": {
        "documents": [
            {
                "id": "1",
                "language": "en",
                "text": "We went to Contoso foodplace located at downtown Seattle last week for a dinner party, and we adore the spot! They provide marvelous food and they have a great menu. The chief cook happens to be the owner (I think his name is John Doe) and he is super nice, coming out of the kitchen and greeted us all. We enjoyed very much dining in the place! The pasta I ordered was tender and juicy, and the place was impeccably clean. You can even pre-order from their online menu at www.contosofoodplace.com, call 112-555-0176 or send email to order@contosofoodplace.com! The only complaint I have is the food didn't come fast enough. Overall I highly recommend it!"
            }
        ]
    }
}
```

### Available PII Categories

You can filter by these categories:
- `Person` - Person names
- `Organization` - Company/organization names
- `Address` - Physical addresses
- `Email` - Email addresses
- `PhoneNumber` - Phone numbers
- `SSN` - Social Security Numbers
- `CreditCard` - Credit card numbers
- `IPAddress` - IP addresses
- `DateTime` - Date and time information
- `URL` - Web URLs
- And many more...

### Send the Request

```bash
curl -i -X POST "https://<YOUR_ENDPOINT>/language/:analyze-text?api-version=2024-11-01" \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <YOUR_API_KEY>" \
-d @request2.json
```

### Result

In this example, only **person names** (like "John Doe") will be redacted. Other PII like phone numbers, emails, and URLs will remain visible in the redacted text.

## Example 3: Using cURL

The `curl-examples` folder contains ready-to-use examples. Follow these steps:

### Step 1: Navigate to the Examples Folder

```bash
cd curl-examples
```

### Step 2: Review the Example Files

- `request1.json` - Basic PII redaction example
- `request2.json` - Selective category redaction example
- `README.md` - Detailed instructions

### Step 3: Run the Examples

For Windows PowerShell:
```powershell
curl.exe -i -X POST https://<YOUR_ENDPOINT>/language/:analyze-text?api-version=2024-11-01 `
-H "Content-Type: application/json" `
-H "Ocp-Apim-Subscription-Key: <YOUR_API_KEY>" `
-d `@request1.json
```

For Linux/Mac:
```bash
curl -i -X POST "https://<YOUR_ENDPOINT>/language/:analyze-text?api-version=2024-11-01" \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <YOUR_API_KEY>" \
-d @request1.json
```

### Expected Output

You should receive an HTTP 200 response with JSON containing:
- Redacted text with PII masked
- List of detected entities with their categories
- Confidence scores for each detection

![Example Response](images/curl-example.jpg)

## Example 4: Azure Container Deployment

For enhanced security and privacy, deploy the PII detection service as a container. This is ideal for:
- **Disconnected environments** with no internet access
- **Data residency requirements** keeping data within your network
- **High-security scenarios** with strict PII handling policies

### Step 1: Create Resource Group

```bash
az group create --name rg-pii-test --location eastus
```

### Step 2: Create Azure AI Language Service

```bash
az cognitiveservices account create \
  --name pii-language-service \
  --resource-group rg-pii-test \
  --kind TextAnalytics \
  --sku S \
  --location eastus
```

Capture your endpoint and API key:
```bash
az cognitiveservices account show \
  --name pii-language-service \
  --resource-group rg-pii-test \
  --query "properties.endpoint" -o tsv

az cognitiveservices account keys list \
  --name pii-language-service \
  --resource-group rg-pii-test \
  --query "key1" -o tsv
```

### Step 3: Deploy Container App

```bash
az containerapp create \
  --name pii-detection-app \
  --resource-group rg-pii-test \
  --image mcr.microsoft.com/azure-cognitive-services/textanalytics/pii:latest \
  --environment managedEnvironment-rgpiiai-8302 \
  --cpu 4 \
  --memory 8Gi \
  --min-replicas 1 \
  --max-replicas 3 \
  --env-vars "Eula=accept" "Billing=https://<your-endpoint>" "ApiKey=<your-api-key>" \
  --ingress external \
  --target-port 5000
```

**Important Notes:**
- If a container environment doesn't exist, one will be created automatically
- Minimum CPU: 4 cores, Memory: 8 GB for optimal performance
- The `Eula=accept` parameter accepts the license terms
- Replace `<your-endpoint>` and `<your-api-key>` with your values

### Step 4: Verify Deployment

Check the health status:
```bash
curl https://<your-app>.eastus.azurecontainerapps.io/status
```

Expected response:
```json
{
  "service": "pii",
  "apiStatus": "Valid",
  "apiStatusMessage": "Api Key is valid, no action needed."
}
```

### Step 5: Test the Container

```bash
curl -X POST "https://<your-app>.eastus.azurecontainerapps.io/language/:analyze-text?api-version=2024-11-01" \
-H "Content-Type: application/json" \
-d '{
  "kind": "PiiEntityRecognition",
  "parameters": {
    "modelVersion": "latest"
  },
  "analysisInput": {
    "documents": [
      {
        "id": "1",
        "language": "en",
        "text": "My SSN is 123-45-6789"
      }
    ]
  }
}'
```

## Understanding the Response

### Response Structure

```json
{
  "kind": "PiiEntityRecognitionResults",
  "results": {
    "documents": [
      {
        "redactedText": "...",      // Text with PII replaced by asterisks
        "id": "1",                   // Document identifier
        "entities": [               // Array of detected PII entities
          {
            "text": "...",          // Original PII text
            "category": "...",      // Type of PII
            "offset": 0,            // Character position where PII starts
            "length": 0,            // Length of PII text
            "confidenceScore": 0.0  // Detection confidence (0.0 to 1.0)
          }
        ],
        "warnings": []              // Any warnings about the analysis
      }
    ],
    "errors": [],                   // Any errors that occurred
    "modelVersion": "2025-02-01"    // Model version used
  }
}
```

### Key Fields

- **redactedText**: The text with all detected PII replaced by asterisks (`*`)
- **entities**: Array of all PII detected, with details about each
- **category**: The type of PII (Person, Email, PhoneNumber, etc.)
- **confidenceScore**: How confident the model is (0.8 or higher is very reliable)
- **offset** and **length**: Position of the PII in the original text

### Confidence Scores

- **0.8 - 1.0**: High confidence - Very likely to be PII
- **0.5 - 0.8**: Medium confidence - Probably PII, review recommended
- **0.0 - 0.5**: Low confidence - May be false positive

## Advanced Configuration

### Multiple Documents in One Request

Process multiple documents efficiently:

```json
{
  "kind": "PiiEntityRecognition",
  "parameters": {
    "modelVersion": "latest"
  },
  "analysisInput": {
    "documents": [
      {
        "id": "1",
        "language": "en",
        "text": "First document text..."
      },
      {
        "id": "2",
        "language": "en",
        "text": "Second document text..."
      }
    ]
  }
}
```

### Domain-Specific Configuration

For healthcare data:
```json
{
  "kind": "PiiEntityRecognition",
  "parameters": {
    "modelVersion": "latest",
    "domain": "phi",  // Protected Health Information
    "piiCategories": [
      "Person",
      "SSN",
      "DateTime"
    ]
  },
  "analysisInput": {
    "documents": [...]
  }
}
```

### Custom Redaction Character

While not directly configurable in the API, you can post-process the redacted text to use different masking characters.

### Language Support

The service supports multiple languages. Specify the language code:

```json
{
  "id": "1",
  "language": "es",  // Spanish
  "text": "Mi número es 555-1234"
}
```

Supported languages include: English (en), Spanish (es), French (fr), German (de), Italian (it), Portuguese (pt), Chinese (zh-Hans), Japanese (ja), Korean (ko), and more.

## Troubleshooting

### Common Issues and Solutions

#### 1. 401 Unauthorized Error

**Problem**: Invalid or missing API key

**Solution**:
- Verify your API key is correct
- Ensure the key hasn't expired
- Check the `Ocp-Apim-Subscription-Key` header is set correctly

#### 2. 400 Bad Request Error

**Problem**: Invalid request format

**Solution**:
- Validate your JSON structure
- Ensure `kind` is set to `"PiiEntityRecognition"`
- Check that document `id` is a string
- Verify `language` is a valid ISO 639-1 code

#### 3. Container App Status Returns "Invalid"

**Problem**: Billing endpoint or API key not configured correctly

**Solution**:
```bash
# Check environment variables
az containerapp show --name pii-detection-app --resource-group rg-pii-test

# Update environment variables if needed
az containerapp update \
  --name pii-detection-app \
  --resource-group rg-pii-test \
  --set-env-vars "Billing=https://<correct-endpoint>" "ApiKey=<correct-key>"
```

#### 4. Slow Response Times

**Problem**: Container resource constraints

**Solution**:
- Increase CPU and memory allocation
- Enable auto-scaling with appropriate replica counts
- Consider using Azure AI Language Service directly (non-containerized)

#### 5. PII Not Detected

**Problem**: Low confidence or unsupported PII type

**Solution**:
- Check the confidence scores in the response
- Review the supported PII categories for your language
- Consider using domain-specific parameters (e.g., `"domain": "phi"`)

### Debugging Tips

1. **Test with Known PII**: Start with simple, obvious PII like "John Doe" or "123-45-6789"
2. **Check API Version**: Ensure you're using the latest API version
3. **Review Logs**: Check Azure Monitor logs for detailed error messages
4. **Use Verbose Mode**: Add `-v` flag to cURL for detailed request/response information

## Additional Resources

### Documentation
- [Azure AI Language Service - PII Detection](https://learn.microsoft.com/en-us/azure/ai-services/language-service/personally-identifiable-information/how-to/redact-text-pii)
- [Azure Container Apps Documentation](https://learn.microsoft.com/en-us/azure/container-apps/)
- [Cognitive Services Containers](https://learn.microsoft.com/en-us/azure/cognitive-services/cognitive-services-container-support)

### Repository Contents
- **[README.md](README.md)**: Main repository documentation
- **[curl-examples/](curl-examples/)**: Ready-to-use cURL examples with detailed instructions
- **[images/](images/)**: Screenshots and visual examples

### API Reference
- [Text Analytics API Reference](https://learn.microsoft.com/en-us/rest/api/language/)
- [PII Categories Reference](https://learn.microsoft.com/en-us/azure/ai-services/language-service/personally-identifiable-information/concepts/entity-categories)

### Support
- For issues or questions about this repository, please open an issue on GitHub
- For Azure AI services support, contact [Azure Support](https://azure.microsoft.com/support/)

### Community
- Join the [Azure AI Community](https://techcommunity.microsoft.com/t5/azure-ai/ct-p/AzureAI)
- Contribute to this repository by submitting pull requests

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Summary

This guide demonstrated:
✅ Basic and advanced PII detection and redaction
✅ Using specific PII categories for selective redaction
✅ Working with cURL and HTTP clients
✅ Deploying as an Azure Container App for enhanced security
✅ Understanding and troubleshooting responses

For more examples, explore the [curl-examples](curl-examples/) directory in this repository.
