
# Cisco Catalyst 2960-X | Hands-On Physical Network Infrastructure Project

**Secure Data Sanitization | Cisco IOS Recovery | Boot Configuration | Hardware Verification | Post-Recovery Validation**

![Brandon Stevenson — IT Professional](assets/brandons-logo.png)

## Project Overview

This real-world, physical, hands-on network infrastructure project documents the servicing, secure sanitization, Cisco IOS recovery, boot configuration, and post-recovery validation of a Cisco Catalyst 2960-X enterprise access switch.

Unlike a simulated networking lab, this project involved working directly with physical enterprise networking equipment using a serial-console connection, Cisco IOS CLI, bootloader recovery environment, and USB-based software installation.

The project demonstrates practical experience in:

- Enterprise network hardware servicing
- Cisco IOS software recovery and installation
- Bootloader access and flash storage management
- Secure factory-reset and zeroization procedures
- USB-to-flash IOS image recovery
- Boot variable configuration and verification
- Running and startup configuration inspection
- VLAN and VTP verification
- Hardware inventory validation
- Technical troubleshooting and documentation

> **Project scope:** This project documents selected operations performed on an isolated physical switch. It is not a production network deployment, comprehensive hardware certification, or formal sanitization certificate.

---

## Physical Equipment Gallery

### Cisco Catalyst 2960-X — Full Switch Overview

![Cisco Catalyst 2960-X Physical Switch](assets/catalyst-2960x-full-switch.jpg)

*Physical Cisco Catalyst 2960-X enterprise switch used for this project.*

### Front Panel and SFP Uplink Interfaces

![Cisco Catalyst 2960-X SFP Ports](assets/catalyst-2960x-sfp-uplinks.jpg)

*Close-up view of the Catalyst 2960-X access ports and SFP uplink interfaces.*

> **Image disclosure:** These equipment photographs have digitally modified office backgrounds for presentation consistency. Actual terminal captures are provided below as technical evidence.

---

## Technical Environment

| Component | Specification |
|---|---|
| Manufacturer | Cisco Systems |
| Product Family | Catalyst 2960-X |
| Model | WS-C2960X-24TS-L |
| Operating System | Cisco IOS |
| Software Image | c2960x-universalk9-mz.152-7.E11.bin |
| Access Interfaces | 24 Gigabit Ethernet |
| Uplink Interfaces | 4 SFP |
| Console Software | Tera Term |
| Recovery Environment | Cisco Switch Bootloader |
| Installation Media | USB Flash Drive |
| Infrastructure Type | Physical Enterprise Networking |
| Network Environment | Offline / Isolated |

---

## Project Objectives

The primary objective was to prepare an enterprise Cisco switch for its next stage of processing through secure sanitization, operating-system recovery, and validation.

The project focused on the following objectives:

1. Access the Cisco bootloader and inspect internal flash.
2. Perform approved secure-sanitization operations.
3. Recover the operating system using external USB media.
4. Restore the approved Cisco IOS image to internal flash.
5. Configure the intended boot image.
6. Verify the boot variable using Cisco IOS commands.
7. Inspect running and startup configurations.
8. Review the VLAN database and VTP settings.
9. Confirm the switch model and hardware inventory.
10. Document the observed CLI results and technical findings.

---

# Technical Implementation Walkthrough

## Phase 1 — Flash Storage Inspection and Formatting

**Commands observed:**

```bash
format flash:
dir flash:
```

### Technical Explanation

The switch's internal flash filesystem was accessed through the bootloader environment.

The formatting operation targeted the contents of internal flash storage.

The directory was subsequently inspected to review the remaining filesystem contents.

This operation is part of the overall sanitization workflow, but flash formatting alone does not establish successful secure sanitization.

### CLI Evidence

![Flash Formatting](evidence/2960x-step1.png)

**Result:** Flash formatting and subsequent directory inspection were captured.

---

## Phase 2 — Booting Cisco IOS from USB

**Command observed:**

```bash
boot usbflash0:c2960x-universalk9-mz.152-7.E11.bin
```

### Technical Explanation

After flash handling, Cisco IOS was loaded from external USB media.

This provides a recovery method when the internal operating-system image has been removed or is unavailable.

The bootloader locates the specified image and initiates the IOS boot process.

### CLI Evidence

![USB IOS Boot](evidence/2960x-step2.png)

**Result:** USB-based IOS boot activity was captured.

---

## Phase 3 — Secure Factory-Reset Operation

**Command observed:**

```bash
factory-reset all secure
```

### Technical Explanation

The secure factory-reset procedure targets previously stored configuration and system data.

Depending on platform capabilities and software behavior, the process can affect configuration data, logs, IOS images, and other persistent storage.

The reset operation is destructive and requires the appropriate authorization and controlled procedure.

### CLI Evidence

![Secure Factory Reset](evidence/2960x-step3.png)

**Result:** The factory-reset invocation and associated confirmation information were recorded.

**Verification limitation:** This screenshot alone does not establish complete sanitization acceptance.

---

## Phase 4 — FIPS Zeroization

**Commands observed:**

