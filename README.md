# My Playbook 
# ☁️ AWS Enterprise Solutions Architecture Portfolio

![AWS](https://img.shields.io/badge/AWS-Cloud%20Architecture-FF9900?logo=amazon-aws)
![Terraform](https://img.shields.io/badge/IaC-Terraform-623CE4?logo=terraform)
![Kubernetes](https://img.shields.io/badge/Container-EKS-326CE5?logo=kubernetes)
![Security](https://img.shields.io/badge/Security-Cloud%20Native-green)
![Architecture](https://img.shields.io/badge/Role-Solutions%20Architect-blue)

---

## Overview

This repository documents enterprise-grade AWS cloud architecture patterns designed for:

- Financial Services
- Regulated Industries (I will focus in on Healthcare- MMIS)
- High-Availability Workloads
- Multi-Environment Deployment Models

The focus is on:

- Scalable AWS infrastructure
- Production-grade EKS architecture
- Infrastructure as Code governance
- Disaster Recovery strategy
- Cloud security & compliance
- Cost optimization engineering

## **🚀 Healthcare Context- MMIS specifically Comment**

State Medicaid agencies have officially retired the era of the “one giant, indestructible Medicaid system” — that proud monolith that did everything… and occasionally reminded everyone of it during a three-hour outage. In its place, we now have a sleek, modular architecture. Think less battleship, more fleet of specialized vessels.
In theory, this is progress — and it is. In practice, critical information now lives across a constellation of relational and NoSQL databases, each loyally serving its own module. The result? Data everywhere. Insight… not always.
   
Without a unified strategy, there’s no single source of truth — just multiple well-intentioned versions of it. Stakeholders trying to understand Medicaid operations often find themselves reconciling numbers that are technically correct, but not collectively aligned. Fragmentation introduces inefficiencies, duplicated effort, and the occasional analytical déjà vu.
Meanwhile, agencies are navigating budget constraints and workforce shortages, all while being asked to do more with less — and do it faster. In short, Medicaid modernization has improved the architecture, but it has also raised the bar on governance, integration, and data strategy.
Because modular systems are powerful — but only if someone is conducting the orchestra.
The traditional Medicaid Management Information System (MMIS) is a comprehensive system responsible for claims adjudication, financial transaction processing, decision support, pharmacy benefits management, contact centers, and the storage of provider and recipient data, alongside the implementation of business rules and reference data. As these functions are deployed into individual modules, data is distributed across various database types such as Amazon Aurora, Microsoft SQL, DB2 on mainframe, PostgreSQL, and Amazon Simple Storage Service (Amazon S3). Medicaid agencies must plan to ingest data from these diverse sources to create a unified ODS. The ODS should catalog data, perform quality checks, manage master data, and use a business glossary to produce actionable insights. This post explains the architectural choices available with Amazon Web Services (AWS) to effectively create and maintain such an ODS.


---
## **🚀 Rapid-Fire Essentials (Top 15)**

1. **AWS Durability (S3)** – 11 nines (99.999999999%).
2. **EKS Control Plane** – AWS-managed, HA, multi-AZ.
3. **Service Types** – ClusterIP, NodePort, LoadBalancer, ExternalName.
4. **HPA Metrics** – CPU, memory, custom metrics.
5. **Rolling Update** – maxUnavailable, maxSurge.
6. **Terraform Backend** – S3 + DynamoDB for lock.
7. **GitOps Tools** – Argo CD, Flux.
8. **CI vs CD** – CI = build/test, CD = deploy.
9. **CrashLoopBackOff** – Logs, probes, limits, config.
10. **Secrets Management** – AWS Secrets Manager / Sealed-Secrets.
11. **StatefulSet Use** – Persistent, ordered workloads.
12. **Image Scanning Tools** – Trivy, AquaSec.
13. **Multi-AZ vs Multi-Region** – HA vs DR.
14. **WAF Function** – Block malicious requests at L7.
15. **Drift Detection** – Argo CD sync status, terraform plan.

---
Here are some tools and that will be needed when provisioing the right settings for MMIS MARe set up formation.
---

### **AWS**

1. **Default VPC CIDR?** – 172.31.0.0/16
2. **EC2 stop vs terminate?** – Stop keeps EBS, terminate deletes.
3. **S3 durability?** – 99.999999999% (11 nines).
4. **IAM role vs user?** – Role = temp credentials, User = permanent.
5. **EKS control plane hosted where?** – AWS-managed.
6. **ALB vs NLB?** – ALB = L7, NLB = L4.
7. **Spot instance use case?** – Batch, non-critical workloads.
8. **CloudFormation vs Terraform?** – CF = AWS native, TF = multi-cloud.
9. **AWS Secrets Manager vs SSM Parameter Store?** – Secrets rotation vs static config.
10. **RDS Multi-AZ purpose?** – High availability failover.

---

### **Kubernetes / EKS**

11. **kubectl get pods -A?** – Lists all namespace pods.
12. **Pod vs Deployment?** – Pod = 1+ containers, Deployment = manages pods.
13. **Service types?** – ClusterIP, NodePort, LoadBalancer, ExternalName.
14. **HPA metric?** – CPU, memory, custom metrics.
15. **Readiness probe purpose?** – Traffic only to ready pods.
16. **ConfigMap vs Secret?** – Plain text vs base64 encoded & secure.
17. **Rolling update strategy?** – maxUnavailable / maxSurge.
18. **StatefulSet use case?** – Persistent, ordered pods.
19. **Ingress controller role?** – Route external traffic into cluster.
20. **CrashLoopBackOff fix?** – Logs, events, resource limits, probe check.

---

### **Terraform / IaC**

21. **terraform init?** – Initialize backend & plugins.
22. **terraform plan?** – Show changes before apply.
23. **terraform state purpose?** – Track infra resources.
24. **Variables.tf usage?** – Input parameterization.
25. **Drift detection?** – terraform plan vs state.
26. **Terraform backend?** – Store state remotely (S3/DynamoDB).
27. **TF module use case?** – Reusable infra blocks.
28. **taint command?** – Force recreate resource.
29. **tfsec purpose?** – IaC security scanning.
30. **Terraform vs Ansible?** – Provision infra vs configure infra.

---

### **GitOps / CI-CD**

31. **GitOps key tools?** – Argo CD, Flux.
32. **Argo CD sync policies?** – Auto-sync, manual.
33. **Rollback in Argo CD?** – Select previous revision.
34. **CI vs CD?** – CI = build/test, CD = deploy.
35. **Bitbucket Pipeline file?** – bitbucket-pipelines.yml.
36. **Jenkins pipeline type?** – Scripted, declarative.
37. **SonarQube purpose?** – Code quality scanning.
38. **Artifact repo example?** – Nexus, Artifactory, ECR.
39. **Blue-Green deployment?** – Zero downtime switch.
40. **Canary deployment?** – Partial traffic rollout.

---

### **Linux / Scripting**

41. **Check disk usage?** – df -h
42. **Find top CPU process?** – top / ps -aux --sort=-%cpu
43. **Make script executable?** – chmod +x script.sh
44. **Crontab syntax?** – minute hour day month weekday
45. **Shebang meaning?** – Script interpreter declaration.
46. **Log rotation tool?** – logrotate
47. **Check open ports?** – netstat -tulnp / ss -tuln
48. **Kill process by name?** – pkill -f process
49. **SSH key generation?** – ssh-keygen -t rsa
50. **Archive folder?** – tar -czvf archive.tar.gz folder

---

### **Security / Best Practices**

51. **Least privilege principle?** – Minimum access needed.
52. **Kubernetes secret encryption?** – Enable envelope encryption.
53. **Image vulnerability scan tools?** – Trivy, AquaSec.
54. **IAM inline vs managed policy?** – Inline = single entity, managed = reusable.
55. **Pod Security Policy status?** – Deprecated → use OPA/Gatekeeper.
56. **EKS IRSA use case?** – Fine-grained IAM for pods.
57. **TLS termination?** – At ingress or load balancer.
58. **RDS encryption?** – KMS-based at rest.
59. **S3 public access block?** – Prevent accidental exposure.
60. **Drift in GitOps?** – Out-of-sync detected by Argo CD.

---

### **Troubleshooting**

61. **Pipeline failed at build stage?** – Check logs, dependencies, environment.
62. **EKS pod Pending?** – Node resources, scheduling, taints/tolerations.
63. **Service not reachable?** – DNS, endpoints, firewall.
64. **Terraform apply stuck?** – API limits, lock state check.
65. **AWS bill spike?** – Cost Explorer, unused resources, rightsizing.
66. **Argo CD app out-of-sync?** – Manual change outside Git.
67. **Pod OOMKilled?** – Increase memory limits.
68. **Slow website on ALB?** – Health checks, backend latency.
69. **Secret not injected in pod?** – Check mounts, RBAC, namespace.
70. **DNS not resolving in pod?** – CoreDNS, network policy.

---

### **Cloud Patterns & Governance**

71. **Multi-AZ vs Multi-Region?** – HA vs DR.
72. **S3 lifecycle policy?** – Move to cheaper storage/expire.
73. **Cloud tagging use case?** – Cost allocation, governance.
74. **VPC peering vs Transit Gateway?** – 1:1 vs hub-spoke.
75. **CloudFront purpose?** – CDN + caching.
76. **WAF function?** – Block malicious traffic.
77. **KMS CMK vs AWS-managed key?** – Customer control vs AWS managed.
78. **Cost optimization tool?** – AWS Trusted Advisor.
79. **Blue/Green DNS switch?** – Route53 weighted routing.
80. **Service quota increase?** – AWS Support request.


## **Notes Q\&A**

### **1. Design a highly available EKS cluster for production workloads**

**WS:**
I’d provision an EKS cluster across at least 3 Availability Zones for HA. Worker nodes would be in multiple subnets (private for workloads, public for ingress). I’d enable cluster autoscaler, set up managed node groups, and integrate an ALB Ingress Controller for routing. For persistence, I’d use EBS or EFS depending on workloads. Security would include network policies, RBAC, IRSA for pods, and Secrets Manager for sensitive data. Monitoring via Prometheus + Grafana, and logging via Fluent Bit to CloudWatch.
<img width="2310" height="1540" alt="image" src="https://github.com/user-attachments/assets/b43d1f53-d6e6-4c6b-b231-667e86e655c2" />

---

### **2. Manage secrets in your pipelines**

**WS:**
I avoid hardcoding secrets — instead, I integrate AWS Secrets Manager or SSM Parameter Store with pipelines. For Kubernetes, I use sealed-secrets or external-secrets operator to inject secrets at runtime. Access is controlled via IAM roles, ensuring least privilege. For example, in one project, we replaced all plain Kubernetes secrets with sealed-secrets encrypted via KMS, reducing exposure risks.

---

### **3. If a pod in EKS is in CrashLoopBackOff, Troubleshoot Steps:**

**WS:**

1. kubectl describe pod → check events & last exit code. (This is usually the first thing I do but oftem missed by Dev Teams)
2. kubectl logs → review application logs.
3. Check readiness/liveness probe configurations.
4. Validate image pull secrets & container registry access.
5. If OOMKilled, adjust resources.requests/limits.
6. If config-related, fix environment variables or ConfigMaps.
   I also check node conditions (kubectl get nodes) to ensure no resource pressure.

### **3. What’s your approach for disaster recovery in AWS?**

**Answer:**
For EKS workloads, I’d back up etcd snapshots (via Velero) and persistent volumes (EBS/EFS). For databases, I’d enable automated RDS snapshots and cross-region replication. S3 buckets would have versioning and replication enabled. Terraform state would be stored in S3 with cross-region replication for infra restore.

-

☁️ AWS DevOps Engineering Reference Guide

This repository documents practical AWS DevOps patterns, infrastructure automation strategies, Kubernetes (EKS) deployment models, CI/CD architectures, security controls, and operational playbooks.

It reflects real-world implementation experience across financial services and regulated environments.

Core AWS DevOps Patterns
1️⃣ Highly Available EKS Architecture
4

Architecture Components:

Multi-AZ EKS cluster (3 Availability Zones)

Private subnets for worker nodes

Public subnets for ALB ingress

Managed Node Groups or Karpenter autoscaling

IAM Roles for Service Accounts (IRSA)

AWS Secrets Manager integration

Prometheus + Grafana monitoring

Fluent Bit → CloudWatch logging

Design Goals:

High availability

Horizontal scalability

Compliance-ready security posture

GitOps-based deployment governance

2️⃣ Infrastructure as Code (Terraform)

State Management

Remote backend: S3

State locking: DynamoDB

Versioned state files

Cross-region replication for DR

Best Practices

Modularized Terraform structure

Environment separation (dev, stage, prod)

Drift detection via scheduled terraform plan

Security scanning using tfsec / checkov

Command Workflow

terraform init
terraform plan
terraform apply

3️⃣ GitOps Deployment Model (Argo CD + EKS)
4

Workflow

Developer commits to Git

CI pipeline builds container image

Image scanned (Trivy / AquaSec)

Image pushed to Amazon ECR

Argo CD syncs manifests

Deployment rolled out to EKS

Environment Strategy

Environment	Sync Policy
Dev	Auto-sync
Stage	Manual Approval
Prod	Controlled promotion

Rollback Strategy

Git revision rollback

Argo CD history restore

Canary + blue/green deployments

💰 AWS Cost Optimization Framework

Optimization Techniques

Right-size EC2 & EKS nodes

Mixed node groups (On-Demand + Spot)

Karpenter dynamic scaling

Savings Plans & Reserved Instances

Automated off-hours scale-down (Lambda)

S3 lifecycle policies

Result Example

35% reduction in EKS infrastructure costs

Improved utilization via autoscaling

🔐 Security & Compliance Controls

Identity & Access

IAM least privilege

IRSA for pod-level permissions

Role assumption model

Secrets Management

AWS Secrets Manager

KMS encryption at rest

External Secrets Operator for Kubernetes

Container Security

Minimal base images

Trivy vulnerability scanning

Cosign image signing

Private ECR repositories

Encryption

RDS encrypted with KMS

S3 versioning + encryption

TLS termination at ALB

🛠 CI/CD Reference Architecture

Pipeline Stages

Code commit (Git)

Build & unit test

Docker image build

Image scan

Push to ECR

Deploy via Argo CD

Integration tests

Production approval gate

Tooling

Jenkins / Bitbucket Pipelines

SonarQube

Amazon ECR

Argo CD

Jira integration for traceability

📊 Disaster Recovery Strategy
4

DR Components

Velero for EKS backup/restore

RDS automated snapshots

Cross-region S3 replication

Terraform state replication

Documented runbooks

Quarterly failover drills

Achieved Metrics

RTO < 1 hour



# ☁️ AWS Enterprise Solutions Architecture Portfolio

![AWS](https://img.shields.io/badge/AWS-Cloud%20Architecture-FF9900?logo=amazon-aws)
![Terraform](https://img.shields.io/badge/IaC-Terraform-623CE4?logo=terraform)
![Kubernetes](https://img.shields.io/badge/Container-EKS-326CE5?logo=kubernetes)
![Security](https://img.shields.io/badge/Security-Cloud%20Native-green)
![Architecture](https://img.shields.io/badge/Role-Solutions%20Architect-blue)

---

## Overview

This repository documents enterprise-grade AWS cloud architecture patterns designed for:


# 🏗️ Reference Architecture – Production EKS Platform

## High-Level Architecture

```mermaid
flowchart TD

Users --> Route53
Route53 --> ALB
ALB --> EKSCluster

subgraph AWS VPC
    subgraph Public Subnets
        ALB
    end

    subgraph Private Subnets
        EKSCluster
        WorkerNodes
        RDS
    end
end

EKSCluster --> WorkerNodes
WorkerNodes --> ECR
WorkerNodes --> SecretsManager
WorkerNodes --> CloudWatch
WorkerNodes --> S3

RDS --> KMS
S3 --> KMS
SecretsManager --> KMS

🔁 GitOps Deployment Architecture
flowchart LR

Developer --> GitRepo
GitRepo --> CI_Pipeline
CI_Pipeline --> ECR
GitRepo --> ArgoCD
ArgoCD --> EKS

CI_Pipeline --> SecurityScan
SecurityScan --> ECR


