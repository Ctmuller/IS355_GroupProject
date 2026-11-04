# Security Risk Assessment and Controls

> **Status: DRAFT (in progress).** Likelihood and impact scores are the group's working estimates and should be reviewed by all members. The full risk register will also be entered into the course risk assessment spreadsheet template and linked here.

---

## 1. Scope and Method

This assessment covers the manufacturing company's proposed network: the on-site office and factory, the seven VLANs in our network design, and the cloud-hosted web and backup servers.

We followed a simple qualitative approach based on NIST SP 800-30:

1. Identify and classify the important assets.
2. Identify the threats and vulnerabilities that affect each asset.
3. Score each risk by **likelihood** and **impact**.
4. Select security controls for the highest risks and map them to the NIST Cybersecurity Framework (CSF) 2.0 and the CIS Critical Security Controls v8.

### Scoring Scale

| Score | Likelihood | Impact |
|---|---|---|
| 1 | Rare — very unlikely in the next year | Minimal — little effect on operations |
| 2 | Unlikely | Minor — short disruption, easily recovered |
| 3 | Possible | Moderate — noticeable disruption or data exposure |
| 4 | Likely | Major — production stops or sensitive data is exposed |
| 5 | Almost certain | Severe — extended shutdown, major financial or IP loss |

**Risk score = Likelihood × Impact**

| Score | Risk Level |
|---|---|
| 1–5 | Low |
| 6–11 | Medium |
| 12–19 | High |
| 20–25 | Critical |

---

## 2. Asset Identification and Classification

| ID | Asset | Location | Classification | Value | CIA Priority |
|---|---|---|---|---|---|
| A1 | Engineering designs and prototype files | File server, VLAN 50 | Restricted | High | Confidentiality, Integrity |
| A2 | Financial and HR records | File server / accounting system, VLAN 50 | Restricted | High | Confidentiality, Integrity |
| A3 | Factory PLCs, HMIs, and PLC programs | VLAN 30 | Confidential | High | Integrity, Availability |
| A4 | Active Directory (user accounts and authentication) | Domain controller, VLAN 50 | Restricted | High | Confidentiality, Integrity, Availability |
| A5 | Firewall and core switch (network infrastructure) | VLAN 10 | Restricted | High | Integrity, Availability |
| A6 | Encrypted cloud backups | Cloud backup server | Restricted | High | Confidentiality, Integrity, Availability |
| A7 | Security cameras and NVR footage | VLAN 40 / VLAN 50 | Confidential | Medium | Integrity, Availability |
| A8 | Public company website | Cloud web server | Public | Medium | Integrity, Availability |
| A9 | Office computers and laptops | VLAN 20 / VLAN 60 | Internal | Medium | Confidentiality, Integrity |

**Classification levels:** Public (anyone), Internal (all employees), Confidential (specific roles), Restricted (small named group; exposure causes serious harm).

---

## 3. Threats and Vulnerabilities

| Threat | Motivation | Vulnerabilities Exploited | Assets Affected |
|---|---|---|---|
| Ransomware groups | Financial gain | Phishing emails, unpatched software, backups reachable from the network | A1, A2, A4, A6, A9 |
| Phishing attackers | Credential theft, fraud | Staff not trained to spot phishing, single-factor logins | A2, A4, A9 |
| Competitors / industrial espionage | Steal intellectual property | Broad file-share permissions, no monitoring of large file transfers | A1 |
| Disgruntled employees (insiders) | Revenge or personal gain | Excess permissions, accounts not removed when staff leave | A1, A2, A3 |
| External hackers | Access or disruption | Unpatched firewall/switch firmware, exposed management interfaces | A5, A8 |
| Compromised contractors or vendors | Use of existing remote access | Shared or permanent vendor accounts, direct access to factory systems | A3, A7 |
| Attackers targeting IoT devices | Foothold into the network | Default camera passwords, outdated camera firmware | A7 |
| Web attackers | Defacement, data theft | Outdated CMS and plugins, weak admin login | A8 |
| Non-malicious events | — | Power loss, hardware failure, single core switch and firewall | A3, A5 |

