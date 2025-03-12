# PII-Detection-Text-Analytics
How to leverage the PII Redaction Container to redact text...

## Typical Use Cases
- ​Mask **sensitive information** to avoid a leak, inappropriate handling or data bias
- **Auto label and classify documents** based on the sensitivity
- Intermediary step before sending to LLM

![redacted-sample](/images/redacted-sample.jpg)

## Quickly Setup and Text Example
In this scenario I will walk you through quickly setting up the PII Redaction Container and show you had to test and validate that it’s working.

### Step 1 - Create the container app on Azure
1. open the Azure Portal and create a **resource group**, for example purposes lets call it **rg-pii-test** 
