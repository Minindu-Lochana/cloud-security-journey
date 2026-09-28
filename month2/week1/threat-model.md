# Threat Model — My AWS Learning Environment

**Author:** Minindu Lochana
**Date:** 28 September 2026
**Framework:** STRIDE
**Review date:** 28 October 2026

## 1. System Overview

This threat model covers my personal AWS learning account used for cloud security training.

**Region:** us-east-1 (N. Virginia)

**Components:**
- EC2 instance
- S3 bucket
- IAM user
- VPC
- CloudTrail
- GuardDuty
- AWS Config# Threat Model — My AWS Learning Environment

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



## 3. STRIDE Analysis — S3 Bucket

| Threat | Attack Scenario | Severity | Current Control | Gap | Remediation |
|---|---|---|---|---|---|
| **Spoofing** | Stolen credentials are used to upload malicious files | HIGH | IAM authentication | IAM user MFA may not be enabled | Enable MFA on the IAM user |
| **Tampering** | Attacker overwrites important files | MEDIUM | Encryption enabled | Versioning not enabled | Enable S3 versioning |
| **Repudiation** | Attacker downloads files and denies doing so | MEDIUM | CloudTrail records API activity | Additional access logging may be useful | Review S3 logging requirements |
| **Information Disclosure** | Bucket is accidentally made public | HIGH | S3 Block Public Access | Sensitive-data discovery not enabled | Review access and consider appropriate data-discovery controls |
| **Denial of Service** | Attacker deletes objects | HIGH | No additional recovery protection | Versioning/MFA Delete not enabled | Enable versioning and appropriate deletion protection |
| **Elevation of Privilege** | Bucket policy provides more permissions than intended | MEDIUM | IAM controls access | Policies require periodic review | Review bucket/IAM policies regularly |


## 4. STRIDE Analysis — IAM User

| Threat | Attack Scenario | Severity | Current Control | Gap | Remediation |
|---|---|---|---|---|---|
| **Spoofing** | Access keys are leaked and used by an attacker | CRITICAL | No keys committed to GitHub | Key age may not be monitored | Rotate credentials and use short-lived credentials where possible |
| **Tampering** | Attacker modifies IAM policies | HIGH | CloudTrail records IAM changes | Excessive permissions increase impact | Apply least privilege |
| **Repudiation** | Unauthorized IAM changes are denied | LOW | CloudTrail records activity | Shared accounts reduce accountability | Use individual identities |
| **Information Disclosure** | Credentials are exposed through an EC2 metadata attack | HIGH | IMDSv2 enforced | Long-lived credentials increase risk | Prefer short-lived credentials |
| **Denial of Service** | Account access is lost | MEDIUM | Root account available as fallback | Recovery procedures may be incomplete | Maintain secure account-recovery procedures |
| **Elevation of Privilege** | User obtains permissions beyond what is required | MEDIUM | Existing permissions reviewed | Administrator-level permissions create large blast radius | Apply least privilege and permission boundaries where appropriate |


## 5. Risk Summary

| Priority | Finding | STRIDE | Remediation |
|---|---|---|---|
| HIGH | IAM user MFA not enabled | Spoofing | Enable MFA |
| HIGH | No file integrity monitoring on EC2 | Tampering | Install AIDE |
| MEDIUM | S3 versioning not enabled | Tampering / DoS | Enable versioning |
| MEDIUM | OS logs not shipped to CloudWatch | Repudiation | Configure CloudWatch logging |
| MEDIUM | No Auto Scaling for EC2 | DoS | Consider appropriate production architecture |
| LOW | Additional S3 access logging not enabled | Repudiation | Review logging requirements |


## 6. Controls Already in Place

- IMDSv2 enforced on EC2
- S3 Block Public Access enabled
- CloudTrail enabled
- GuardDuty enabled
- AWS Config enabled
- SSH restricted to My IP
- Root account MFA enabled
- EC2 web server does not run as root
