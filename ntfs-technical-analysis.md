# NTFS Technical Analysis

## NTFS Structures Involved

### $MFT (Master File Table)

The Master File Table is the core metadata structure in NTFS.  
Each file and directory is represented as an MFT entry containing:

- File attributes
- Timestamps
- Data run mappings
- Security descriptors

Corruption in the MFT prevents the filesystem from reconstructing directory structure and file metadata.

---

### $Bitmap

The $Bitmap file tracks cluster allocation status.

If corrupted:
- The system may believe used clusters are free
- Or free clusters are occupied
- Mount operations may fail due to allocation inconsistency

---

### $LogFile (NTFS Journal)

$LogFile stores transactional updates to maintain filesystem consistency.

If journal replay fails:
- NTFS may refuse to mount
- Metadata consistency checks may fail

---

### Boot Sector & Backup Boot Sector

The NTFS boot sector contains:

- Bytes per sector
- Sectors per cluster
- MFT start location
- Volume serial number

If the primary boot sector is damaged, NTFS may use the backup boot sector.  
Corruption in both prevents mount operations.

---

## Why Metadata Recovery Was Prioritized

Raw file carving (PhotoRec):

- Ignores filesystem structure
- Strips filenames
- Removes directory hierarchy
- Loses original timestamps
- Cannot reconstruct fragmented files reliably

Therefore:

1. Partition integrity was analyzed first.
2. NTFS boot sector consistency was verified.
3. MFT-based recovery was attempted.
4. Raw carving was used only as a last resort.

This preserves maximum evidentiary and structural integrity.
