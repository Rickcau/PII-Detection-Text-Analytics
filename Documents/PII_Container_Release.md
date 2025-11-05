# PII Redaction Container Release

## Overview

The **Azure AI Text PII Redaction Container** enables organizations to detect and redact Personally Identifiable Information (PII) from text locally in their own infrastructure. This containerized solution brings Azure's powerful PII detection capabilities to on-premises and edge environments, providing the same sophisticated AI models available in the cloud.

## Why Use the PII Redaction Container?

### 1. **Stringent Security and Privacy Requirements**

Organizations with strict security policies or regulatory requirements can process sensitive data within their own controlled environments without sending it to external cloud services. This is particularly critical for:

- **Healthcare organizations** handling Protected Health Information (PHI) under HIPAA
- **Financial institutions** processing customer financial data under PCI-DSS
- **Government agencies** managing classified or sensitive citizen data
- **Legal firms** handling confidential client information

### 2. **Data Sovereignty and Compliance**

Keep data within specific geographic boundaries to comply with:
- **GDPR** (General Data Protection Regulation) requirements
- **Regional data residency laws**
- **Industry-specific compliance mandates**
- **Corporate data governance policies**

### 3. **Reduced Latency and Improved Performance**

Run PII detection physically close to your data sources and applications for:
- **Faster processing** of large volumes of text
- **Lower network latency** by eliminating round-trips to cloud services
- **High-throughput scenarios** where milliseconds matter
- **Real-time processing** requirements

### 4. **Cost Optimization**

For organizations with predictable, high-volume workloads:
- **Commitment tier pricing** offers discounted rates for long-term usage
- **Eliminate data egress costs** associated with sending data to the cloud
- **Predictable costs** based on commitment plans rather than per-transaction billing

### 5. **Hybrid and Edge Deployment**

Deploy containers in environments where cloud connectivity is:
- **Limited or unavailable** (edge computing scenarios)
- **Unreliable** (remote locations)
- **Restricted** (air-gapped networks)
- **Controlled** (specific network security zones)

## When to Use the PII Redaction Container

### Ideal Use Cases

#### 1. **Anonymizing Customer Support Data**
Redact PII from customer service call transcripts, chat logs, and support tickets before:
- Storing in data warehouses for analytics
- Training AI models
- Sharing with third-party vendors
- Archiving for compliance

#### 2. **Legal Document Processing**
Process legal documents, contracts, and court filings that contain:
- Names and personal identifiers
- Social Security Numbers
- Financial account information
- Medical information
- Before sharing with external parties or storing in document management systems

#### 3. **Healthcare Data Management**
Anonymize medical records, clinical notes, and patient communications:
- Electronic Health Records (EHR) for research purposes
- Medical transcriptions
- Insurance claims
- Patient correspondence

#### 4. **AI and LLM Integration**
Protect user privacy when:
- Sending prompts to Large Language Models (LLMs)
- Building training datasets for AI models
- Creating chatbots and conversational AI applications
- Processing user-generated content

#### 5. **Financial Services**
Redact sensitive information from:
- Transaction records
- Customer communications
- Loan applications
- Credit reports
- Audit logs

#### 6. **Government and Public Sector**
Secure citizen data in:
- Freedom of Information Act (FOIA) requests
- Public records
- Benefits applications
- Census data

## Key Features

### Comprehensive PII Detection

The container can identify and redact 50+ entity types including:
- **Personal identifiers**: Names, email addresses, phone numbers
- **Government IDs**: Social Security Numbers, passport numbers, driver's licenses
- **Financial data**: Credit card numbers, bank account numbers, international bank account numbers
- **Healthcare data**: Medical record numbers, health insurance information
- **Location data**: Addresses, IP addresses
- **Professional data**: Organization names, job titles
- **Region-specific entities**: Sort codes (UK), license plate numbers, and more

### Multi-Language Support

- Supports **70+ languages**
- Ongoing development for Chinese, Japanese, Korean, and Thai

### Flexible Deployment Options

#### Connected Containers
- Maintain connection to Azure for metering
- **Pay-as-you-go pricing** for variable workloads
- **Commitment tier pricing** for predictable usage patterns
- Easier billing and usage tracking

#### Disconnected Containers
- Operate completely offline after initial configuration
- **Commitment tier pricing only**
- Generate local usage reports
- Ideal for air-gapped environments
- Require application and approval process

### Customization Options

- **Custom regex patterns**: Define domain-specific PII patterns
- **Entity synonyms**: Map custom terminology to standard entity types
- **Value exclusion policies**: Specify values that should not be redacted (e.g., "police officer," "witness")
- **Selective redaction**: Choose which entity categories to detect and redact
- **Redaction policies**: 
  - Mask with characters (e.g., `****`)
  - Mask with entity type (e.g., `[PERSON_1]`)
  - No redaction (detection only)

## Getting Started

### Prerequisites

- Docker Engine installed on your host system
- Azure subscription
- Azure AI Language resource
- Application approval (for disconnected containers)
- Commitment plan purchase (for cost-optimized or disconnected deployments)

### Basic Workflow

