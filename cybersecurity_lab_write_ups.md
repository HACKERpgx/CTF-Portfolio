# Cybersecurity & Technical Lab Write-Ups

This repository contains practical lab write-ups documenting troubleshooting processes, environment setups, and core cybersecurity concepts.

---

## 🛠️ Lab 1: WSL 2 Ubuntu Environment Setup & Troubleshooting

### **Overview**
The objective of this lab was to set up a functional Ubuntu environment using **Windows Subsystem for Linux (WSL 2)** to serve as a platform for installing cybersecurity and digital forensics tools.

---

### **Challenges Encountered**
1. **Distribution Corruption:** WSL failed to launch due to missing shell components and corrupt user-account files in the existing Ubuntu installation.
2. **Path & Mounting Errors:** Windows host and Linux guest filesystem translation errors prevented directory integration.
3. **Firmware Virtualization Disabled:** After a fresh reinstallation, WSL 2 failed to initialize because hardware virtualization was disabled at the firmware level.

---

### **Commands & Methodologies Used**

#### **1. Environment Inspection & Reset**
```cmd
# Check running WSL distributions and their architecture version
wsl --list --verbose

# Force shutdown of stuck instances
wsl --shutdown

# Unregister corrupted distribution instance
wsl --unregister Ubuntu-24.04

# Perform a clean installation of Ubuntu 24.04 LTS
wsl --install -d Ubuntu-24.04
```

#### **2. Enabling Windows Features & Hypervisor**
```cmd
# Enable Windows Subsystem for Linux feature via DISM
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# Enable Virtual Machine Platform feature via DISM
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# Ensure hypervisor launch type is set to auto
bcdedit /set hypervisorlaunchtype auto
```

#### **3. Firmware & System Verification**
* Enabled **Intel Virtualization Technology (VT-x)** in the HP BIOS configuration menu.
* Verified hardware virtualization status using the command:
```cmd
systeminfo
```

---

### **Key Insights & Retrospective**
* **Core Dependency:** WSL 2 relies on both host OS virtual machine components and BIOS/UEFI-level hardware virtualization.
* **Lessons Learned:**
  1. Always verify firmware virtualization status prior to deploying hypervisor tools.
  2. Maintain export backups of configured distributions (`wsl --export`) before troubleshooting major updates.
  3. Change and test settings systematically, verifying results step-by-step rather than making multiple changes concurrently.

---

## 🔐 Lab 2: Base64 Encoding & Decoding with CyberChef

### **Overview**
This lab focuses on fundamental cryptographic concepts by encoding and decoding data using **Base64** via the online utility CyberChef.

* **Target Data:** `CompTIA Security Plus`
* **Tool:** [CyberChef](https://gchq.github.io/CyberChef/)

---

### **Execution Steps**

```
[ Plain Text Input ] ➡️ [ Operation: To Base64 ] ➡️ [ Encoded Output ]
"CompTIA Security Plus"                       "Q29tcFRJQSBTZWN1cml0eSBQbHVz"

[ Encoded Input ]   ➡️ [ Operation: From Base64 ] ➡️ [ Decoded Output ]
"Q29tcFRJQSBTZWN1cml0eSBQbHVz"                       "CompTIA Security Plus"
```

1. **Encoding:** Inputted `CompTIA Security Plus` into CyberChef and applied the `To Base64` recipe.
   * **Result:** `Q29tcFRJQSBTZWN1cml0eSBQbHVz`
2. **Decoding:** Took the encoded payload and applied the `From Base64` recipe.
   * **Result:** Successfully restored original plain text `CompTIA Security Plus`.

---

### **Key Insights & Retrospective**
* **Encoding vs. Encryption:** Base64 is a data transformation mechanism used to safely transmit binary data over text-based protocols; **it is not encryption** and offers zero confidentiality.
* **Tool Operation Precision:** Distinguishing between target inputs (raw payload vs. encoded string) determines whether `To Base64` or `From Base64` operations should be applied.