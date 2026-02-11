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

Fast forward hello !!!  :
With modularization of Medicaid modules, as shown in the following figure, the monolithic application is broken into multiple smaller applications. Each application (for claims and encounters, provider, pharmacy, and so on) might be managed by different vendors using different databases to store the corresponding data. The data still gets shipped to the EDW through the systems integrator (SI) module on a weekly or monthly basis for the established canned reports. But the states now must join the data across multiple modules to gather the operational insights they used to get from their monolithic application. With this, state agencies are forced to depend on the EDW for operational analytics. But with the rigid schema, this model could cause additional delays in getting actionable insights.
<img width="699" height="699" alt="image" src="https://github.com/user-attachments/assets/0f6a1a2e-8870-45e2-bbbe-a21c1012939c" />

## **🚀 The Architecture **

<img width="1430" height="667" alt="image" src="https://github.com/user-attachments/assets/4b068dc8-6200-4490-afee-49e300f03bcd" />

**1. Data sources:** This is a database of choice for the vendor implementing a module. The approach used to build ODS should be able to accommodate a wide variety of databases.


Quick word about security:
CMS is deploying a seven-tiered framework, as shown in Figure 1, for managing the
administrative, operational, and technical aspects of security and privacy of ACA systems.
The Minimum Acceptable Risk Standards (Tier 4) are central to the framework. These
standards are founded on:
• Tier 1 – Federal Legislation and Executive Mandates
• Tier 2 – HHS ACA Regulations
• Tier 3 – Federal Regulations and Guidance
• Tier 4 – Minimum Acceptable Risk Standards for Administering Entities
Tiers 5, 6, and 7 are instrumental to implementing the Minimum Acceptable Risk Standard

<img width="1002" height="537" alt="image" src="https://github.com/user-attachments/assets/e28a27ae-fa33-4091-976c-41103d6aff8b" />
no kidding -here is the linl https://www.cms.gov/files/document/mars-e-v2-2-vol-1final-signed08032021-1.pdf


**2. Data ingestion:** AWS Glue efficiently handles mainframe bulk data ingestion, and AWS Marketplace offers solutions for capturing delta changes. For relational database migration, AWS Database Migration Service (AWS DMS) provides seamless transfer capabilities. AWS DataSync facilitates smooth data movement from existing data lakes, and Amazon Simple Queue Service (Amazon SQS) enables real-time ingestion through a flexible publish-subscribe framework for streaming sources.
https://aws.amazon.com/glue/

**3. Bronze data storage:** Store your data in a straightforward, low-cost, highly resilient storage that you can always come back to for auditing and provenance determinations. Amazon S3 Glacier is used to archive historical raw data for long-term retention while maintaining accessibility for compliance requirements and enabling retrieval when needed for data lineage verification or reprocessing through your analytics pipeline.

**4. Catalog and data quality:** A data catalog is essential in a modern data architecture because it serves as a centralized inventory system that documents and organizes metadata about all data assets, making data discovery and understanding efficient across the organization. AWS Glue Data Quality uses machine learning (ML) algorithms to automate data quality management in data lakes, reducing manual efforts from days to hours with automatic statistics, rule recommendations, monitoring, and alerts. AWS Glue DataBrew is used to cleanse data.  https://aws.amazon.com/glue/features/data-quality/

**5. Silver data storage:** Clean data has been validated and standardized but maintains its original granularity separately from raw data. This approach is essential for artificial intelligence and machine learning (AI/ML) applications that typically require access to cleaned but minimally transformed data while also facilitating transparent data lineage tracking throughout the data lifecycle.

**6. Data transformation and entity resolution:** AWS Glue provides serverless extract, transform, and load (ETL) capabilities to transform and prepare data at scale, and AWS Entity Resolution identifies and resolves duplicate or conflicting records across different systems. The native transformation features of AWS Glue enable data normalization, aggregation, and enrichment, promoting data accuracy and preventing redundancy while maintaining consistent identifiers across the Medicaid ecosystem.

**7. Operational data store (Gold Zone):** Serves as the organization’s single source of truth by housing transformed, validated, and business-ready datasets in optimized formats, providing data quality, compliance, and governance while enabling efficient self-service analytics and ML applications through standardized schemas.

**8. Consumption layer –** Analytics and dashboard: Amazon Redshift delivers high-performance data warehousing for complex analytical queries across massive datasets, providing the computational power needed for business intelligence workloads. Amazon Athena offers serverless SQL querying directly against your Amazon S3 data lake, enabling immediate insights without data movement or infrastructure management. Amazon QuickSight transforms these analytical results into intuitive, interactive dashboards and visualizations that make data accessible to all stakeholders through its cloud-native business intelligence platform. The layer could also house the state’s EDW eventually. I have used Cognos and PowerBi and perfer those over QuickSight. There are cost considerations but makes the solution less rigid. 

**9. Consumption layer**  – Data sharing: Amazon DataZone creates a unified data management environment specifically designed for simplified data sharing and discovery. This platform provides a business-friendly data catalog where data producers can publish curated datasets with clear documentation, quality metrics, and usage policies. Consumers can easily discover, request access to, and integrate information through Amazon DataZone self-service capabilities, bridging organizational boundaries. The solution maintains end-to-end data lineage, enforces security policies, and provides regulatory compliance while providing a streamlined experience for data sharing across teams, departments, and external partners, creating a secure yet accessible data ecosystem for all stakeholders.

**10. Consumption layer –** AI/ML: This layer represents the final stage in modern data architecture, where processed data transforms into actionable insights through enterprise data warehousing solutions and advanced AI capabilities. Amazon Bedrock provides foundation models (FMs) to power generative AI applications that can analyze patterns, predict outcomes, and automate decision processes with minimal coding. Combined with traditional analytics tools, this creates a comprehensive intelligence environment—enabling high-performance analytics, interactive dashboards, self-service reporting, and sophisticated AI/ML applications while maintaining security through role-based access controls and delivering customizable visualizations that support both business users and automated systems.

**11. Consumption layer – AI/ML:** - Call me :) 

**Consumption layer – Data collaboration** call me :)

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

### **4. Ahhh...Sigh..here is comes - My approach for disaster recovery in AWS**

**Answer:**
For EKS workloads, I’d back up etcd snapshots (via Velero) and persistent volumes (EBS/EFS). For databases, I’d enable automated RDS snapshots and cross-region replication. S3 buckets would have versioning and replication enabled. Terraform state would be stored in S3 with cross-region replication for infra restore.

-



# ☁️ AWS Enterprise Solutions Architecture Portfolio

![AWS](https://img.shields.io/badge/AWS-Cloud%20Architecture-FF9900?logo=amazon-aws)
![Terraform](https://img.shields.io/badge/IaC-Terraform-623CE4?logo=terraform)
![Kubernetes](https://img.shields.io/badge/Container-EKS-326CE5?logo=kubernetes)
![Security](https://img.shields.io/badge/Security-Cloud%20Native-green)
![Architecture](https://img.shields.io/badge/Role-Solutions%20Architect-blue)

---
