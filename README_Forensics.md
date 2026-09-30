# Insider Data Exfiltration — iOS Backup Forensic Analysis

A digital forensics project that investigates an insider data-leak scenario on a company-issued iPad and examines what evidence an iOS logical backup does — and does not — preserve. Built as an individual project for **Cyberattack Lifecycle (CSC G0040)** at **The City College of New York (CCNY)**.

> **Safety notice:** All activity was performed on a personal test device inside an isolated forensic virtual machine, using only simulated files. No real confidential data was used.

---

## Scenario

An employee with legitimate access to confidential business files on a company-issued iPad secretly transfers documents to personal cloud storage (iCloud Drive, Google Drive, Dropbox). The goal of the investigation is to simulate that activity, capture a device backup, and determine how much of the insider's behavior can be reconstructed from forensic evidence.

## Objectives

1. Simulate insider file activity on an iPad — opening, copying, deleting, and uploading files.
2. Acquire a full logical device backup for analysis.
3. Analyze the backup in Autopsy to identify file interactions and cloud-sync artifacts.
4. Assess the limitations of logical backups as a source of forensic evidence.

## Methodology

1. **Evidence creation** — perform file actions on the iPad and upload documents to personal cloud storage.
2. **Backup acquisition** — create a full unencrypted iTunes backup on Windows (stored under `MobileSync/Backup`).
3. **Analysis environment** — transfer the backup into an isolated Ubuntu forensic VM (Oracle VirtualBox).
4. **Examination** — load the logical evidence into Autopsy and review the file system, metadata, and any cloud-related artifacts, then build a timeline of user actions.

## Tools

| Tool | Purpose |
|---|---|
| iTunes | Create a full logical backup of the iPad |
| Oracle VirtualBox | Host an isolated Ubuntu forensic analysis VM |
| Autopsy 4.22 | Examine the backup — file system, metadata, timeline |

## Key Findings

- A logical iTunes backup does preserve useful artifacts such as document metadata (creation dates, file names) recoverable in Autopsy.
- However, a logical backup does **not** capture several categories of user-activity evidence, because Apple stores them in protected areas that the backup does not export:
  - File open, copy, and move actions
  - Upload activity to Google Drive / Dropbox / iCloud
  - File deletion activity
  - Files app usage history
  - A detailed event timeline and cloud-sync transaction logs
- **Conclusion:** a logical backup alone is insufficient to fully prove insider exfiltration. Stronger evidence requires additional sources.

## Why This Matters (Control Perspective)

From an IT audit / GRC standpoint, this project highlights the controls an organization should have in place to detect and investigate insider data exfiltration on mobile devices:

- **Mobile Device Management (MDM)** on company-issued devices to control and log activity
- **Data Loss Prevention (DLP)** to block uploads of confidential files to personal cloud storage
- **Cloud-side logging** from the organization's own cloud tenant, since device backups do not capture upload activity
- **Least-privilege access** to confidential files, so exposure is limited from the start

## Limitations and Future Work

- Analysis was limited to a single logical backup; a full file-system or physical acquisition would recover more.
- Future work could correlate device evidence with cloud-provider audit logs to build a complete exfiltration timeline.

## References

- Apple, *iTunes/Finder device backup documentation*
- Autopsy / The Sleuth Kit documentation
- NIST SP 800-101 Rev. 1, *Guidelines on Mobile Device Forensics*

## Author

**Chanakan Arponpong**
M.S. Cybersecurity, The City College of New York
[LinkedIn](https://www.linkedin.com/in/chanakan-arponpong-33956a1bb)
