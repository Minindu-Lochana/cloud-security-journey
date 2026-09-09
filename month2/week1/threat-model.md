# Threat Model — My AWS Learning Environment

**Framework:** STRIDE  
**System:** Personal AWS learning account (ap-south-1)  

## Components
- EC2 instance (t2.micro, Amazon Linux 2023)
- S3 bucket
- IAM user / roles
- VPC (default VPC, Mumbai region)
- CloudTrail (multi-region trail)
- GuardDuty (enabled)

## STRIDE Analysis

### EC2 Instance
| Threat | Scenario | Current Control | Gap |
|---|---|---|---|
| Spoofing | Attacker uses stolen SSH key | Key pair auth + restrict to My IP | No centralized IAM Identity Center |
| Tampering | Attacker modifies web files | Default Linux permissions | No File Integrity Monitoring (FIM) |
| Repudiation | Attacker clears OS log files | CloudTrail tracks AWS API level actions | CloudWatch Agent not pushing OS logs |
| Info Disclosure | Port 22 open to internet | SG restricted to My IP | Instance runs in a public subnet |
| Denial of Service | HTTP flood crashes web server | None | No AWS WAF or Auto Scaling Group |
| Elevation of Privilege | Attacker breaks out of web process | App runs as ec2-user (non-root) | IMDSv2 not enforced (SSRF risk)
