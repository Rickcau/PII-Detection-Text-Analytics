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
1. open the Azure Portal and create a **resource group**, for example purposes lets call it **rg-pii-test**.
2. Create an Azure AI Language service and make sure to capture the **Endpoint** and your API Key, you will need it in the next step.
3. Run the following command from the Terminal Window in VS Code or via the Azure CLI in the portal.

```
      az containerapp create --name pii-detection-app --resource-group rg-pii-test --image mcr.microsoft.com/azure-cognitive-services/textanalytics/pii:latest --environment managedEnvironment-rgpiiai-8302 --cpu 4 --memory 8Gi --min-replicas 1 --max-replicas 3 --env-vars "Eula=accept" "Billing=https://<your-ai-language-endpoint>" "ApiKey=<your API key>" --ingress external --target-port 5000
```
