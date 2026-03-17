# Google Drive Quota & Photos Optimization Lab

## Executive Summary
This project demonstrates migrating critical data from a near-capacity SaaS environment to local physical storage. It highlights the ability to manage storage quotas, ensure data integrity during transit, and implement archival policies to maintain service continuity.

* **Primary Skill:** Cloud Data Migration
* **Certification Alignment:** CompTIA A+, Cloud+

## The Scenario
A professional Google Workspace account reached 97% storage utilization, creating an imminent risk of service interruption (inability to receive emails or sync documents). A solution was required to audit the data, archive "cold" assets to on-premise hardware, and optimize remaining cloud storage without data loss.


## Environment & Tools
* **Platform/OS:** Google Workspace / Windows 11
* **Tools Used:** Google Takeout, Google One Storage Manager, PowerShell
* **Methodology:** 3-2-1 Backup Strategy, Data Lifecycle Management

## Deployment & Execution

### 1. Requirements & Scope
* Goal: Reduce cloud storage utilization to below 20%.

* Constraint: Ensure zero data loss during migration and maintain original file metadata.

* Scope: 15GB of unstructured data (Photos, Videos, and Drive documents).

### 2. Design & Strategy
Before execution, I conducted a storage audit to categorize data. I decided to use Google Takeout to export all high-utilization categories. To ensure download stability and prevent file corruption, I configured the export to use 2GB .zip volumes.


### 3. Implementation
1. SaaS Audit: Utilized Google One Storage Manager to identify the largest files for removal.
2. Data Export: Initiated a Google Takeout request for Drive and Photos.
3. Physical Transfer: Downloaded the multi-part archive directly to a local NTFS-formatted external drive.
4. Cloud Optimization: Executed the "Recover Storage" tool to compress remaining cloud media and purged files confirmed as backed up.


### 4. Quality Assurance (QA) & Verification
- Hash Verification: Executed PowerShell Get-FileHash on downloaded volumes to ensure file integrity.
- Data Parity: Compared local folder properties (size and file count) against the Google Takeout manifest.
- Service Restoration: Verified the Google One dashboard reflected a "Healthy" status (<15% used) post-purge.

### 5. Documentation & Maintenance
- Retention Policy: Established a policy to move media older than 1 year to local archival storage annually.
- Audit Schedule: Set a quarterly calendar reminder to monitor storage growth and perform manual deduplication.


## Troubleshooting Log
| Issue Encountered | Root Cause Analysis | Resolution & Verification |
| :--- | :--- | :--- |
| Service "WebSrv" failed to start. | Configuration syntax error in `nginx.conf`. | Corrected syntax; ran `nginx -t` to verify; restarted service. |
| Remote users unable to access VPN. | Security Group ingress rules blocked Port 443. | Updated AWS Security Group; verified with successful handshake. |
| [New Issue] | [Why it broke] | [How it was fixed & verified] |

> **Note:** For labs involving physical disassembly or hardware repair, ensure all components are documented and screws are organized/mapped to ensure proper reassembly.


## Key Achievements & Insights

- **Achievement**: Successfully recovered 85% of available cloud storage, preventing service lockout.

- **Insight**: Identified that "Original Quality" media uploads were the primary driver of storage exhaustion; adjusted sync settings to "Storage Saver" to mitigate future growth.

- **Metric**: Avoided immediate recurring costs associated with upgrading to the next SaaS storage tier.


## Visual Documentation

### Before

#### Initial Storage Audit & Identification
<img src="images/Storage%20Audit%20Before.png" alt="Google One Storage 97 Percent Full Breakdown" width="70%">
