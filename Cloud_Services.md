# Cloud Services (v0.1)

**IS355 Networking and Cyber Security — Colin Muller, Collin Morris, Kairi Thomas, Mohammad Abukhdeir**

> **Status: IN PROGRESS.** AWS pricing is complete. Azure pricing, the provider recommendation, and the backup strategy comparison are still to be completed.

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
| Web Server | $30.37 (B2ls v2 at $0.0416/hr) | $10.21 (64 GiB P6 Premium SSD) | $0.00 (100 GB free tier) | $40.58 |
| Backup Server | $30.37 (B2ls v2 at $0.0416/hr) | $77.82 (2,048 GiB S40 Standard HDD) | $0.00 (10 GB free tier) | $108.19 |

**Note on pricing model:** On-Demand pricing was used for a fair comparison. The calculator shows that a 1-year Compute Savings Plan would reduce the t3.medium compute cost from $30.37 to $15.04 per month per server, so the company could lower costs by committing to a longer term.

### 1.3 Evidence of Cloud Pricing Calculators

- AWS estimate export: [AWS_Estimate.csv](diagrams/AWS_Estimate.csv)
- AWS estimate summary screenshot: [AWS_Estimate_SC](diagrams/AWS_Estimate.png)
- AWS public estimate link: [View AWS estimate](https://calculator.aws/#/estimate?id=1881fd93680bab847ba45aead7e49db160b8f34d)
- Azure estimate export: [Azure_Estimate.xlsx](diagrams/Azure_Estimate.xlsx)
- Azure estimate summary screenshot: [Azure_Estimate.png](diagrams/Azure_Estimate.png)
- Azure public estimate link: TBD

### 1.4 Recommended Provider

TBD — to be completed after Azure pricing.

## 2. Backup Strategy Comparison

TBD — compare local backup only, encrypted cloud backup, and unencrypted cloud backup.

## 3. Group Members' University Work Backup Approaches

TBD — each group member to describe the specific tools or services they use (or intend to use) to back up university work, and how their approach would help if their primary computer were stolen.

## References

- Amazon Web Services. *AWS Pricing Calculator*. https://calculator.aws
- Microsoft. *Azure Pricing Calculator*. https://azure.microsoft.com/pricing/calculator
- Anthropic. *Claude* (AI assistant), used to help select comparable specifications and structure this section.