---

## 4. Risk Register

| Risk ID | Risk Description | Assets | L | I | Score | Level |
|---|---|---|---|---|---|---|
| R1 | Ransomware delivered by phishing encrypts the file server and on-network backups | A1, A2, A6, A9 | 4 | 5 | 20 | **Critical** |
| R2 | Employee credentials stolen by phishing and used to access email, VPN, or file shares | A2, A4, A9 | 4 | 4 | 16 | **High** |
| R3 | Malware spreads from an office device to factory PLCs/HMIs, stopping production | A3 | 3 | 5 | 15 | **High** |
| R4 | Unpatched vulnerability in the firewall or switches is exploited remotely | A5 | 3 | 5 | 15 | **High** |
| R5 | Engineering designs stolen by an insider or competitor | A1 | 3 | 5 | 15 | **High** |
| R6 | Firewall or core switch fails, or power is lost, causing a site-wide outage | A3, A5 | 3 | 4 | 12 | **High** |
| R7 | Contractor remote access misused to change PLC programs | A3 | 2 | 5 | 10 | Medium |
| R8 | IP camera compromised and used as a foothold, or footage tampered with | A7 | 3 | 3 | 9 | Medium |
| R9 | Public website defaced or compromised through an outdated CMS | A8 | 3 | 3 | 9 | Medium |
| R10 | Guest Wi-Fi user bypasses isolation and reaches internal VLANs | A9 | 2 | 3 | 6 | Medium |

**Highest risks:** R1 (ransomware) is the only Critical risk. R2 to R6 are High and are the focus of the controls in Section 5.

---

## 5. Security Controls

Controls were chosen to reduce the Critical and High risks first. Each one maps to the NIST CSF 2.0 and the CIS Critical Security Controls v8.

| ID | Control | Risks Addressed | NIST CSF 2.0 | CIS Controls v8 |
|---|---|---|---|---|
| C1 | Multi-factor authentication (MFA) for VPN, email, cloud, and all admin accounts | R1, R2, R7 | PR.AA-03 | 6.3, 6.4, 6.5 |
| C2 | Immutable, encrypted off-site backups with monthly restore testing | R1, R5 | PR.DS-11 | 11.2, 11.3, 11.4, 11.5 |
| C3 | Deny-by-default segmentation with a factory jump host for all access to VLAN 30 | R3, R7, R10 | PR.IR-01 | 12.2, 13.4 |
| C4 | Patch and vulnerability management for servers, PCs, network devices, cameras, and the website | R4, R8, R9 | PR.PS-02, ID.RA-01 | 7.1–7.4 |
| C5 | Centralized logging (SIEM) with firewall IPS alerts | R2, R3, R5, R8 | DE.CM-01, DE.AE-03 | 8.2, 8.9, 13.1, 13.3 |
| C6 | Security awareness training with regular phishing simulations | R1, R2 | PR.AT-01 | 14.1, 14.2 |
| C7 | Least-privilege file permissions and prompt account removal for leavers | R5, R2 | PR.AA-05 | 5.3, 6.1, 6.2, 6.8 |
| C8 | Endpoint detection and response (EDR) on all PCs and servers | R1, R3 | DE.CM-09 | 10.1, 10.7 |

### Control Justifications

**C1 — Multi-factor authentication.** Phishing is the most likely way an attacker gets in. MFA means a stolen password alone is not enough to log in to the VPN, email, or cloud services. Admin accounts are the highest-value target, so they need MFA most.

**C2 — Immutable off-site backups.** Modern ransomware looks for backups and destroys them before encrypting files. Immutable backups cannot be changed or deleted for a set period, even by an administrator account, so the company can always restore. Monthly restore tests confirm the backups actually work.

**C3 — Segmentation and factory jump host.** Our design already separates the network into seven VLANs with the firewall as the gateway. This control adds a **deny-by-default** rule set and requires all access to factory systems, including contractor access, to go through a single monitored jump host. This follows the zones-and-conduits approach in IEC 62443 for industrial systems.