```bash
configure terminal
fips zeroize
```

### Technical Explanation

FIPS zeroization is intended to remove applicable cryptographic security material and reset associated security state.

The operation is irreversible and may remove stored IOS images and cause the switch to reboot.

This operation was performed as part of the controlled sanitization sequence.

### CLI Evidence

![FIPS Zeroization](evidence/2960x-step4.png)

**Result:** Zeroization prompts and related reset output were captured.

---

## Phase 5 — Bootloader Environment Verification

**Commands observed:**

```bash
set
dir usbflash0:
```

### Technical Explanation

The `set` command displays bootloader environment variables.

These can include boot paths, reset information, hardware identifiers, and other platform-specific settings.

The USB filesystem was also inspected to verify the availability of the recovery image.

### CLI Evidence

![Bootloader Verification](evidence/2960x-step5.png)

**Result:** Bootloader variables and removable-media contents were inspected.

Device serial numbers were redacted from the evidence.

---

## Phase 6 — IOS Recovery Following Zeroization

**Command observed:**

```bash
boot usbflash0:c2960x-universalk9-mz.152-7.E11.bin
```

### Technical Explanation

Following the reset and zeroization sequence, the Cisco IOS recovery image was booted again from USB.

This allowed access to the IOS command-line environment for further recovery operations.

### CLI Evidence

![IOS Recovery](evidence/2960x-step6.png)

**Result:** The post-zeroization USB boot sequence was captured.

---

## Phase 7 — IOS Installation and Boot Variable Configuration

**Commands observed:**

```bash
copy usbflash0: flash:
```

The source image was:

```text
c2960x-universalk9-mz.152-7.E11.bin
```

The boot configuration sequence included:

```bash
configure terminal
boot system flash:c2960x-universalk9-mz.152-7.E11.bin
end
copy running-config startup-config
show boot
```

### Technical Explanation

The IOS image was copied from removable USB storage into the switch's internal flash.

A boot statement was then configured to reference the recovered image.

The running configuration was saved, and the boot configuration was inspected.

### Why This Matters

A switch may contain a valid IOS image in flash but still fail to boot automatically if its boot configuration references an incorrect or unavailable image.

Verifying the boot variable is therefore a separate and important recovery step.

### CLI Evidence

![IOS Installation and Boot Verification](evidence/2960x-step7.png)

**Result:** IOS file transfer, boot configuration, and boot-variable inspection were captured.

**Verification limitation:** An independent successful reload from internal flash without USB is not documented in the supplied screenshots.

---

## Phase 8 — Running Configuration Inspection

**Command observed:**

```bash
show running-config
```

### Technical Explanation

The running configuration represents the configuration currently active in memory.

Reviewing it helps identify unexpected device-specific configuration settings.

Relevant items may include:

- Hostname
- Interface configuration
- VLAN assignments
- Management addressing
- Remote-access settings
- Authentication configuration
- Boot statements

### CLI Evidence

![Running Configuration](evidence/2960x-step8.png)

**Result:** A portion of the running configuration was captured.

The visible output does not establish that every configuration setting was inspected.

---

## Phase 9 — Startup Configuration Inspection

**Command observed:**

```bash
show startup-config
```

### Technical Explanation

The startup configuration represents the saved configuration intended to persist across reboots.

Comparing running and startup configurations helps identify configuration differences.

In this recovery workflow, saving the boot configuration may intentionally create a startup configuration.

The final state must match the applicable equipment-processing requirements.

### CLI Evidence

![Startup Configuration](evidence/2960x-step9.png)

**Result:** Startup configuration output was captured.

---

## Phase 10 — VLAN Database Verification

**Command observed:**

```bash
show vlan
```

### Technical Explanation

The VLAN database was inspected for expected default and reserved VLANs.

Cisco Catalyst switches commonly include default VLAN 1 and reserved VLANs 1002 through 1005.

Unexpected customer-created VLANs may indicate residual configuration requiring further review.

### CLI Evidence

![VLAN Verification](evidence/2960x-step10.png)

**Result:** Default VLAN information was visible in the captured output.

No custom VLAN was visible in the screenshot.

---

## Phase 11 — VTP and Power Capability Inspection

**Commands observed:**

```bash
show vtp status
show power inline
```

### Technical Explanation

VLAN Trunking Protocol (VTP) status was inspected to review the switch's VLAN management state.

Relevant fields include:

- VTP operating mode
- VTP version
- VTP domain
- Configuration revision
- VLAN-related settings

The `show power inline` command was also used to inspect power-delivery capabilities.

### Hardware Consideration

The WS-C2960X-24TS-L is a non-PoE model.

Therefore, unavailable PoE output is expected and should not be interpreted as a failed PoE test.

### CLI Evidence

![VTP Verification](evidence/2960x-step11.png)

**Result:** VTP status and power capability information were recorded.

---

## Phase 12 — Hardware Inventory Validation

**Command observed:**

```bash
show inventory
```

### Technical Explanation

The inventory command identifies installed hardware information.

This is useful for confirming the model and validating the device against the expected equipment specification.

### CLI Evidence

![Hardware Inventory](evidence/2960x-step12.png)

