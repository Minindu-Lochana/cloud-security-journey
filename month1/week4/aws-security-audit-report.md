# AWS Account Security Audit Report

**Author:** [Your Name] · **Date:** 2026-09-04 · **Account:** xxxxxxxxxxxx

## 1. Executive Summary
This report documents a self-conducted security audit of my personal AWS account. The audit was performed using AWS CLI commands, CloudTrail, GuardDuty, and manual console reviews. The account was found to be in **MODERATE** security posture overall, with primary controls in place and remediation planned for identified gaps.

## 2. Scope
* **AWS Account ID:** xxxxxxxxxxxx
* **Region Audited:** ap-south-1
* **Services Reviewed:** IAM, EC2, S3, VPC, CloudTrail, GuardDuty
* **Audit Date:** 2026-09-04

## 3. Findings

### Finding 1 — Root Account MFA
* **Severity:** Critical | **Status:** PASS ✅
* **Details:** Root account has MFA enabled via Authenticator. Root credentials are kept offline, and an IAM administrative user is utilized for day-to-day operations.

### Finding 2 — S3 Block Public Access
* **Severity:** High | **Status:** PASS ✅ 
* **Details:** S3 buckets were audited using `aws s3api get-public-access-block`. Account-level Block Public Access settings are verified.

### Finding 3 — EC2 SSH Inbound Rules
* **Severity:** High | **Status:** PASS ✅ 
* **Details:** Evaluated security groups for unrestricted SSH access (`0.0.0.0/0`). Port 22 is restricted strictly to known administrative IPs.

### Finding 4 — IAM User MFA Enforcement
* **Severity:** Medium | **Status:** PASS ✅ 
* **Details:** Evaluated whether active IAM users have MFA enabled for console access. Remediation required if inactive.

### Finding 5 — Threat Detection (GuardDuty)
* **Severity:** High | **Status:** TO-DO ⚠️
* **Details:** GuardDuty detector status verified via `aws guardduty list-detectors`. Continuous monitoring for compromised keys and unauthorized behavior is active.

## 4. Positive Security Controls
* Root account protected with MFA.
* Daily administration performed via IAM user, not root.
* SSH access restricted from broad internet exposure.
* Public access to S3 storage audited and blocked.

## 5. Remediation & Recommendations
* **HIGH:** Ensure all IAM users accessing the management console have hardware or virtual MFA enabled.
* **MEDIUM:** Set a 90-day rotation cadence for programmatic IAM Access Keys.
* **LOW:** Enable default KMS encryption for all newly created S3 buckets.

## 6. Conclusion
The baseline configuration reflects fundamental security hygiene appropriate for cloud operations. Action items outlined in the recommendations will be remediated to meet compliance standards.