**C4 — Patch and vulnerability management.** In Tutorial 8, our research on recent Cisco and Chrome CVEs showed how quickly network devices and browsers become vulnerable. A monthly patch cycle, with critical fixes applied faster, reduces the window for attackers. Cameras and the website CMS are included because they are often forgotten.

**C5 — Centralized logging and monitoring.** The firewall sees all traffic between VLANs, but alerts are only useful if someone reviews them. A SIEM collects logs from the firewall, Active Directory, servers, and EDR, and correlates them to detect patterns like a login from an unusual location followed by a large file download.

**C6 — Security awareness training.** Technical controls cannot stop every phishing email. Yearly training with regular phishing simulations helps employees recognize and report suspicious messages.

**C7 — Least privilege and account management.** Only engineers should access engineering designs, and only finance staff should access financial records. Accounts must be disabled the day an employee leaves, which reduces insider risk.

**C8 — Endpoint detection and response.** EDR can detect and stop ransomware behavior, such as mass file encryption, on individual computers before it spreads.

### Residual Risk

| Risk ID | Original Score | Controls Applied | Residual L | Residual I | Residual Score | Residual Level |
|---|---|---|---|---|---|---|
| R1 | 20 | C1, C2, C6, C8 | 2 | 3 | 6 | Medium |
| R2 | 16 | C1, C5, C6, C7 | 2 | 3 | 6 | Medium |
| R3 | 15 | C3, C5, C8 | 2 | 4 | 8 | Medium |
| R4 | 15 | C4, C5 | 2 | 4 | 8 | Medium |
| R5 | 15 | C2, C5, C7 | 2 | 4 | 8 | Medium |
| R6 | 12 | UPS (already in design) | 2 | 4 | 8 | Medium |

R6 is mainly an availability risk. The UPS in our design covers short power outages. A second internet connection and redundant firewall would reduce it further but add cost, which is listed as a future recommendation.

---

## 6. Changes to the Network Design

Several controls change the network architecture. These updates should be made to the network design and diagram.

| Change | Control | Location | Details |
|---|---|---|---|
| Add a log server / SIEM | C5 | VLAN 50 — 10.10.50.40 | Receives logs from the firewall, AD, servers, and EDR |
| Add a factory jump host | C3, C7 | VLAN 10 — 10.10.10.50 | The only device allowed to connect to PLCs and HMIs; contractors reach it through the VPN with MFA |
| Change firewall policy to deny-by-default | C3 | Firewall | Only traffic listed in the traffic rules table is allowed between VLANs |
| Enable immutable storage on the cloud backup server | C2 | Cloud backup server | Backups cannot be changed or deleted for 30 days |
| Add a web application firewall (WAF) | C4 | In front of the cloud web server | Blocks common web attacks while the CMS is being patched |
| Require MFA on the site-to-site and remote-access VPN | C1 | Firewall | Applies to staff, administrators, and contractors |

---

## 7. Open Items

- [ ] Enter the risk register into the course risk assessment spreadsheet template and link it here.
- [ ] Group review of likelihood and impact scores.
- [ ] Update the network diagram with the log server and jump host.
- [ ] Add the WAF and immutable backup costs once the cloud provider is chosen.

---

## References

- National Institute of Standards and Technology. (2024). *The NIST Cybersecurity Framework (CSF) 2.0*. https://www.nist.gov/cyberframework
- National Institute of Standards and Technology. (2012). *SP 800-30 Rev. 1: Guide for Conducting Risk Assessments*. https://csrc.nist.gov/pubs/sp/800/30/r1/final
- Center for Internet Security. *CIS Critical Security Controls v8*. https://www.cisecurity.org/controls
- International Society of Automation. *ISA/IEC 62443 Series of Standards*. https://www.isa.org/standards-and-publications/isa-standards/isa-iec-62443-series-of-standards
- Anthropic. *Claude* (AI assistant), used to help draft and structure this section.
