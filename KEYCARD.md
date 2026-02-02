#KEYCARD

The KEYCARD is a credit‑card sized stainless steel plate used in the BYOB model to securely store the base passphrase and wallet fingerprint.

It is designed for long‑term, offline, physical durability, and for shared family or group custody.


---

## Purpose

The KEYCARD stores only what should never be memorized, and nothing else.

It exists to:

Preserve the base passphrase in a durable medium

Provide a wallet fingerprint for verification

Support wallet recovery without storing seed words

Enable split custody between individual and family


The KEYCARD does not contain enough information to access a wallet on its own.


---

## What Is Engraved on a KEYCARD

Required Fields

**1. Base Passphrase**

24 random characters

Generated offline

Engraved exactly as generated

Not reused across wallets



**2. Wallet Fingerprint**

BIP‑32 master fingerprint (e.g. A1B2C3D4)

Used to verify correct wallet regeneration

Not a private key




What Is NOT Engraved

❌ Mnemonic words

❌ Word Wall positions

❌ Directions

❌ Passphrase suffix

❌ Private keys

❌ Public addresses



---

## Physical Specifications

Material: Stainless steel

Size: Credit‑card format

Engraving: Deep mechanical engraving

Designed to resist:

Fire

Water

Corrosion

Long‑term wear




---

## Engraving Process

**Overview**

KEYCARD engraving is performed using a SeedHammer engraving machine with an offline controller.

The SeedHammer controller is a stateless device and operates similarly to a SeedSigner device.


---

**Step‑by‑Step Process**

1. Generate Base Passphrase

Offline generator produces a 24‑character string

Output is displayed as a QR code



2. Create Wallet (Optional Timing)

Wallet may be created before or after engraving

Wallet fingerprint is known at this stage



3. Prepare Engraving Data

QR code contains:

Base passphrase

Wallet fingerprint


No mnemonic data included



4. Offline Scanning

SeedHammer controller scans QR code

No network connectivity is used



5. Engraving

Machine engraves passphrase and fingerprint onto steel plate

No digital copy is stored after engraving



6. Verification

Human visual inspection

Optional QR re‑scan test (if supported)



7. Secure Storage

KEYCARD is stored by family or trusted group member





---

## Security Model

What the KEYCARD Protects Against

Loss of digital devices

Forgotten base passphrase

Fire or water damage

Single‑point digital failure



---

## What the KEYCARD Cannot Do Alone

Generate seed words

Reveal Word Wall positions

Reveal direction or suffix

Spend funds


Possession of a KEYCARD does not grant wallet access.


---

## Role in Wallet Recovery

To regenerate a wallet:

1. Individual provides:

Word Wall position

Direction

Passphrase suffix



2. Family provides:

KEYCARD (base passphrase + fingerprint)



3. SeedSigner recreates wallet offline


4. Fingerprint is verified against KEYCARD


5. Wallet access is confirmed




---

## Best Practices

Create one KEYCARD per wallet

Do not store multiple wallets on one plate

Store in separate secure locations if desired

Periodically verify fingerprint against regenerated wallet

Do not photograph or digitize the engraved text unnecessarily



---

## Threat Model Notes

Theft of KEYCARD alone is insufficient to access funds

Family custody is intentional and explicit

BYOB does not attempt to prevent betrayal by trusted parties


Trust decisions are part of self‑custody.


---

## Relationship to Other BYOB Components

```
Component	        Role

Word Wall	        Public mnemonic reference

Generator	        Creates high‑entropy base passphrase

SeedSigner	      Wallet creation and recovery

KEYCARD	          Durable passphrase + fingerprint storage

Public Ledger	    Public wallet address visibility

```

---

**Disclaimer**

The KEYCARD is a storage aid, not a wallet.

Loss, theft, or misuse can result in permanent loss of funds.

Use at your own risk.


Here’s a short, clear technical description of the SeedHammer engraving machine suitable for your Engraving Process Overview section. You can paste this into your documentation under the SEEDCARD or engraving process.


---

## SeedHammer Engraving Machine — Technical Overview

The SeedHammer is a dedicated metal engraving machine designed specifically for securely backing up Bitcoin wallet secrets — including seeds, QR representations, and related identifiers — onto durable stainless steel plates in an air‑gapped, mechanical process. 

**Purpose**

SeedHammer was created to solve the challenge of producing robust, long‑lasting metal backups of wallet data. Standard manual metal marking methods are slow, imprecise, or error‑prone; SeedHammer automates engraving in a way that is repeatable, auditable, and suitable for high‑value self‑custody use cases. 

**Hardware and Architecture**

Engraving Machine: Heavy‑duty mechanical engraver capable of hammering or cutting into stainless steel backup plates. 

Controller: A small embedded controller based on the same hardware platform as SeedSigner (typically a Raspberry Pi Zero with integrated display and camera). 

Air‑gapped Operation: The controller runs entirely offline with no network access, minimizing risk of data leakage. 

Input: Controller software accepts wallet data (e.g., seeds or passphrase QRs) via offline scanning or manual entry. 

Output: The machine engraves text and QR codes directly onto steel plates (e.g., 316L 3 mm plates), producing permanent backups that resist corrosion, fire, and time. 


**Controller Software**

The SeedHammer controller software runs on offline hardware (Raspberry Pi Zero) with a small LCD display and camera module. It guides the user through the engraving process, shows instructions, and ensures the correct data is encoded into the plates before engraving begins. 




**Security Features**

Air‑gapped: No wireless or network connectivity to reduce exfiltration risk. 

Stateless: Controller does not permanently store private data; users can remove the SD card before entering sensitive information. 

Constant‑time engraving: The engraving process is designed so that each character’s engraving sound and timing are uniform, mitigating side‑channel leakage of secret content. 

QR Codes: Engraved QR representations make recovery easier and reduce transcription mistakes. 



**Typical Usage**

1. Prepare plates: Install the correct steel plates for engraving. 


2. Load controller: Boot the offline controller loaded with the SeedHammer software. 


3. Scan data: Use the camera to scan QR or manually enter engraving data. 


4. Engrave: Start engraving; the machine permanently marks the plate with the encoded information. 


5. Verify: Inspect engraving visually or via QR re‑scan where supported. 




**Role in BYOB**

In the BYOB workflow, SeedHammer (or a similar controlled engraving system) is used to engrave the base passphrase and wallet fingerprint onto a KEYCARD — a credit‑card–sized stainless plate. This physical backup is held in family custody as part of the recovery process. SeedHammer’s offline, uniform, and durable engraving workflow makes such plates safe to rely on for long‑term preparedness.



**References**

SeedHammer was created for automated, air‑gapped, stateless metal backups of Bitcoin wallets, solving gaps in existing metal backup options. 

The device supports engraving text and QR codes for both singlesig and multisig wallets and uses a dedicated controller. 

The controller runs on Raspberry Pi Zero hardware with open‑source software, enabling offline use. 

Engraving processes are designed with security in mind, including constant‑time operation to minimize side‑channel risk. 











