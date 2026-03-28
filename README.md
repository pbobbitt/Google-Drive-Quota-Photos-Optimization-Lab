# Cloud Account Quota Management & Data Archival

**Professional & Educational Credentials**  
<p align="left">
  <a href="https://linkedin.com/in/patrick-bobbitt"><img src="https://img.shields.io/badge/LinkedIn-Patrick%20Bobbitt-0072b1?style=for-the-badge&logo=linkedin&logoColor=white" /></a> <img src="https://img.shields.io/badge/Education-WGU%20BS%20Cloud%20%26%20Network%20Engineering%20Student-00ADEE?style=for-the-badge&logo=google-scholar&logoColor=white" /> <a href="https://github.com/pbobbitt/pbobbitt/blob/main/CompTIA%20A%2B%20ce%20certificate.pdf"><img src="https://img.shields.io/badge/Certification-CompTIA%20A%2B-E31837?style=for-the-badge&logo=comptia&logoColor=white" /></a>&nbsp;<a href="mailto:pbobbitt176@gmail.com"><img src="https://img.shields.io/badge/Email-Contact%20Me-red?style=for-the-badge&logo=gmail&logoColor=white" /></a>
</p>

**Lab Series Technical Stack**  
<p align="left">
  <img src="https://img.shields.io/badge/SaaS-Google%20Workspace-4285F4?style=for-the-badge&logo=google&logoColor=white" />
  <img src="https://img.shields.io/badge/Storage-NTFS%20Physical%20Archive-0078D4?style=for-the-badge&logo=windows&logoColor=white" />
  <img src="https://img.shields.io/badge/Tools-Google%20Takeout-FBBC05?style=for-the-badge&logo=googlecloud&logoColor=white" />
  <img src="https://img.shields.io/badge/Mobile-Endpoint%20Management-34A853?style=for-the-badge&logo=android&logoColor=white" />
</p>

# Project Overview
This project addressed a critical "Storage Full" event in a cloud environment. I audited the account to identify data hotspots, executed a fault-tolerant migration to local physical storage, and redesigned the account architecture to ensure long-term availability successfully **recovering 50% of the account capacity.**

## Tech Stack & Skills
*   **Systems:** Google Workspace Admin Tools, Google Takeout, Windows NTFS File Systems
*   **Networking & Security:** Data Integrity Verification, Fault Tolerance (Volume Chunking), IAM (Partner Sharing)
*   **Core Skills:** SaaS Audit & Reporting, Cloud-to-Local Migration, Endpoint Sync Management, Risk Mitigation

## Key Accomplishments

### Data Audit & Capacity Analysis
* Performed a granular audit of cloud storage to identify specific folders consuming the 15GB quota. Prioritized high-capacity media for migration while preserving essential account metadata.

### Fault-Tolerant Data Migration
* Implemented a **Volume Chunking** strategy (2GB segments) to migrate 11GB of data. This ensured that if the internet connection dropped, only a small segment was affected, preventing total file corruption and saving bandwidth.

### Endpoint Sync Management & Risk Mitigation
* Prevented accidental data loss by decoupling mobile devices from the cloud before the purge. By disabling bidirectional sync, I ensured that clearing cloud "quota" did not trigger an automatic command to delete the user's local phone photos.

### Infrastructure Redesign & Quota Hardening
* Deployed a "Separation of Concerns" architecture. I moved future media backups to a secondary storage identity and used **Partner Sharing** to give the user a "Single Pane of Glass" view. This permanently protects the primary email account from being blocked by photo storage.

## Final Validation & Project Completion
**The account was successfully returned to a "Healthy" status with 7.27 GB of recovered capacity. All archived data was verified for integrity on the local physical drive, and new automated policies are in place to prevent future storage overages.**

> **Full Technical Deep Dive**
> For specific configuration steps and the detailed implementation log, please see: **[Implementation Log (Detailed Version)](https://github.com/pbobbitt/Google-Drive-Quota-Photos-Optimization-Lab)**

## Visual Proof

### SaaS Quota Recovery
*   **Action:** Bulk purge of data after physical verification.
*   **Context:** Restoring account functionality so the user can send/receive emails.
*   **Validation:** Confirmed storage dropped from 99% full to ~50% via the Google Management Console.

<img src="https://github.com/pbobbitt/Google-Drive-Quota-Photos-Optimization-Lab/blob/main/images/Storage%20Audit%20After.png" alt="Storage Audit showing 50% recovery" width="70%">

### Architecture Redesign
*   **Action:** Configured Partner Sharing and "Storage Saver" ingestion.
*   **Context:** Ensuring future photos don't "clog up" the primary business account.
*   **Validation:** Primary account can view all new photos without utilizing any of its own 15GB quota.

## Professional Key Takeaways
*   **Data Integrity First:** I learned that migration isn't just about moving files; it’s about verifying they arrived safely and ensuring that "Sync" settings don't turn a cloud cleanup into a local disaster.
*   **Proactive Problem Solving:** Instead of just buying more storage, I implemented a technical solution that saves cost and organizes data more logically.
*   **User Experience:** By using "Partner Sharing," I solved the technical storage problem without making the end-user's life harder.

## Contact & Connect  
How to reach me:  
The best way to contact me is via [LinkedIn](https://www.linkedin.com/in/patrickbobbitt/). For technical inquiries regarding my labs, feel free to open an issue.

<p align="left">
  <a href="https://linkedin.com/in/patrick-bobbitt"><img src="https://img.shields.io/badge/LinkedIn-Patrick%20Bobbitt-0072b1?style=for-the-badge&logo=linkedin&logoColor=white" /></a>&nbsp;<a href="mailto:pbobbitt176@gmail.com"><img src="https://img.shields.io/badge/Email-Contact%20Me-red?style=for-the-badge&logo=gmail&logoColor=white" /></a>
</p>

## Disclaimer & AI Disclosure
While 100% of the technical implementation and verification within this environment was conducted by the author, Generative AI was employed to assist in structuring the final report and ensuring professional terminology standards were met throughout the documentation.
