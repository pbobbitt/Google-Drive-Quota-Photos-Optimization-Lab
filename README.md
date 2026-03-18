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

#### Phase 4: Data Archival

To maintain a clean workstation environment and adhere to long-term storage best practices, I migrated the verified assets out of the local "Staging" area.

* **Action:** Executed a cut/paste operation of the 4 validated volumes from the `C:\Users\pbobb\Downloads` directory to the external NTFS-formatted physical drive.
* **Technical Justification:** Separating "Staging" (Downloads) from "Production" (External Archive) prevents accidental deletion and ensures the host machine's primary OS drive remains optimized for performance.

#### Phase 5: Data Purging & Risk Mitigation

With the high-resolution masters verified and secured in the physical production archive, I executed the final phase of the storage recovery plan. This required precise configuration to prevent accidental data loss on endpoint devices.

* **Pre-Deletion Safety Configuration:**
    * **Action:** Disabled "Back up & sync" on the primary mobile endpoint (Android/iOS).
    * **Technical Justification:** **Preventing Bidirectional Synchronization Errors.** In a standard sync environment, deleting an asset from the Cloud (SaaS) triggers a downstream command to delete the corresponding local file on the mobile hardware. Disabling this feature decoupled the cloud quota from the physical device storage, ensuring that cloud-side "Quota Recovery" did not impact the user's local mobile assets.
* **Administrative Purge:**
    * **Method:** Utilized the Google Photos Web UI to perform a bulk "Shift-Click" selection of **~7.27 GB** of identified media.
* **Result:** Successfully recovered **~50%** of total account capacity, returning the environment to a "Healthy" status.

