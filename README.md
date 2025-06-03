
# Cyber Forensics Investigation

## Overview
This repository contains all materials, scripts, and documentation for a complete digital forensics investigation conducted on a USB storage device. The goal of this project was to recover deleted data, validate evidence integrity, and analyze recovered files to uncover illicit activities. The investigation focuses on reconstructing user actions, recovering hidden or deleted files, and presenting findings in a final forensic report.

## Table of Contents
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Setup & Installation](#setup--installation)
- [Usage & Methodology](#usage--methodology)
  - [Media Acquisition & Imaging](#media-acquisition--imaging)
  - [Data Recovery](#data-recovery)
  - [Hash Verification](#hash-verification)
  - [File Analysis](#file-analysis)
  - [Reporting](#reporting)
- [Tools & Scripts](#tools--scripts)
- [Key Findings](#key-findings)
- [Reporting](#reporting-1)
- [Disclaimer](#disclaimer)
- [License](#license)
- [Contact](#contact)

## Project Structure
```
├── evidence/                 
│   ├── original_usb.dd         # Disk image of original USB device
│   ├── primary_usb.ad1         # Primary AD1 image for analysis
│   └── failsafe_usb.ad1        # Failsafe AD1 image copy
├── recovery/                  
│   ├── recovered_files/        # All recovered files (documents, images, logs)
│   └── file_hashes.txt         # MD5 and SHA-256 hashes of recovered files
├── scripts/                   
│   ├── recover.sh              # Automated data recovery script (using Autopsy/FTK)
│   ├── hash_verify.sh          # Script to verify hash values on evidence images
│   └── analysis_pipeline.py    # Python script for bulk metadata extraction
├── reports/                    
│   ├── Full_Report.pdf         # Detailed forensic report (PDF format)
│   └── summary_report.md       # Markdown summary of key findings
├── logs/                       
│   └── john_crack.log          # John the Ripper output for password cracking
├── README.md                   # This file
└── LICENSE                     # Licensing information
```

## Prerequisites
- **Operating System**: A Linux-based environment (e.g., Kali Linux, Ubuntu forensics distro).
- **Installed Forensic Tools**:
  - [Autopsy](https://www.sleuthkit.org/autopsy/) v4.x or later
  - [FTK Imager](https://accessdata.com/product-download/forensic-toolkit-ftk) v4.x
  - [The Sleuth Kit](https://www.sleuthkit.org/) command-line tools (fls, icat, etc.)
  - [John the Ripper](https://www.openwall.com/john/) for password cracking
  - [Hashes](https://sourceforge.net/projects/hashdeep/) (md5deep, sha256deep) for hash verification
  - [Python 3](https://www.python.org/) and required modules (`pytsk3`, `olefile`, `PIL`, etc.)

## Setup & Installation
1. **Clone the repository**:
   ```bash
   git clone https://github.com/RajabinandhanPG/Cyber_Forensics_Investigation.git
   cd Cyber_Forensics_Investigation
   ```
2. **Install required tools** on Debian-based systems:
   ```bash
   sudo apt update
   sudo apt install autopsy sleuthkit john hashdeep python3 python3-pip
   pip3 install pytsk3 olefile pillow
   ```
3. **Ensure that evidence images** are located in the `evidence/` directory:
   - `original_usb.dd`
   - `primary_usb.ad1`
   - `failsafe_usb.ad1`

## Usage & Methodology

### Media Acquisition & Imaging
1. **Create a disk image** using FTK Imager or `dd`:
   ```bash
   sudo dd if=/dev/sdX of=evidence/original_usb.dd bs=4M status=progress
   ```
2. **Convert or capture AD1 image** using Autopsy or FTK:
   - Use FTK Imager to create `primary_usb.ad1` and `failsafe_usb.ad1` clones.
   - Maintain chain-of-custody logs for each acquisition.

### Data Recovery
1. **Launch Autopsy** to process the AD1 image:
   ```bash
   autopsy &
   ```
2. **Add a new case** and ingest `primary_usb.ad1`:
   - Recover deleted files, extract file metadata, and export to `recovery/recovered_files/`.
3. **Automated recovery** (example using `recover.sh`):
   ```bash
   cd scripts
   chmod +x recover.sh
   ./recover.sh ../evidence/primary_usb.ad1 ../recovery/recovered_files/
   ```

### Hash Verification
1. **Verify integrity** of evidence images:
   ```bash
   cd scripts
   chmod +x hash_verify.sh
   ./hash_verify.sh ../evidence/original_usb.dd ../evidence/file_hashes.txt
   ```
2. **Record MD5 and SHA-256 hashes** in `recovery/file_hashes.txt` for chain-of-custody.

### File Analysis
1. **Metadata extraction** (sample via `analysis_pipeline.py`):
   ```bash
   python3 scripts/analysis_pipeline.py -i recovery/recovered_files/ -o logs/metadata_report.csv
   ```
2. **Password cracking** for protected documents:
   ```bash
   john --wordlist=/usr/share/wordlists/rockyou.txt logs/password_protected_hashes.txt >> logs/john_crack.log
   ```
3. **Analyze recovered images** manually or with Python:
   ```bash
   python3 scripts/image_metadata.py recovery/recovered_files/*.jpg
   ```

### Reporting
1. **Generate detailed findings** in PDF:
   - The complete forensic narrative, screenshots, and appendices are in `reports/Full_Report.pdf`.
2. **Create a summary report** in Markdown:
   ```bash
   cp reports/summary_report_template.md reports/summary_report.md
   # Edit summary_report.md with key findings and recommendations
   ```
3. **Review and finalize** the summary for quick reference.

## Tools & Scripts
- **`scripts/recover.sh`**: Automates Autopsy ingestion and file export.
- **`scripts/hash_verify.sh`**: Computes and verifies MD5/SHA-256 hashes for evidence.
- **`scripts/analysis_pipeline.py`**: Extracts file metadata (timestamps, authorship, file types).
- **`logs/john_crack.log`**: Contains John the Ripper output for password-protected documents.
- **`recovery/recovered_files/`**: Directory containing all recovered documents, images, and logs.

## Key Findings
- Multiple Microsoft Word documents password-protected with keywords (`special`, `picture`, `collect`) were recovered, indicating attempts to conceal illicit communications.
- Evidence of workplace sexual harassment and distribution of illegal pornographic content (both adult and child) was uncovered.
- Hash verification demonstrated that recovered files matched original evidence images, ensuring data integrity.
- Recovered images were classified under “Child Pornography” and “Adult Pornography” based on Illinois statutes, confirming criminal activity.

## Reporting
- **Full Forensic Report**: `reports/Full_Report.pdf`  
- **Summary of Findings**: `reports/summary_report.md`

## Disclaimer
This repository is intended for educational and authorized forensic examination only. Accessing or using these materials without proper authorization may violate legal statutes. The author and contributors assume no liability for unauthorized use.

## License
This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

## Contact
For questions or further information, please open an issue on GitHub or contact:
- **Investigator**: Rajabinandhan Deekshitha  
- **Email**: your-email@example.com
