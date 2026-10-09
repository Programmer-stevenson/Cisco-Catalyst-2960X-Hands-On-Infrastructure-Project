Cisco Catalyst 2960-X | Hands-On Physical Network Infrastructure Project
Secure Sanitization · IOS Recovery · Boot Configuration · Post-Recovery Validation
![Brandon Stevenson — IT Professional](assets/brandons-logo.png)
Physical equipment gallery
> **Image disclosure:** These are realistic, background-edited presentation photographs of the actual Catalyst 2960-X switch. The office surroundings were generated/changed for a consistent visual presentation. They are **not unmodified documentary evidence** of the workbench, workflow, or commands. The unaltered nature of the hardware in a photograph should not be interpreted as independent verification of its internal state; see the terminal captures below for CLI evidence.
![Cisco Catalyst 2960-X physical switch, office-desk presentation photograph](assets/catalyst-2960x-full-switch.jpg)
Full chassis view — physical Catalyst 2960-X bench hardware.
![Close-up of the Catalyst 2960-X SFP uplink area](assets/catalyst-2960x-sfp-uplinks.jpg)
Front-right close-up — Ethernet access ports and SFP uplink area. Appearance photos are illustrative, not proof of tested port health.
Platform: Cisco Catalyst WS-C2960X-24TS-L  
Environment: Offline/isolated bench switch  
Console: Tera Term serial terminal  
Scope: Hands-on physical device servicing, observed sanitization commands, USB-to-flash IOS recovery, boot-variable configuration, and selected post-recovery verification. No production network was connected.
> **Evidence and authorization:** This portfolio write-up is an original explanation of selected operations recorded in the supplied console screenshots, **not** an EPC/company SOP, authorized customer wipe certificate, or production deployment record. Follow approved workplace controls for device operations and publication. Serial numbers in attached screenshots are blurred; review all material for other organizational restrictions before posting publicly.
Executive summary
This project documents a recovery workflow on a Cisco Catalyst 2960-X access switch. The captured sequence includes bootloader flash handling, USB-based IOS boot, secure reset and FIPS zeroization prompts, copying the specified 2960-X IOS image to internal flash, setting a boot variable, and inspecting startup/running configuration, VLAN and VTP state, and hardware inventory. The images do not show a complete port-health test, independent post-reload flash boot, trusted checksum comparison, or formal sanitization audit signoff. Those are recorded as not evidenced, not passed.
Observed architecture
`Approved USB media → 2960-X bootloader → IOS boot → Secure reset / zeroization → IOS recovery into flash → Persistent boot setting → CLI verification`
Evidence-based walkthrough
#	Stage	Recorded command(s)	Evidence
01	Flash formatting and directory check	`format flash:; dir flash:`	Step 1
02	Booting approved IOS from USB	`boot usbflash0:c2960x-universalk9-mz.152-7.E11.bin`	Step 2
03	Secure factory-reset invocation	`factory-reset all secure`	Step 3
04	FIPS zeroization procedure	`conf t; fips zeroize`	Step 4
05	Bootloader environment inspection	`set; dir usbflash0:`	Step 5
06	USB boot after zeroization	`boot usbflash0:c2960x-universalk9-mz.152-7.E11.bin`	Step 6
07	Flash recovery and persistent boot configuration	`copy usbflash0: flash:; boot system flash:...; copy run start; show boot`	Step 7
08	Running configuration inspection	`show run`	Step 8
09	Startup configuration inspection	`show start`	Step 9
10	VLAN database inspection	`show vlan`	Step 10
11	VTP and PoE capability check	`show vtp status; show power inline`	Step 11
12	Device identity validation	`show inv`	Step 12
Step 01 — Flash formatting and directory check
Observed commands: `format flash:; dir flash:`
The visible terminal output shows flash formatting and a follow-up directory listing with minimal occupied space. This is one phase of the prescribed removal workflow, not standalone proof of irreversible sanitization.
![Step 1: Flash formatting and directory check](evidence/2960x-step1.png)
Step 02 — Booting approved IOS from USB
Observed commands: `boot usbflash0:c2960x-universalk9-mz.152-7.E11.bin`
The bootloader loads the selected 2960-X image from removable storage and displays image-verification output. This does not independently verify the origin of the USB image.
![Step 2: Booting approved IOS from USB](evidence/2960x-step2.png)
Step 03 — Secure factory-reset invocation
Observed commands: `factory-reset all secure`
The console displays the destructive reset confirmation and list of categories targeted for removal. The screenshot does not by itself establish the final complete outcome.
![Step 3: Secure factory-reset invocation](evidence/2960x-step3.png)
Step 04 — FIPS zeroization procedure
Observed commands: `conf t; fips zeroize`
The irreversible zeroization prompts and restart-related output are visible. This is part of the controlled sanitization sequence.
![Step 4: FIPS zeroization procedure](evidence/2960x-step4.png)
Step 05 — Bootloader environment inspection
Observed commands: `set; dir usbflash0:`
The bootloader environment and USB directory were inspected. Serial-number fields in the supplied evidence have been blurred.
![Step 5: Bootloader environment inspection](evidence/2960x-step5.png)
Step 06 — USB boot after zeroization
Observed commands: `boot usbflash0:c2960x-universalk9-mz.152-7.E11.bin`
The specified image was booted again and console output includes image verification activity.
![Step 6: USB boot after zeroization](evidence/2960x-step6.png)
Step 07 — Flash recovery and persistent boot configuration
Observed commands: `copy usbflash0: flash:; boot system flash:...; copy run start; show boot`
Console capture shows the image copied into flash, the boot statement configured, the running configuration copied to startup configuration, and the BOOT path pointing to the internal image.
![Step 7: Flash recovery and persistent boot configuration](evidence/2960x-step7.png)
Step 08 — Running configuration inspection
Observed commands: `show run`
The screenshot displays the start of the running configuration, including switch identification and IOS configuration lines. A partial screenshot is not evidence of all configuration fields.
![Step 8: Running configuration inspection](evidence/2960x-step8.png)
Step 09 — Startup configuration inspection
Observed commands: `show start`
The screenshot displays the start of the startup configuration. The presence of a saved configuration is intentional in the pictured boot setup and must be reconciled with the required delivery state.
![Step 9: Startup configuration inspection](evidence/2960x-step9.png)
Step 10 — VLAN database inspection
Observed commands: `show vlan`
The output lists VLAN 1 and reserved default VLANs. No custom VLAN is visible in the displayed portion.
![Step 10: VLAN database inspection](evidence/2960x-step10.png)
Step 11 — VTP and PoE capability check
Observed commands: `show vtp status; show power inline`
The capture shows VTP parameters and PoE output. This 24TS-L model is non-PoE; n/a output is expected, not a failed PoE test.
![Step 11: VTP and PoE capability check](evidence/2960x-step11.png)
Step 12 — Device identity validation
Observed commands: `show inv`
Inventory output identifies the WS-C2960X-24TS-L model and version. Its serial number has been blurred.
![Step 12: Device identity validation](evidence/2960x-step12.png)
Validation matrix
Requirement	Status supported by supplied captures	Notes
Flash format executed	Observed	Console displays format and directory check.
USB image booted	Observed	Image verification/boot output shown.
Secure reset and FIPS zeroization invoked	Observed	Complete security audit result requires separate approved evidence.
Specified IOS image copied to internal flash	Observed	Copy output captured.
Boot variable points to flash IOS image	Observed	`show boot` displayed.
Running/startup configuration inspected	Observed (partial)	Screenshots show portions of both.
VLAN / VTP status inspected	Observed	No custom VLAN visible in captured segment.
Hardware inventory inspected	Observed	Model confirmed, serial obscured.
Trusted IOS checksum comparison	Not evidenced	Image signature output is visible during boot, but a trusted post-copy checksum comparison is not shown.
Independent boot from internal flash with USB removed	Not evidenced	Could be added with explicit authorization.
Port-by-port link, traffic and error testing	Not evidenced	No port test results in supplied screenshots.
Secure-sanitization audit acceptance	Not evidenced	Requires the controlled organizational audit record.
Suggested next authorized QA (not claimed as completed)
Read-only: `show version`, `show boot`, `show env all`, `show interfaces status`, `show interfaces counters errors`, `show interfaces status err-disabled`, `show logging`.
If approved, use known-good isolated endpoints to test copper ports; document link speed/duplex, traffic forwarding, and error-counter deltas. SFP uplinks require compatible transceivers and a test peer. Do not mark untested ports as passing. Additional configuration must be removed using the approved final-state workflow.
Engineering lessons
Sanitization and operability are separate outcomes. Hardware can boot normally without all prior data having been securely removed.
An IOS image in flash is not enough. Boot state must reference the intended image, and independent reboot testing provides stronger confirmation.
CLI screenshots document observations, not assumptions. A partial `show run` screenshot does not prove the entire startup/running configuration is clean.
Persistent boot setup can leave a startup configuration. Final handoff criteria should explicitly define whether the boot setting is required and whether startup configuration is acceptable.
Device-specific constraints matter. WS-C2960X-24TS-L is an access switch without PoE, so PoE `n/a` output is expected.
Why this is a hands-on infrastructure project
This work used a physical Cisco Catalyst WS-C2960X-24TS-L, a serial-console session, and removable USB media rather than a network simulator. The core activity was low-level device recovery and validation, not production switching design. The evidence links below let a reviewer distinguish commands performed, outcomes actually shown, and tests that would require further verification.
Evidence and publication boundaries
Terminal captures document specific CLI operations. Sanitized console photographs are primary evidence; the two edited equipment images above are presentation assets only.
Successful sanitation must be established by the organization's controlled acceptance process; this repository is not a wipe certificate.
Hardware readiness is limited to the checks actually shown. No all-port link/traffic qualification or independent boot-from-flash result is claimed.
Confidentiality: Screenshots are serial-redacted, but publication still requires equipment-owner authorization and a separate check for MAC addresses, internal domains, asset tags, credentials, and customer data.
Repository layout
```text
README.md
Cisco_2960X_Engineering_Validation_Report.pdf
assets/brandons-logo.png
assets/catalyst-2960x-full-switch.jpg
assets/catalyst-2960x-sfp-uplinks.jpg
evidence/2960x-step1.png ... 2960x-step12.png
```
Security/privacy: Do not publish confidential internal procedures, customer settings or unauthorised employer imagery. Serial-number blur is not sufficient to authorize external publication.
