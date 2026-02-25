# SSD NTFS Recovery Case Study

## 📌 Project Overview

This case study documents the forensic-style recovery of a 2TB NTFS-formatted disk that became inaccessible due to logical filesystem corruption.

The disk was detected at the hardware level but failed to mount on both Windows and Linux systems. The objective was to recover maximum data while preserving original media integrity using a controlled, forensic-safe workflow.

---

## Initial Symptoms

- Disk detected as `/dev/sdX`
- Partition table present
- Volume not mountable
- Windows unable to access filesystem
- Linux mount attempts returned errors
- Installer reported disk access failure
- Disk reported full capacity but directory structure inaccessible

<img width="961" height="1079" alt="image" src="https://github.com/user-attachments/assets/6311f113-5400-4da6-b455-fbb4783cb5db" />

These indicators suggested logical filesystem corruption rather than catastrophic hardware failure.

---

## SMART Analysis

SMART diagnostics reported:

- Overall health: PASSED
- Pending sectors: Non-zero
- Reallocated sectors: Low / None

### Interpretation

- No immediate mechanical failure
- Early-stage sector instability suspected
- Filesystem likely corrupted due to unstable I/O combined with full disk condition

SMART “PASSED” does not guarantee data integrity.

---

## Probable Causes

- Sudden power interruption
- Unsafe device removal
- NTFS journal inconsistency
- Disk operating at 100% capacity
- Weak sectors causing repeated read retries

Conclusion: Logical corruption with early hardware instability.

---

## Recovery Strategy

### Step 1 — Block-Level Imaging

**Tool used:** `ddrescue`

**Reason:**

- Sector-by-sector cloning
- Logs unreadable sectors
- Minimizes repeated stress
- Preserves exact disk state

**Command used:**

```bash
sudo ddrescue -f -n /dev/sdX /dev/sdY rescue.log
sudo ddrescue -d -r3 /dev/sdX /dev/sdY rescue.log
```

A complete clone was created before any repair attempts.

---

### Step 2 — Operate on Clone Only

All further recovery operations were performed on the cloned disk to ensure:

- Original disk preserved in read-only state
- Safe experimentation with repair tools
- No additional degradation of unstable sectors

---

### Step 3 — Filesystem Recovery

**Tools used:**

- TestDisk
- PhotoRec
- ddrescue

**Recovery order:**

1. Analyze partition structure
2. Repair NTFS boot sector if damaged
3. Attempt MFT-based recovery
4. Use raw carving only if metadata unrecoverable

Metadata recovery was prioritized over file carving.

---

## Recovery Outcome

- Majority of data successfully recovered
- Minimal sector-level loss
- Some files partially corrupted due to unreadable sectors
- NTFS structure largely restored

---

## Key Lessons

- Clone first, repair later
- Never operate directly on failing media
- SMART “PASSED” does not mean safe
- Full disks increase corruption risk
- Avoid virtualization for hardware-level recovery
- Follow the 3-2-1 backup rule

---

## Tools Used

- Kali Linux (Live Environment)
- ddrescue
- TestDisk
- PhotoRec
- SMART diagnostic utilities

---

## Skills Demonstrated

- Block-level disk imaging
- Filesystem corruption analysis
- SMART interpretation
- NTFS recovery methodology
- Forensic-safe workflow design
- Risk-controlled recovery execution

---

## Conclusion

The original disk was preserved throughout the recovery process. All operations were performed on a cloned replica to maintain data integrity and prevent irreversible loss.

---

## 👨‍💻 Author

Yogesh Mondal  
Class 10 Student | Cybersecurity Enthusiast

---

## Original Inages 


<img width="1600" height="903" alt="image" src="https://github.com/user-attachments/assets/cc6aefac-756a-49ef-b95d-4ec9d0fad499" />
<img width="961" height="1079" alt="image" src="https://github.com/user-attachments/assets/5fa2f651-7591-4c09-b98c-06c034365e6e" />
<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/9f82c279-53ef-489e-9e58-dda47b0d7152" />
<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/17509eb8-3b69-49b6-befa-dc5a5aaf8f12" />
<img width="1152" height="817" alt="Untitled design (3)" src="https://github.com/user-attachments/assets/1555d39b-54b6-4e29-a3cb-6602df63c60a" />