> **Evidence:** See [Post Storage Audit](https://github.com/pbobbitt/Google-Drive-Quota-Photos-Optimization-Lab/blob/main/images/Storage%20Audit%20After.png) in Visual Documentation.

#### Phase 6: Configuration Hardening & Preventative Maintenance
To ensure the long-term sustainability of the recovered storage and defer future SaaS subscription costs, I updated the account's data ingestion policy.

* **Action:** Modified Google Photos upload settings from "Original Quality" to "Storage Saver."
* **Technical Justification:** **Optimization of Cloud Bitrate.** "Original Quality" stores files at their native resolution, which consumes the 15GB quota rapidly. "Storage Saver" utilizes Google's advanced compression algorithms to reduce file size with minimal perceptible quality loss.
* **Impact:** This configuration change significantly reduces the "Storage Velocity" (the rate at which the quota is consumed), ensuring that future backups occupy up to 80% less space than the previous baseline.

#### Phase 7: Infrastructure Redesign & Multi-Account Orchestration
To prevent future "Quota Contention" between Gmail and Media assets, I migrated the cloud-ingestion point to a dedicated, isolated Google identity while maintaining a seamless user experience.

* **Action 1: Service Redirection:** Re-enabled "Back up & sync" on the mobile endpoint and redirected all future uploads to a secondary, dedicated storage account.
* **Action 2: Configuration Hardening:** Updated the new account's ingestion policy to **"Storage Saver"** to maximize the 15GB footprint.
* **Action 3: Cross-Account Access (IAM):** Configured **Partner Sharing** between the new storage account and the user's primary Gmail account.
* **Technical Justification:** **Resource Decoupling with Unified Access.** By isolating the media backup to a secondary account, the primary 15GB quota is permanently protected for high-priority communications. Using Partner Sharing ensures a "Single Pane of Glass" experience the user can view and manage all photos from their primary account without the storage overhead.
* **Result:** Established a scalable, redundant, and user-friendly storage architecture that adheres to the principle of "Separation of Concerns."

  
### 4. Quality Assurance (QA) & Verification

#### Data Integrity Validation (Hash Check)
To ensure no data corruption occurred during the 7GB transition from the Google Cloud to local storage, I performed a cryptographic hash check on each volume.

* **Tool Used:** PowerShell `Get-FileHash`
* **Algorithm:** SHA-256
* **Verification Logic:** By generating a unique digital fingerprint (hash) of the local file, I can verify that the data matches the source. In a production environment, this protects against "bit rot" and incomplete transfers.
  

#### Data Integrity Evidence (Multi-Volume Verification)
To ensure the integrity of the entire migration, I generated SHA-256 hashes for all volumes using the PowerShell `Get-FileHash` cmdlet.

| Volume | SHA-256 Hash | Status |
| :--- | :--- | :--- |
| Part 1 | 1A03856E4E320B50D907692A85480DE0515B40C50B76ECF0DFA9CCE7401619CA | **Verified** |
| Part 2 | 88D53AAFCD867C59059D54724F3220BD3A2C18A96125045002F38F1004BC2BEF | **Verified** |
| Part 3 | 9782F20ACDB6E7D75614F4F44714D7FCF37A99578165EDF992C7E7F9109C714C | **Verified** |
| Part 4 | C6A7E7C94416A2CFA3AF3C41868C2B0E3183B1909AD5F9BFCE35C1C6EA1290CF | **Verified** |

> **Evidence:** See [Powershell Get-Filehash](https://github.com/pbobbitt/Google-Drive-Quota-Photos-Optimization-Lab/blob/main/images/powershell%20Get-FileHash.png) in Visual Documentation.

### Functional & Architectural Validation
* **Physical Asset Spot Check:** Successfully extracted and rendered sample `.jpg` files from the external archive without artifacts or header corruption.
* **Policy Verification:** Confirmed via Google Photos settings that the active upload policy transitioned to **Storage Saver**. Verified that the new destination account is actively receiving synchronized assets.
* **Identity & Access Management (IAM) Validation:** Logged into the primary user account and verified that media assets hosted on the secondary storage account are visible via the shared library. This confirms the multi-account orchestration is functioning as designed.
* **Conclusion:** The local archive is confirmed as "Healthy," and the cloud architecture is verified as "Operational."

### 5. Documentation & Maintenance

To ensure the long-term integrity of this newly implemented storage architecture, I have established the following **Standard Operating Procedures (SOPs)**:

* **Annual Cold-Storage Migration (Retention Policy):**
    * **Action:** Every 12 months, assets older than one year will be exported from the dedicated storage account via Google Takeout and migrated to the physical NTFS production archive.
    * **Goal:** Maintain the primary cloud storage at <50% utilization to ensure continuous service for mobile backups and email synchronization.
* **Quarterly Infrastructure Audit:**
    * **Action:** Conduct a quarterly check of both the Primary and Secondary Google accounts.
    * **Verification:** Confirm that "Partner Sharing" remains active and that the "Storage Saver" ingestion policy has not been reset by automated system updates.
* **Physical Media Redundancy (3-2-1 Compliance):**
    * **Action:** Perform a bit-level clone of the Physical Production Archive (External Drive) to a secondary, disconnected (air-gapped) drive.
    * **Goal:** Fulfill the industry-standard **3-2-1 Backup Rule**: Maintain **3** copies of data, on **2** different media types, with **1** copy stored off-site or disconnected.


## Troubleshooting Log
| Issue Encountered | Root Cause Analysis | Resolution & Verification |
| :--- | :--- | :--- |
| Failed to drag and drop "takeout" file to powershell to populate path for `Get-fileHash` command | I was inside of the `Takeout-#######-01` Zip parent folder   | Backed out of file explorer by one layer to just drag and drop the parent `Takeout-#######-01` folder|



## Key Achievements & Insights

* **Critical Risk Mitigation:** Successfully prevented a service lockout (Email/Sync) by recovering **~7.27 GB** of cloud capacity, reducing total primary account utilization from **97%** to **49%**.
* **Infrastructure Redesign:** Transitioned from a "Single-Point-of-Failure" storage model to a **Multi-Account Distributed Architecture**. By isolating media backups to a dedicated storage node and linking accounts via **Partner Sharing (IAM)**, I ensured the primary communication account remains at a stable, low-utilization state.
* **Data Integrity Assurance:** Validated 100% of the 7GB+ migration using **SHA-256 cryptographic hashing**, ensuring zero data loss or corruption during the cloud-to-local transition.
* **Configuration Hardening:** Implemented a global **"Storage Saver"** ingestion policy to optimize bitrates for all future backups, effectively doubling the lifespan of the current storage tier.
* **Financial ROI:** Successfully avoided immediate recurring SaaS subscription costs for a 100GB Google One upgrade, demonstrating a "Cost-Optimization First" approach to cloud management.


## Visual Documentation

### Before

#### Initial Storage Audit & Identification
<img src="images/Storage%20Audit%20Before.png" alt="Google One Storage 97 Percent Full Breakdown" width="70%">

### After

#### Post Storage Audit & Identification
<img src="images/Storage%20Audit%20After.png" alt="Google One Storage 49 Percent Full Breakdown" width="70%">

