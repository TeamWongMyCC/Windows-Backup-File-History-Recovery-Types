# Windows-Backup-File-History-Recovery-Types
This repository documents a lab exercise focusing on configuring, managing, and restoring data using native Windows backup tools to understand operational trade-offs between Full, **Incremental, Differential, and Synthetic Full backup schemes.

# Hands-On Lab: Windows Backup, File History & Recovery Types

## 📌 Executive Summary
This repository documents a practical lab exercise performed using **Practice Labs**, focusing on configuring, managing, and restoring data using native Windows backup tools. 

Understanding data protection strategies is a foundational skill in IT administration. This lab covers full image backups, File History versioning, System Restore points, and the operational trade-offs between **Full**, **Incremental**, **Differential**, and **Synthetic Full** backup schemes.

---

## ⚙️ Environment Topology
* **Domain Controller (`PLABDC01`):** Windows Server 2019
* **Workstation 1 (`PLABWIN10`):** Windows 10 (Domain Member)
* **Workstation 2 (`PLABWIN11`):** Windows 11 (Domain Member)

---

## 🛠️ Key Lab Exercises & Tasks

### Task 1: Full Backup Configuration (`PLABWIN10`)
* **Objective:** Create an exact snapshot/backup of user-created data files and schedule recurring jobs.
* **Process:**
  1. Navigated to `Control Panel` > `System and Security` > `Backup and Restore (Windows 7)`.
  2. Targeted secondary volume `ISO (D:)` as the backup destination.
  3. Configured customized scope ("Let me choose") to isolate user directories.
  4. Established an automated schedule (`Every Sunday at 7:00 PM`).
  5. Triggered and completed an initial **22.27 GB** full backup job.

### Task 2: Data Restoration from Full Backup (`PLABWIN10`)
* **Objective:** Validate disaster recovery capabilities by restoring C: drive directories.
* **Process:**
  1. Accesses `Settings` > `Update & Security` > `Backup`.
  2. Initiated the **Restore my files** workflow via the legacy restore interface.
  3. Browsed `ISO (D:)` target directory, resolved file conflicts with "Copy and Replace", and completed the restore sequence.

### Task 3: File History & Retention Policy (`PLABWIN11`)
* **Objective:** Configure real-time file-versioning for user data directories (Documents, Pictures, Desktop, etc.).
* **Process:**
  1. Enabled **File History** on `PLABWIN11`.
  2. Configured exclusion rules (e.g., excluding the `Music` folder to optimize storage space).
  3. Explored **Advanced Settings** to configure version intervals and retention policies (cleaning up versions older than 1 year).

### Task 4 & 5: System Restore Point Creation & Recovery (`PLABWIN11`)
* **Objective:** Protect OS state integrity prior to critical system updates or application deployments.
* **Process:**
  1. Enabled System Protection on `Local Disk (C:)` and allocated **20% maximum disk usage**.
  2. Purged legacy restore states and generated a manual restore point named `Restore point 1`.
  3. Executed a system recovery to `Restore point 1`, validating state rollback across a system reboot cycle (~10 min execution window).

---

## 🧠 Theory & Concepts Matrix

| Backup Type | Description | Archive Bit Behavior | Backup Speed | Restore Speed | Storage Cost |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Full** | Copies all target data files. | Cleared (reset) after job finishes | Slowest | **Fastest** (1 set needed) | Highest |
| **Incremental** | Copies only data changed *since last backup* of any type. | Cleared (reset) after job finishes | **Fastest** | Slowest (Full + all incrementals) | Lowest |
| **Differential** | Copies all data changed *since last full backup*. | **Not** cleared (remains set) | Moderate | Moderate (Full + last differential) | Moderate |
| **Synthetic Full** | Merges previous full backup + recent incrementals into a new baseline. | Cleared according to job type | Fast (network-efficient) | Fast | Optimized |

> 💡 **Key Takeaway on Archive Bits:** The archive bit acts as a flag indicating whether a file has been modified since the last backup. Incremental backups clear this flag so the next job only captures new modifications; Differential backups leave the flag active so subsequent jobs capture all cumulative changes since the baseline Full backup.

---

## 🚀 How to Replicate
1. Ensure access to a Windows 10/11 environment with a secondary volume or network share attached.
2. Follow steps outlined in `docs/` for task-by-task execution commands and UI workflows.
