# Google Drive Quota & Photos Optimization Lab

## Executive Summary
This project demonstrates migrating critical data from a near-capacity SaaS environment to local physical storage and redesigning cloud architecture to prevent future quota exhaustion. It highlights the ability to manage storage quotas, ensure data integrity, and implement multi-account orchestration (IAM) to maintain service continuity.

* **Primary Skill:** Cloud Data Migration, Identity & Access Management (IAM)
* **Certification Alignment:** CompTIA A+, Cloud+

## The Scenario
A professional Google Workspace account reached 97% storage utilization, creating an imminent risk of service interruption (inability to receive emails or sync documents). A solution was required to audit the data, archive "cold" assets to on-premise hardware, and restructure the cloud ingestion workflow to optimize remaining storage without data loss or service interruption.


## Environment & Tools
* **Platform/OS:** Google Workspace / Windows 11
* **Tools Used:** Google Takeout, Google One Storage Manager, PowerShell, Google Photos Partner Sharing (IAM).
* **Methodology:** 3-2-1 Backup Strategy, Data Lifecycle Management, Resource Isolation.

## Deployment & Execution

### 1. Requirements & Scope
* Goal: Reduce cloud storage utilization and establish a sustainable long-term backup architecture.

* Constraint: Ensure zero data loss during migration and maintain original file metadata.

* Scope: 15GB of unstructured data (Photos, Videos, and Drive documents) and multi-account synchronization logic.

### 2. Design & Strategy
Before execution, I conducted a storage audit to categorize data by utilization and retention value. I developed a two-pronged remediation strategy to address both the immediate service risk and long-term sustainability:

* **Immediate Remediation (Data Migration):** Utilize **Google Takeout** to export high-utilization "hotspots" into 2GB fault-tolerant volumes. This ensures a bit-perfect transfer to local physical storage while minimizing the risk of archive corruption over standard residential network connections.
* **Architectural Pivot (Service Isolation):** Transition future data ingestion to a dedicated, isolated storage service account. By utilizing **Partner Sharing (IAM)**, the primary account maintains a "Single Pane of Glass" user experience (viewing all assets in one library) while completely isolating the primary 15GB quota from future media growth.

### 3. Implementation Summary
The remediation was executed through a methodical 7-phase "Audit-to-Architecture" workflow. For the granular technical procedures, PowerShell commands, and configuration steps associated with each phase, refer to the **[Full Implementation Log](TECHNICAL_IMPLEMENTATION.md)**.

| Phase | Milestone | Key Technical Action |
| :--- | :--- | :--- |
| **Phase 1** | SaaS Audit | Performed storage hotspot identification (7.35 GB identified). |
| **Phase 2** | Export Configuration | Established 2GB fault-tolerant volume chunking via Google Takeout. |
| **Phase 3** | Data Ingestion | Migrated verified 11GB+ payload to NTFS-formatted physical storage. |
| **Phase 4** | Asset Archival | Transitioned data from "Staging" (Downloads) to "Production" (Archive). |
| **Phase 5** | Quota Recovery | Mitigated bidirectional sync risks and purged ~50% cloud capacity. |
| **Phase 6** | Policy Hardening | Updated bitrate to "Storage Saver" to reduce future storage velocity. |
| **Phase 7** | Infrastructure Pivot | Redirected mobile backups to a dedicated service account via IAM. |
  
### 4. Quality Assurance (QA) & Verification

Data Integrity Validation (Hash Check)
To ensure no data corruption occurred during the 7GB transition from the Google Cloud to local storage, I performed a cryptographic hash check on each volume.

* **Tool Used:** PowerShell `Get-FileHash`
* **Algorithm:** SHA-256
* **Verification Logic:** By generating a unique digital fingerprint (hash) of the local file, I can verify that the data matches the source. In a production environment, this protects against "bit rot" and incomplete transfers.
  

Data Integrity Evidence (Multi-Volume Verification)
To ensure the integrity of the entire migration, I generated SHA-256 hashes for all volumes using the PowerShell `Get-FileHash` cmdlet.

| Volume | SHA-256 Hash | Status |
| :--- | :--- | :--- |
| Part 1 | 1A03856E4E320B50D907692A85480DE0515B40C50B76ECF0DFA9CCE7401619CA | **Verified** |
| Part 2 | 88D53AAFCD867C59059D54724F3220BD3A2C18A96125045002F38F1004BC2BEF | **Verified** |
| Part 3 | 9782F20ACDB6E7D75614F4F44714D7FCF37A99578165EDF992C7E7F9109C714C | **Verified** |
| Part 4 | C6A7E7C94416A2CFA3AF3C41868C2B0E3183B1909AD5F9BFCE35C1C6EA1290CF | **Verified** |

> **Evidence:** See [Powershell Get-Filehash](https://github.com/pbobbitt/Google-Drive-Quota-Photos-Optimization-Lab/blob/main/images/powershell%20Get-FileHash.png) in Visual Documentation.

Functional & Architectural Validation
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

## Technical Competencies Demonstrated

* **SaaS Administration:** Expertly managed Google Workspace quotas and service continuity for a high-utilization environment (97% saturation), preventing a critical "Service Refusal" state.
* **Data Lifecycle Management (DLM):** Executed a multi-tier storage strategy, transitioning "Hot" data (active Cloud assets) to "Cold" storage (Physical NTFS Archive) to optimize infrastructure costs and performance.
* **Information Security:** Implemented rigorous data integrity protocols using **SHA-256 Cryptographic Hashing** to verify bit-perfect transfers and mitigate the risk of "bit rot" during migration.
* **Identity & Access Management (IAM):** Orchestrated cross-tenant resource sharing via **Partner Linking**, successfully decoupling physical storage from a primary identity while maintaining a seamless, unified user experience.

## Visual Documentation

Before

Initial Storage Audit & Identification
<img src="images/Storage%20Audit%20Before.png" alt="Google One Storage 97 Percent Full Breakdown" width="70%">

After

Post Storage Audit & Identification
<img src="images/Storage%20Audit%20After.png" alt="Google One Storage 49 Percent Full Breakdown" width="70%">

Powershell Hash Command
<img src="images/powershell%20Get-FileHash.png" alt="Powershell Command" width="70%">


## Disclaimer & AI Disclosure
While the **technical architecture, troubleshooting, data migration, and verification** of this lab were performed entirely by the author, **Generative AI** was utilized as a collaborative tool to assist in the structured formatting, professional terminology refinement, and documentation of this report. 

This approach was taken to ensure the lab's technical findings are communicated with the clarity and professional standards required in a production IT environment.
