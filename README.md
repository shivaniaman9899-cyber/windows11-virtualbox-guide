**Windows 11 Installation in VirtualBox -
Troubleshooting Guide**
**Problem**
Installing Windows 11 in Oracle VirtualBox fails due to TPM 2.0 and Secure Boot
requirements.
**TPM 2.0 (Trusted Platform Module 2.0)**: A security chip either built into your motherboard
or firmware that stores encryption keys, passwords, and other sensitive data.
                     **Command & Term Reference**
  A quick glossary of every command and key term used in this guide:
         Command / Term Meaning:
## regedit:
Opens the Windows Registry Editor — a tool
to view and edit low-level system settings
stored in the Windows Registry
## HKEY_LOCAL_MACHINE:
A top-level registry hive (root key) that stores
system-wide configuration settings applying to all users on the machine
## \SYSTEM\Setup\MoSetup: 
A registry path under HKLM that controls Windows setup/upgrade compatibility checks
##\SYSTEM\Setup\LabConfig:
A registry path used during Windows installation to override hardware requirement checks
## DWORD (32-bit) Value:
A registry data type that stores a 32-bit
integer (whole number). Used here to store a 0 (disabled) or 1 (enabled) flag
## AllowUpgradesWithUnsupportedTPMOrCPU:
A registry value that tells the Windows
installer to allow installation even if the CPU
or TPM does not meet requirements
## BypassTPMCheck: 
Skips the check for a TPM 2.0 chip during
Windows 11 installation
## BypassSecureBootCheck: 
Skips the check for Secure Boot being
enabled during Windows 11 installation
## BypassRAMCheck: 
Skips the check for the minimum 4 GB RAM
requirement during Windows 11 installation
## BypassStorageCheck:
Skips the check for the minimum 64 GB
storage requirement during Windows 11
installation
## Shift + F10:
A keyboard shortcut during Windows Setup
that opens a Command Prompt window
## F12:
At VM startup, opens the one-time boot
menu so you can choose which device to
boot from
## UEFI:
Unified Extensible Firmware Interface — a
modern replacement for BIOS that supports
Secure Boot and is required by Windows 11
## Secure Boot:
A UEFI feature that ensures only trusted,
signed software can run at startup —
Windows 11 requires it to be enabled
## ISO:
A disk image file ( .iso ) that is an exact
copy of a CD/DVD. Used here as a virtual
installation disc for Windows 11
## Optical Drive: 
A virtual CD/DVD drive inside VirtualBox
where the ISO file is mounted
 ## Disk 0 Unallocated Space: 
 The empty, unformatted portion of the virtual
hard disk where Windows 11 will be installed
## MoSetup:
Short for “Modern Setup” — a Windows
registry key used to configure compatibility
during installation
## LabConfig:
A registry key specifically designed to let
users configure and bypass hardware checks
during Windows Setup
**Step 1: Mount the Windows 11 ISO**
1. Open Oracle VirtualBox.
2. Go to Devices → Optical Drives.
3. Click Choose/Create a Disk Image.
4. In the Optical Disk Selector, select Win11_25H2_English_x64_v2.iso.
5. Click Choose.
**Step 2: Configure Boot Order**
1. Go to VM Settings → System → Motherboard.
2. Scroll down and ensure UEFI and Secure Boot are checked.
3. The Boot Device Order will be grayed out (BIOS only) when UEFI is enabled.
4. To work around this, use F12 at VM startup to access the one-time boot menu.
**Step 3: Boot from the ISO**
1. Start the VM.
2. Immediately and repeatedly press F12 to open the boot menu.
3. Select the Optical Drive to boot from the Windows 11 ISO.
**Step 4: Bypass TPM and Secure Boot Requirements
When the installer shows “This PC doesn’t currently meet Windows 11 system
requirements”:**
1. Go Back to the Select Image screen (version selection).
2. Press Shift + F10 to open a Command Prompt.
Shift + F10 — opens a Command Prompt directly inside Windows Setup.
3. Type regedit and press Enter.
regedit — launches the Registry Editor so you can modify system settings.
4. In Registry Editor, navigate to:
HKEY_LOCAL_MACHINE\SYSTEM\Setup\MoSetup
HKEY_LOCAL_MACHINE\SYSTEM\Setup\MoSetup — the registry location where Windows
Setup reads compatibility override settings.
Note: If the MoSetup key does not exist, right-click Setup → New → Key and name it
MoSetup .
5. Inside MoSetup, create the following DWORD value:
Right-click → New → DWORD (32-bit) Value → name it
AllowUpgradesWithUnsupportedTPMOrCPU → set value to 1
Setting this to 1 tells the installer to allow installation even without a supported TPM
or CPU.
6. Alternatively, navigate to:
HKEY_LOCAL_MACHINE\SYSTEM\Setup\LabConfig
Note: If the LabConfig key does not exist, right-click Setup → New → Key and name it
LabConfig .
7. Inside LabConfig, create the following DWORD values:
BypassTPMCheck = 1 → Skips TPM 2.0 chip verification
BypassSecureBootCheck = 1 → Skips Secure Boot verification
BypassRAMCheck = 1 → Skips the 4 GB RAM minimum check
BypassStorageCheck = 1 → Skips the 64 GB storage minimum check
For each: Right-click → New → DWORD (32-bit) Value → enter the name → doubleclick → set value to 1 .
8. Close Registry Editor and Command Prompt.
9. Click Next in the installer.
**Step 5: Select Windows Edition and Install**
1. Select Windows 11 Pro.
2. Click Next.
3. On the “Select location to install Windows 11” screen, select Disk 0 Unallocated
Space.
Disk 0 Unallocated Space — the empty virtual hard disk space where Windows will
be installed.
4. Click Next to begin installation.
**Step 6: Post-Installation (Important)**
1. After installation completes and the VM reboots, go to VM Settings → System →
Motherboard.
2. Change the boot order so the Hard Disk is listed before the Optical Drive, or remove the
ISO from the optical drive via Devices → Optical Drives → Remove disk from virtual
drive.
3. This ensures the VM boots from the installed Windows 11 on the hard disk and not the
ISO again.
**Notes**
The LabConfig registry bypass tricks the installer into skipping hardware checks.
This is safe for VirtualBox VMs as TPM and Secure Boot are virtual requirements.
After installation, re-check boot order so the VM boots from the hard disk by default.
The MoSetup key method ( AllowUpgradesWithUnsupportedTPMOrCPU ) is the
recommended Microsoft-acknowledged path; LabConfig is the installer-time bypass.
**What I Learned**
##Virtual Skills:
1. How to mount an ISO file to a virtual optical drive.
2. How to configure VM settings.
3. How to use F12 to access the one-time boot menu.
Windows Knowledge:
1. What TPM 2.0 is and why Windows 11 requires it.
2. What Secure Boot is and its role in system security.
General Tech Skills:
1. Problem solving.
2. Understanding why each step is necessary.
