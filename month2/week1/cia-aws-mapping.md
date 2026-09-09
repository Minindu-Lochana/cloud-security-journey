# AWS Services Mapped to CIA Triad

## S3 Bucket Review
* **Confidentiality:**
  - Control: Block Public Access is ON
  - Control: Default SSE-S3 encryption is enabled
  - Gap: Bucket access logging is disabled
* **Integrity:**
  - Control: CloudTrail tracks S3 management events
  - Gap: Bucket Versioning is disabled (risk of silent file overwrites)
* **Availability:**
  - Control: Standard S3 11 9's durability
  - Gap: No Cross-Region Replication configured

## EC2 Review
* **Confidentiality:**
  - Control: Security Group restricts SSH to My IP
  - Gap: Running in a public subnet
  - Gap: IMDSv2 is not strictly enforced
* **Integrity:**
  - Control: CloudTrail logs EC2 start/stop actions
  - Gap: No OS-level File Integrity Monitoring (FIM)
* **Availability:**
  - Control: Manual reboot/recovery
  - Gap: Single instance in a single AZ (no Auto Scaling or load balancer)
