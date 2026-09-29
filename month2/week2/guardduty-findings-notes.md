
# GuardDuty Findings — Week 2 Day 1

## My GuardDuty Setup

**Detector ID:** 
fed01c98de207c2bba4ac7f04153dd3f


**AWS Region:**  
us-east-1

**GuardDuty Status:**  
ENABLED

---

## Findings I Investigated

Today I generated some sample GuardDuty findings and tried to understand what each alert means and what I should do as a security analyst.

### 1. CryptoCurrency:EC2/BitcoinTool.B!DNS

**Severity:** 8.0 — HIGH

**What happened:**  
This finding means an EC2 instance was trying to communicate with a domain that is known to be related to cryptocurrency mining.

**Affected resource:**  
YOUR_EC2_INSTANCE_ID

**What I would do first:**  
I would isolate the EC2 instance and preserve the evidence before taking further action.

---

### 2. UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B

**Severity:** 7.0 — HIGH

**What happened:**  
This finding represents a successful IAM console login from an unusual location or IP address.

**Affected resource:**  
YOUR_IAM_USER

**What I would do first:**  
I would first check whether the login was actually made by the user. If it was not legitimate, I would secure the IAM account and its credentials.

---

### 3. Stealth:IAMUser/CloudTrailLoggingDisabled

**Severity:** 5.0 — MEDIUM

**What happened:**  
This finding means CloudTrail logging was disabled.

This is important because CloudTrail helps us see what is happening in the AWS account. If logging is disabled, some activities may not be recorded.

**Affected resource:**  
CloudTrail

**What I would do first:**  
I would re-enable CloudTrail and check the `StopLogging` event to find out when and how the logging was disabled.

---

### 4. Recon:EC2/PortProbeUnprotectedPort

**Severity:** 5.0 — MEDIUM

**What happened:**  
An external IP address was probing ports on an EC2 instance.

This could be part of an attacker trying to find open services.

**Affected resource:**  
YOUR_EC2_INSTANCE_ID

**What I would do first:**  
I would check the Security Group and make sure that unnecessary ports are not publicly accessible.

---

### 5. Policy:S3/BucketPublicAccessGranted

**Severity:** 5.0 — MEDIUM

**What happened:**  
An S3 bucket was made publicly accessible.

This could become a security problem if the bucket contains sensitive information.

**Affected resource:**  
YOUR_S3_BUCKET

**What I would do first:**  
I would enable S3 Block Public Access again and then investigate whether any data was exposed.

---

## Severity Levels I Learned

While doing this lab, I learned that GuardDuty findings have different severity levels.

- **HIGH (7.0 – 8.9):** Should be investigated quickly.
- **MEDIUM (4.0 – 6.9):** Should be investigated after high-priority findings.
- **LOW (0.1 – 3.9):** Usually has a lower priority.

---

## What I Learned Today

Today I learned the basic workflow of investigating security findings with GuardDuty.

Some important things I learned:

- GuardDuty is an AWS managed threat detection service.
- GuardDuty creates **findings** when it detects suspicious activity.
- A finding gives information about what happened and which AWS resource is involved.
- Sample findings are useful for learning how security alerts work.
- When investigating a finding, I should look at things like the affected resource, timestamp, source IP and CloudTrail activity.
- A security alert does not automatically mean that an attack really happened. In this lab, I was working with **sample findings** to practice investigation.

## My Main Takeaway

Before this lab, I knew GuardDuty was an AWS security service, but I did not really understand what a **finding** looked like.

After investigating these examples, I have a better idea of how a security analyst can receive an alert, understand what happened, identify the affected AWS resource, and decide what should be checked next.
