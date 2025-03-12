# PII-Detection-Text-Analytics
How to leverage the PII Redaction Container to redact text.  The Redaction container can be run in a disconnecte3d mode if needed for customers that have strict policies around dealing with PII, this solution can help solve those challenges.

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
**Important Notes:**
- If a container environment does not exist, a new one will be created
- Replace the endpoint and API key with your endpoint and API Key

### Step 2 - Testing the solution and verifying it's up and running

1. Navigate to the deployed container app:  `https://<your app>.eastus.azurecontainerapps.io/status`

If everything is properly setup you will see the following JSON response:

```
   {"service":"pii","apiStatus":"Valid","apiStatusMessage":"Api Key is valid, no action needed."}
```