1. **Request Access** (for disconnected containers)
   - Fill out the [application request form](https://customervoice.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR7en2Ais5pxKtso_Pz4b1_xUNDVVMVBPV09ITVBBR0E5T05QQ1VESFlSMCQlQCN0PWcu)
   - Wait for approval from Azure AI services team

2. **Create Azure Resources**
   - Provision an Azure AI Language resource
   - Select appropriate pricing tier (connected or disconnected commitment tier)

3. **Download Container**
   ```bash
   docker pull mcr.microsoft.com/azure-cognitive-services/textanalytics/pii:{IMAGE_TAG}
   ```

4. **Configure and Run**
   ```bash
   docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 \
     mcr.microsoft.com/azure-cognitive-services/textanalytics/pii:{IMAGE_TAG} \
     Eula=accept \
     Billing={ENDPOINT_URI} \
     ApiKey={API_KEY}
   ```

5. **Send Requests**
   - Use REST API to send text for PII detection
   - Receive redacted text and entity information
   - No customer data is sent to Microsoft (only billing metrics)

## Related Azure AI Language Services

The PII Redaction Container is part of a broader ecosystem of PII detection offerings:

### Conversational PII (Azure-hosted)
Optimized for speech-to-text transcripts and conversational text:
- Handles "um"s, "ah"s, and filler words
- Manages multiple speakers
- Processes chat logs and informal text
- Better context understanding for spoken language

### Native Document PII (Azure-hosted)
Process entire documents without pre-processing:
- Supports **PDF, DOCX, and TXT** files
- Direct input/output from Azure Blob Storage
- No need to extract text manually
- Maintains document structure

### Text PII (Azure-hosted API)
Cloud-based API for standard text processing:
- No infrastructure management
- Automatic scaling
- Latest model updates
- Integration with other Azure services

## System Requirements

### Minimum Requirements
- **CPU**: 1 core (2.6 GHz or faster)
- **Memory**: 8 GB RAM
- **Storage**: Sufficient space for container image and working data
- **CPU Instruction Set**: AVX-512 recommended for best performance and accuracy

### Supported Platforms
- Linux x64-based systems
- Azure Kubernetes Service (AKS)
- Azure Container Instances
- Kubernetes clusters (including Azure Stack)
- On-premises servers
- Edge devices

### API Limits
- **Single synchronous call**: 5,120 characters per document
- **Maximum documents per call**: 10 documents

## Pricing Models

### Connected Containers
1. **Pay-as-you-go**: Flexible pricing based on actual usage
2. **Commitment tier**: Discounted rates with annual commitment

### Disconnected Containers
- **Commitment tier only**: Fixed annual pricing
- Requires upfront commitment plan purchase
- Ideal for predictable, high-volume workloads
- Cost savings for long-term customers

## Documentation and Resources

- [PII Container Documentation](https://learn.microsoft.com/azure/ai-services/language-service/personally-identifiable-information/how-to/use-containers)
- [Disconnected Container Guide](https://learn.microsoft.com/en-us/azure/ai-services/containers/disconnected-containers)
- [Text PII Service Overview](https://learn.microsoft.com/en-us/azure/ai-services/language-service/personally-identifiable-information/overview)
- [Adapting PII to Your Domain](https://learn.microsoft.com/en-us/azure/ai-services/language-service/personally-identifiable-information/how-to/adapt-to-domain-pii)
- [Azure AI Language Overview](https://learn.microsoft.com/en-us/azure/ai-services/language-service/overview)
- [Pricing Information](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/language-service/)

## Important Considerations

### Security and Limitations

⚠️ **Important**: While the PII Redaction Container uses advanced detection mechanisms, it cannot guarantee 100% detection of all sensitive information. Organizations should:
- Implement additional security measures and protections
- Conduct manual reviews for highly sensitive data
- Use the container as part of a defense-in-depth strategy
- Test thoroughly with representative data before production deployment

### Billing and Metering

- **Connected containers**: Send usage metrics to Azure every 10-15 minutes
- **Disconnected containers**: Generate local usage reports that can be retrieved via REST endpoints
- Containers will not serve queries if billing endpoint is unavailable (connected mode)
- Customer data (text being analyzed) is **never sent to Microsoft**

## Support and Community

- **Azure Support Plans**: For production deployments
- **GitHub Discussions**: Community support and questions
- **Documentation**: Comprehensive how-to guides and API references
- **Stack Overflow**: Technical Q&A with tags like `azure-cognitive-services`

## Conclusion

The PII Redaction Container Release provides organizations with a powerful, flexible solution for protecting sensitive data while maintaining control over their infrastructure. Whether you need to meet compliance requirements, optimize costs, reduce latency, or operate in disconnected environments, this containerized approach brings enterprise-grade PII detection capabilities to your on-premises or edge deployments.

For organizations balancing data privacy, regulatory compliance, and operational efficiency, the PII Redaction Container represents a critical tool in building secure, privacy-first applications and data pipelines.

---

**Release Date**: October 2024  
**Platform**: Azure AI Language  
**Container Type**: Docker (Linux)  
**Availability**: Generally Available (GA)
