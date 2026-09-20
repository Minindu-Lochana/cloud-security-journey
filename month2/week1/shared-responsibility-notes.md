# AWS Shared Responsibility Model

## 1. Summary of the Model
The AWS Shared Responsibility Model outlines the division of security obligations between AWS and the customer. AWS manages security "OF" the cloud, covering physical infrastructure, hardware, hypervisors, and core networking facilities across global regions. The customer is strictly responsible for security "IN" the cloud, which includes data protection, Identity and Access Management (IAM), network configurations, firewall settings (Security Groups), and guest operating system maintenance.

## 2. Breakdown by Service Type
* **EC2 (IaaS):** Offers the greatest customer control and responsibility. AWS handles physical server hardware and the hypervisor, while the customer must manage guest OS patching, firewall configurations, IAM permissions, and application-level security.
* **S3 (Object Storage):** A fully managed storage tier where AWS guarantees 11 nines of durability and physical storage resilience. The customer is solely responsible for bucket access policies, enabling Block Public Access, configuring default encryption, and monitoring access logs.
* **RDS (PaaS):** A managed relational platform where AWS manages physical hardware, OS-level maintenance, automatic backups, and minor database engine updates. The customer manages DB user access, table privileges, connection security groups, encryption keys, and safeguards against application-level vulnerabilities like SQL injection.
* **Lambda (Serverless):** An event-driven compute tier where AWS manages the underlying hardware, operating system, scaling, and execution runtimes. The customer is exclusively responsible for function source code, input sanitization, library dependencies, secrets management, and assigning least-privilege IAM execution roles.

## 3. Interview Focus: Who Patches an EC2 Instance?
The customer is entirely responsible for patching the guest operating system on an EC2 instance. While AWS updates the underlying physical host and hypervisor, customers must routinely execute system updates (such as `dnf update` or `apt upgrade`) or orchestrate automated patching via AWS Systems Manager (SSM) Patch Manager.

## 4. Real-World Breach: Customer Responsibility Failure
* **Incident:** The 2019 Capital One data breach.
* **Root Cause:** A misconfigured open-source Web Application Firewall (WAF) hosted on an EC2 instance allowed a Server-Side Request Forgery (SSRF) attack. The attacker exploited this flaw to query the EC2 Instance Metadata Service (IMDSv1), stole the attached IAM role credentials, and extracted sensitive data from misconfigured S3 buckets.
* **Takeaway:** The incident stemmed from customer-managed IAM privileges, an unpatched application vulnerability, and improper metadata service hardening—not a failure of AWS's underlying infrastructure.
