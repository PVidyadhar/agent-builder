# Bedrock Managed Knowledge Base Support

## Changes
- Updated agent KB retrieval module to default to `managedSearchConfiguration`
- Added `knowledge_base_type` config option in agent builder KB integration
- Retriever factory selects search configuration shape based on KB type
- Added `AgenticRetrieveStream` support for enhanced agentic retrieval
- Existing VECTOR agent retrieval paths remain unchanged

## Design
- VECTOR is the default; MANAGED via explicit --knowledge-base-type MANAGED or env var
- AgenticRetrieveStream for agentic retrieval with managed foundation/reranking models
- Backward compatible: existing VECTOR paths and agent interfaces unchanged
- Agent builder UI/config surfaces KB type selection when creating retrieval tools

## API Shapes
- KB Creation: `type: MANAGED` + `managedKnowledgeBaseConfiguration.embeddingModelType: MANAGED`
- Retrieval: `managedSearchConfiguration` (not `vectorSearchConfiguration`)
- Agentic: `AgenticRetrieveStream` with `foundationModelType: MANAGED`, `rerankingModelType: MANAGED`

## Configuration
| Variable | Description | Default |
|---|---|---|
| KNOWLEDGE_BASE_TYPE | MANAGED or VECTOR | VECTOR |
| USE_AGENTIC_RETRIEVAL | Enable agentic retrieval | true |
| KNOWLEDGE_BASE_ID | KB identifier | (required) |

## SDK Requirements
- boto3 >= 1.43 for managed search and agentic retrieval
- JS SDK >= 3.750.0 for managed KB support (if using JS client)

## Required IAM Permissions
```json
{
  "Effect": "Allow",
  "Action": [
    "bedrock:Retrieve",
    "bedrock:AgenticRetrieve"
  ],
  "Resource": "arn:aws:bedrock:<region>:<account-id>:knowledge-base/<kb-id>"
}
```
