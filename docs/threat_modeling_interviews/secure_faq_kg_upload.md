# Q&A: Securing FAQ/Knowledge Graph Upload Functionality

## Question
How are we securing upload FAQ/knowledge graph functionality for partner to Sprinklr?

## Tech Lead's Answer
Partners upload FAQ and knowledge graph data using pre-signed, time-limited S3 URLs. Each upload is scoped to a specific partner and path, and access is authenticated and authorized via the API Gateway. All uploads are validated for file type, size, and content before being accepted.

## Recommendation
Continue to use pre-signed URLs with strict expiration and scope. Validate all uploads on the backend and monitor for unusual activity.

## Risk
If upload URLs are leaked or validation is bypassed, unauthorized or malicious data could be uploaded.

## Mitigation
- Limit URL validity and scope.
- Enforce backend validation of all uploads.
- Monitor and alert on unusual upload patterns.

## Incident Response Plan
- Revoke affected URLs.
- Remove unauthorized or malicious data.
- Notify affected partners and review upload logs.
