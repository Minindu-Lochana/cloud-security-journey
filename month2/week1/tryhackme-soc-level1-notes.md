
# TryHackMe SOC Level 1 — Week 1 Notes

## Room 1: Cyber Defence Frameworks

### MITRE ATT&CK
A structured knowledge base of real-world adversary behavior organized into Tactics (goals), Techniques (actions), and Procedures. It provides a common language across SOC analysis, detection engineering, and threat hunting.

### Cyber Kill Chain (7 Stages)
1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command & Control (C2)
7. Actions on Objectives

### Pyramid of Pain
- Hash Values (Trivial to change)
- IP Addresses (Easy)
- Domain Names (Simple)
- Network & Host Artifacts (Annoying)
- Tools (Challenging)
- TTPs (Tough to change; provides highest defensive return)

### Cloud/GuardDuty ATT&CK Mapping
- GuardDuty: `Stealth:CloudTrailLoggingDisabled` -> T1562.001 (Impair Defenses: Disable or Modify Tools)
- GuardDuty: `CryptoCurrency:EC2/BitcoinTool.B!DNS` -> T1496 (Resource Hijacking)

---

## Room 2: Cyber Threat Intelligence

### Threat Intelligence Tiers
- **Strategic:** High-level trends and threat landscapes for leadership.
- **Operational:** Threat groups (APTs), campaigns, and actor intentions.
- **Tactical:** TTPs and adversary behavior patterns (MITRE-aligned).
- **Technical:** Tactical IOCs (IPs, hashes, domains) fed directly into detection tools.

### Indicators of Compromise (IOC)
Forensic evidence indicating an asset has been breached or targeted.
- Example 1: `pool.minergate.com` (Cryptomining mining pool domain)
- Example 2: MD5/SHA256 hash of an unauthorized reverse shell binary
- Example 3: Known malicious C2 IP address (e.g., scanning automated web roots)

### VirusTotal Analysis
- Target: `pool.minergate.com`
- Finding: Flagged across multiple vendors as a crypto mining pool endpoint.
- AWS Tie-in: GuardDuty checks DNS query logs against threat intelligence feeds containing these domains to generate cryptocurrency alerts.

---

## Room 3: Network Security and Traffic Analysis

### IDS vs. IPS
- **IDS (Intrusion Detection System):** Passively monitors and alerts on traffic matches without modifying packets (AWS analog: Amazon GuardDuty).
- **IPS (Intrusion Prevention System):** Inline device that inspects traffic and actively drops/blocks unauthorized packets (AWS analog: AWS Network Firewall / AWS WAF).

### Port 4444 Analysis
Port 4444 is historically the default listener port for Metasploit reverse shells. Observing outbound connections to external port 4444 in VPC Flow Logs strongly points to an established reverse shell / active command-and-control session.