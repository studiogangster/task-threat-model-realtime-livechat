# Deployment

This section describes the deployment architecture and infrastructure for the Sprinklr Live Chat platform.

## Cloud Environment

- **AWS Cloud:**  
  The platform is deployed on Amazon Web Services (AWS), leveraging managed services for compute, storage, networking, and security.

- **Multi-Region:**  
  Services are deployed across multiple AWS regions to ensure high availability, disaster recovery, and low-latency access for global users.

## Core Infrastructure Components

- **Elastic Load Balancer (ELB):**  
  Distributes incoming traffic across multiple application servers for scalability and fault tolerance.

- **API Gateway:**  
  Serves as the single entry point for all public, partner, and admin API requests, enforcing authentication and rate limiting.

- **ECS/EKS:**  
  Container orchestration for scalable, isolated application services.

- **RDS:**  
  Managed relational database for storing persistent data.

- **S3:**  
  Object storage for static assets, knowledge uploads, and logs.

- **CloudFront CDN:**  
  Delivers static content and SDK assets globally with low latency.

- **Secrets Manager/Vault:**  
  Centralized management of secrets, certificates, and configuration.

## Security and Operations

- **mTLS:**  
  Mutual TLS is enforced for all partner-to-cloud and internal service-to-service communication.

- **Auto-Scaling:**  
  Services automatically scale based on demand to maintain performance and cost efficiency.

- **Monitoring & Logging:**  
  Cloud-native tools (CloudWatch, GuardDuty, VPC Flow Logs) are used for monitoring, alerting, and incident response.

- **Disaster Recovery:**  
  Regular backups and cross-region replication ensure business continuity.