**Result:** The switch was identified as a Cisco Catalyst WS-C2960X-24TS-L.

The serial number was redacted from the screenshot.

---

# Verification and QA Matrix

| Validation Item | Evidence Status |
|---|---|
| Physical enterprise switch used | Documented |
| Flash formatting | Observed |
| USB-based IOS boot | Observed |
| Secure reset invocation | Observed |
| FIPS zeroization procedure | Observed |
| IOS image recovery into flash | Observed |
| Boot variable configuration | Observed |
| Boot variable inspection | Observed |
| Running configuration inspection | Partially documented |
| Startup configuration inspection | Partially documented |
| VLAN database inspection | Observed |
| VTP inspection | Observed |
| Hardware inventory verification | Observed |
| Trusted image checksum comparison | Not documented |
| Independent reboot from internal flash | Not documented |
| Complete copper port-health testing | Not documented |
| SFP uplink connectivity testing | Not documented |
| Formal sanitization audit acceptance | Outside portfolio evidence |

---

# Additional Infrastructure Diagnostics

The following commands are recommended for future authorized validation work. They are not represented as completed in this project.

## System Health

```bash
show version
show inventory
show env all
show processes cpu sorted
show memory statistics
show logging
```

These commands can help assess:

- IOS version and image information
- Hardware inventory
- Environmental operating status
- CPU utilization
- Memory usage
- System logs and errors

## Ethernet Interface Health

```bash
show interfaces status
show interfaces description
show interfaces counters errors
show interfaces status err-disabled
show interfaces gigabitEthernet1/0/1
```

These commands can help identify:

- Interface link state
- Port speed and duplex
- CRC and input/output errors
- Error-disabled interfaces
- Potential connectivity faults

A disconnected interface is not necessarily defective.

Meaningful physical-port validation requires a compatible test connection and, preferably, actual data traffic.

## Boot Verification

```bash
show boot
show version
dir flash:
```

These commands help validate the configured boot path, running IOS version, and available flash files.

A controlled reload from internal flash would provide stronger evidence, when authorized.

---

# Engineering Knowledge Demonstrated

This project demonstrates practical exposure to several important network infrastructure concepts.

### Network Operating-System Recovery

Using a bootloader and removable media to recover switch operating-system functionality.

### Flash Storage Management

Understanding how IOS images, configuration files, and other system data are stored and managed.

### Boot Configuration

Configuring and verifying the image the switch should load during startup.

### Secure Sanitization

Executing authorized device-reset and zeroization operations within a controlled process.

### Configuration Validation

Reviewing device settings to identify unexpected or residual configurations.

### Hardware Identification

Using Cisco IOS commands to confirm hardware model and inventory information.

### Technical Documentation

Preserving command-line evidence and explaining the purpose and outcome of each operation.

---

# Key Engineering Takeaways

1. A successful operating-system boot does not independently prove that a device has been securely sanitized.

2. The presence of an IOS image in flash does not guarantee that the switch has the correct boot variable.

3. Bootloader recovery and normal IOS administration are different operating environments.

4. Running and startup configurations must be interpreted separately.

5. Default and reserved VLANs should not be confused with customer-created VLANs.

6. Hardware capabilities must be verified against the exact switch model.

7. Technical documentation should distinguish observed results from assumptions and tests that have not been performed.

---

# Project Documentation

For a formatted technical walkthrough with terminal evidence, see:

[**Cisco Catalyst 2960-X Engineering Validation Report (PDF)**](Cisco_2960X_Engineering_Validation_Report.pdf)

---

# Repository Structure

```text
Cisco-Catalyst-2960X-Hands-On-Infrastructure-Project/
│
├── README.md
│
├── Cisco_2960X_Engineering_Validation_Report.pdf
│
├── assets/
│   ├── brandons-logo.png
│   ├── catalyst-2960x-full-switch.jpg
│   └── catalyst-2960x-sfp-uplinks.jpg
│
└── evidence/
    ├── 2960x-step1.png
    ├── 2960x-step2.png
    ├── 2960x-step3.png
    ├── 2960x-step4.png
    ├── 2960x-step5.png
    ├── 2960x-step6.png
    ├── 2960x-step7.png
    ├── 2960x-step8.png
    ├── 2960x-step9.png
    ├── 2960x-step10.png
    ├── 2960x-step11.png
    └── 2960x-step12.png
```

---

## About This Project

This project forms part of my hands-on IT infrastructure and network engineering portfolio.

My focus is developing practical skills in enterprise networking, systems administration, infrastructure troubleshooting, cloud technologies, and automation.

The project uses real physical networking equipment rather than a virtual simulator.

**Documentation disclaimer:** All terminal evidence and equipment images must be approved for public release by the equipment owner. The project does not disclose confidential internal procedures or constitute an official organizational sanitization certificate.

---

**Brandon Stevenson — IT Infrastructure & Network Engineering**

[Portfolio Website](https://brandons-resume.com) | [GitHub](https://github.com/Programmer-stevenson) | [LinkedIn](https://www.linkedin.com/in/brandon-in-tech/)
