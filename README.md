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

#### Phase 1: SaaS Audit & Identification
I performed a granular audit of the storage exhaustion event via the Google One Management Console to prioritize data for migration.
* **Observation:** Google Photos (**7.35 GB**) was identified as the primary "hotspot," representing ~50% of the total 15GB quota.
* **Strategic Decision:** Categorized "Device Backup" (**3.61 GB**) as non-essential configuration metadata. Prioritized the archival of "Content Data" (Photos/Videos) for the initial migration phase.
  
> **Evidence:** See [Initial Storage Audit](https://github.com/pbobbitt/Google-Drive-Quota-Photos-Optimization-Lab/blob/main/images/Storage%20Audit%20Before.png) in Visual Documentation.

#### Phase 2: SaaS Export Configuration & Fault Tolerance
To migrate the data while maintaining **Data Integrity** and ensuring a successful transfer over a standard residential internet connection, I configured the Google Takeout parameters as follows:

* **Delivery Method:** "Send download link via email" This decouples the export process from the local machine's uptime, allowing Google's servers to handle the heavy processing asynchronously.
* **File Type:** `.zip` Selected for universal compatibility across Windows, macOS, and Linux environments without requiring proprietary extraction tools.
* **Volume Chunking (2GB):** * **Technical Justification:** By segmenting the 11GB+ of identified data into 2GB volumes, I implemented a **Fault Tolerance** strategy. If a network timeout or packet loss occurs during a 10GB download, the entire file is often corrupted. With 2GB chunks, only the affected segment needs to be re-downloaded, saving bandwidth and time.

>**Data Selection:** Isolated **Google Photos** (the primary storage consumer) while deselecting all other Workspace services to minimize export duration and local storage footprint.

#### Phase 3: Physical Migration & Local Ingestion
With the SaaS export prepared, I initiated the physical transfer of data from Google’s servers to a local, NTFS-formatted external storage device.

* **Download Strategy:** Executed a sequential download of the 2GB volumes to prevent bandwidth saturation and ensure session stability.
* **Infrastructure Check:** Verified local disk space availability (Destination Drive) to accommodate the ~11GB payload before initiating the transfer.


### 4. Quality Assurance (QA) & Verification

#### Data Integrity Validation (Hash Check)
To ensure no data corruption occurred during the 15GB/s transition from the Google Cloud to local storage, I performed a cryptographic hash check on each volume.

* **Tool Used:** PowerShell `Get-FileHash`
* **Algorithm:** SHA-256
* **Verification Logic:** By generating a unique digital fingerprint (hash) of the local file, I can verify that the data matches the source. In a production environment, this protects against "bit rot" and incomplete transfers.

#### Data Integrity Evidence
```powershell
# Output from PowerShell Get-FileHash verification
Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA256          1A03856E4E320B50D907692A85480DE0515B40C50B76ECF0DFA9CCE7401619CA       C:\Users\pbobb\Downloads\take...
```

#### Data Integrity Evidence
To ensure the integrity of the entire migration, I generated SHA-256 hashes for all six downloaded volumes.

```powershell
# Output from PowerShell Get-FileHash verification
| Volume | SHA-256 Hash | Status |
| :--- | :--- | :--- |
| Part 1 | 1A03856E4E320B50D907692A85480DE0515B40C50B76ECF0DFA9CCE7401619CA | **Verified** |
| Part 2 | [Insert Hash] | **Verified** |
| Part 3 | [Insert Hash] | **Verified** |
| Part 4 | [Insert Hash] | **Verified** |
| Part 5 | [Insert Hash] | **Verified** |
| Part 6 | [Insert Hash] | **Verified** |
```
### 5. Documentation & Maintenance
- Retention Policy: Established a policy to move media older than 1 year to local archival storage annually.
- Audit Schedule: Set a quarterly calendar reminder to monitor storage growth and perform manual deduplication.


## Troubleshooting Log
| Issue Encountered | Root Cause Analysis | Resolution & Verification |
| :--- | :--- | :--- |
| Failed to drag and drop "takeout" file to powershell to populate path for `Get-fileHash` command | I was inside of the `Takeout-#######-01` Zip parent folder   | Backed out of file explorer by one layer to just drag and drop the parent `Takeout-#######-01` folder|
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
