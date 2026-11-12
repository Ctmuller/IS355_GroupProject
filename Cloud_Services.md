# Cloud Services (v0.1)

**IS355 Networking and Cyber Security — Colin Muller, Collin Morris, Kairi Thomas, Mohammad Abukhdeir**

> **Status: NEARLY COMPLETE.** AWS and Azure pricing, the provider recommendation, and the backup strategy comparison are complete. Still needed: ( the Azure public estimate link (linked to excel and picture) (Section 1.3) and each member's backup approach (Section 3).

## 1. Pricing for Cloud Services

The company requires two cloud-hosted servers:

1. **Web server** — Linux, hosting the public website with company information and product details (specifications and pictures).
2. **Backup server** — storing encrypted backups of important company files, such as technical designs, high-resolution product images, and technical specifications. Files are encrypted on-site before being sent to the cloud.

Both servers must be hosted in the USA. Pricing was estimated using two providers: **Amazon Web Services (AWS)** and **Microsoft Azure**.

### 1.1 Selected Specifications

The same or equivalent specifications were selected for both providers so the comparison is fair.

#### Web Server

| Specification | AWS | Azure | Justification |
|---|---|---|---|
| Region | US East (N. Virginia) | East US | Meets the USA hosting requirement; East Coast region close to the company's customers |
| Operating system | Linux | Linux (Ubuntu) | Linux is required by the scenario and avoids Windows licensing costs |
| Instance | t3.medium (2 vCPU, 4 GiB RAM) | B2s (2 vCPU, 4 GiB RAM) | Sufficient for a company website with product pages and images under moderate traffic |
| Pricing model | On-Demand, 730 hours/month | Pay as you go, 730 hours/month | The website must be available 24/7; the same billing model is used on both providers |
| Storage | 64 GB General Purpose SSD (gp3) | 64 GiB Premium SSD (P6) | SSD storage keeps page and image loading fast |
| Outbound data transfer | 100 GB/month to Internet | 100 GB/month to Internet | Estimated visitor traffic, including downloads of high-resolution product images |

#### Backup Server

| Specification | AWS | Azure | Justification |
|---|---|---|---|
| Region | US East (N. Virginia) | East US | Meets the USA hosting requirement; same region as the web server for simpler management |
| Operating system | Linux | Linux (Ubuntu) | Any OS is allowed; Linux is lower cost, and files arrive already encrypted |
| Instance | t3.medium (2 vCPU, 4 GiB RAM) | B2s (2 vCPU, 4 GiB RAM) | The server mainly receives and stores files, so it needs little compute power |
| Pricing model | On-Demand, 730 hours/month | Pay as you go, 730 hours/month | Available whenever scheduled backups run |
| Storage | 2,048 GB Throughput Optimized HDD (st1) | 2,048 GiB Standard HDD (S40) | Large, low-cost storage for CAD designs and high-resolution images, with room for multiple backup versions |
| Outbound data transfer | 10 GB/month to Internet | 10 GB/month to Internet | Used only for occasional file restores; inbound backup traffic is free on both providers |

### 1.2 Annual Cost by Provider

| Server | AWS Monthly | AWS Annual | Azure Monthly | Azure Annual |
|---|---|---|---|---|
| Web Server | $44.49 | $533.86 | $40.58 | $486.90 |
| Backup Server | $123.43 | $1,481.14 | $108.19 | $1,298.30 |
| **Total** | **$167.92** | **$2,015.04** | **$148.77** | **$1,785.20** |

*AWS figures are taken from the AWS Pricing Calculator export. Azure figures are taken from the Azure Pricing Calculator export (`Azure_Estimate.xlsx`). The total annual figure is reported by the calculator based on the rounded monthly total.*

#### AWS Cost Breakdown

| Server | Compute | Storage | Data Transfer | Monthly Total |
|---|---|---|---|---|
| Web Server | $30.37 (t3.medium at $0.0416/hr) | $5.12 (64 GB gp3) | $9.00 (100 GB) | $44.49 |
| Backup Server | $30.37 (t3.medium at $0.0416/hr) | $92.16 (2,048 GB st1) | $0.90 (10 GB) | $123.43 |

#### Azure Cost Breakdown

| Server | Compute | Storage | Data Transfer | Monthly Total |
|---|---|---|---|---|
| Web Server| $30.37 (B2ls v2 at $0.0416/hr)| $10.21 (64 GiB P6 Premium SSD) | $0.00 (100 GB free tier) | $40.58 |
| Backup Server| $30.37 (B2ls v2 at $0.0416/hr)| $77.82 (2,048 GiB S40 Standard HDD) | $0.00 (10 GB free tier) | $108.19 |

**Note on pricing model:** On-Demand pricing was used for a fair comparison. The calculator shows that a 1-year Compute Savings Plan would reduce the t3.medium compute cost from $30.37 to $15.04 per month per server, so the company could lower costs by committing to a longer term.

### 1.3 Evidence of Cloud Pricing Calculators

- AWS estimate export: [AWS_Estimate.csv](diagrams/AWS_Estimate.csv)
- AWS estimate summary screenshot: [AWS_Estimate_SC](diagrams/AWS_Estimate.png)
- AWS public estimate link: [View AWS estimate](https://calculator.aws/#/estimate?id=1881fd93680bab847ba45aead7e49db160b8f34d)
- Azure estimate export: [Azure_Estimate.xlsx](diagrams/Azure_Estimate.xlsx)
- Azure estimate summary screenshot: [Azure_Estimate.png](diagrams/Azure_Estimate.png)
- Azure public estimate link: Not generated (Full breakdown provided in `Azure_Estimate.xlsx` and `Azure_Estimate.png`)

### 1.4 Recommended Provider

**Recommendation: Microsoft Azure.**

With equivalent specifications, Azure costs **$1,785.20 per year** compared with **$2,015.04 per year** for AWS, a saving of **$229.84 per year (about 11%)**. Compute cost is identical on both providers ($30.37 per server per month), so the difference comes from two areas:

1. **Backup storage.** Azure's 2 TiB Standard HDD costs $77.82 per month versus $92.16 for AWS st1, a difference of $14.34 per month. Because the backup server is the larger of the two costs and its storage will grow as the company keeps more backup versions, this is the gap most likely to widen over time.
2. **Outbound data transfer.** Azure includes the first 100 GB of outbound transfer each month at no charge, while AWS charges $0.09/GB. This saves about $9.90 per month at the estimated traffic level.

AWS is slightly cheaper for the web server's SSD ($5.12 vs. $10.21), but this does not offset Azure's savings on storage and bandwidth. Both providers offer US-based regions, Linux support, encryption at rest, and similar long-term commitment discounts, so neither has a technical advantage that outweighs the cost difference for this company's needs. Azure is therefore the better value.

**Considerations:** If the website's traffic grows well beyond 100 GB/month, Azure's free bandwidth advantage shrinks, so the company should review its usage after the first year. The company could also reduce costs on either provider by storing backups in object storage (Amazon S3 or Azure Blob Storage, using a cool or archive tier) instead of a disk attached to a running server, since backup data is written often but read rarely.

## 2. Backup Strategy Comparison

The company's important files (technical designs, high-resolution product images, and technical specifications) could be protected in three ways:

1. **Local backup only** — copies kept on drives or a server at the company's own site.
2. **Encrypted cloud backup** — files encrypted on-site, then sent to a cloud server (the approach in Section 1).
3. **Unencrypted cloud backup** — files sent to a cloud server without being encrypted first.

| Factor | Local Backup Only | Encrypted Cloud Backup | Unencrypted Cloud Backup |
|---|---|---|---|
| Fire, flood, or other site disaster | Not protected — backups are lost along with the originals | Protected — copies are stored off-site | Protected — copies are stored off-site |
| Theft of on-site equipment | Not protected, and stolen backup drives expose the data | Protected — data is safe off-site and unreadable to others | Protected from loss, but data remains readable if the cloud account is breached |
| Ransomware | High risk if backup drives are connected to the infected network | Lower risk, especially with versioning and immutable backups | Lower risk of loss, but no protection for confidentiality |
| Breach of the cloud provider or account | Not applicable | Data stays confidential because only the company holds the key | Designs and specifications are exposed to the attacker |
| Confidentiality of intellectual property | Depends on physical security of the site | Strongest — the provider never sees readable data | Weakest — relies entirely on the provider's security |
| Restore speed | Fastest — data is on the local network | Slower — limited by internet speed, plus decryption | Slower — limited by internet speed |
| Cost | Up-front hardware cost, no monthly fees | Monthly fees (about $1,300–$1,480/year for the backup server) plus key management | Same monthly fees as encrypted cloud backup |
| Main risk | A single event can destroy both originals and backups | Losing the encryption key makes backups unrecoverable | Data exposure if the account or provider is compromised |

**Local backup only** is fast and inexpensive to run, but it fails the most important purpose of a backup: surviving an event that affects the main site. A fire, flood, theft, or ransomware attack can destroy the original files and the backups at the same time.

**Unencrypted cloud backup** solves the off-site problem, but the company's technical designs and specifications are valuable intellectual property. Storing them in readable form means a stolen cloud password, a misconfigured storage permission, or a breach at the provider would expose them to competitors or attackers.

**Encrypted cloud backup** keeps copies off-site and keeps them confidential, because files are encrypted before they leave the company and only the company holds the key. Its main weakness is key management: if the key is lost, the backups cannot be restored, so the key must be stored securely in more than one place (for example, a password manager and an offline copy kept in a safe).

**Conclusion:** Encrypted cloud backup is the strongest of the three options for this company. Ideally, it should be combined with a local backup to follow the **3-2-1 rule**: three copies of the data, on two different types of media, with one copy off-site. The local copy provides fast restores for everyday mistakes, while the encrypted cloud copy protects against disasters, theft, and breaches.

## 3. Group Members' University Work Backup Approaches

*Each member should replace the placeholder text with their own approach: the specific tools or services used, how often backups happen, and how they would recover their work if their primary computer were stolen.*

### Colin Muller

**Tools/services:** GitHub (private repositories), Microsoft OneDrive through Loyola's Microsoft 365 account, and built-in device encryption (BitLocker on Windows / FileVault on macOS).

**How it works:** Coursework that involves code, documentation, or group projects, such as this IS355 project and my weekly tutorial journal, is kept in private GitHub repositories. Every commit and push creates a versioned copy off my computer, so I can restore any earlier version of a file. Other university files, such as Word documents, Excel models, and presentations, are saved in a OneDrive folder that syncs automatically whenever I'm online. OneDrive also keeps version history and a recycle bin, which protects against accidental deletion or overwriting. Together this follows the 3-2-1 idea: the copy on my laptop, plus two separate cloud services in different locations.

**If my computer were stolen:** I would lose the hardware, not my work. I could sign in to GitHub and OneDrive from any other computer, clone my repositories, and continue working with at most the changes since my last sync or push. Both accounts are protected with strong passwords and multi-factor authentication, so the thief could not get into them, and I would sign out of all sessions and change my passwords right away. Because the laptop's drive is encrypted and requires a password to unlock, the thief could not read the files stored on it.

### Collin Morris

**Tools/services:** GitHub (private repositories), Google Drive, and a local USB flash drive (plus Windows BitLocker drive encryption).

**How it works:** All the lab work done by me including my coding labs, network diagrams, and other documents of the group projects are stored in private GitHub repositories for version control in order to make sure that everything is being saved properly. Classnotes, assignments, and PowerPoint slides are uploaded to Google Drive and they automatically get uploaded to the cloud if I’m connected to the Internet. Major assignments are also stored on my USB at home.

**If my computer were stolen:** I would only lose the physical laptop and nothing to do with my coursework. I would still be able to login to my GitHub account and Google Drive account from another computer and continue working there since both accounts are protected using strong passwords and two-factor authentication, also I would immediately logout of other logged-in sessions and change my passwords. Since the hard drive on the laptop is encrypted, anyone who got the laptop will not be able to access the files stored on it.

### Kairi Thomas

**Tools/services:** [Google Drive, OneDrive, GitHub]

**How it works:** 

**Google Drive**: is what I use for my google docs, slides, and photos that are related to assignments. 

**OneDrive**: is the what I use for other assignments like Microsoft word, Microsoft Excel, and Microsoft presentation. This program is very useful for me because I work on a windows pc so I can easily access any of the Microsoft files I just describe

**GitHub**: is a platform that is cloud based, this platform allows for the making of repositories. These repositories are used to store project files. The neat part about GitHub is the collaboration that the platform allows, this means that I can work on a file with other students. GitHub is also great for saving data, I can restore previous versions of the file if I am not comfortable with the current form of the file.  

**If my computer were stolen:** [ ]

### Mohammad Abukhdeir

**Tools/services:** [ ]

**How it works:** [ ]

**If my computer were stolen:** [ ]

## References

- Amazon Web Services. *AWS Pricing Calculator*. https://calculator.aws
- Microsoft. *Azure Pricing Calculator*. https://azure.microsoft.com/pricing/calculator
- Anthropic. *Claude* (AI assistant), used to help select comparable specifications, structure this section, and draft the provider recommendation and backup strategy comparison.
