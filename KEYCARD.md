#KEYCARD

The KEYCARD is a credit‑card sized stainless steel plate used in the BYOB model to securely store the base passphrase and wallet fingerprint.

It is designed for long‑term, offline, physical durability, and for shared family or group custody.


---

Purpose

The KEYCARD stores only what should never be memorized, and nothing else.

It exists to:

Preserve the base passphrase in a durable medium

Provide a wallet fingerprint for verification

Support wallet recovery without storing seed words

Enable split custody between individual and family


The SEEDCARD does not contain enough information to access a wallet on its own.


---

What Is Engraved on a SEEDCARD

Required Fields

1. Base Passphrase

24 random characters

Generated offline

Engraved exactly as generated

Not reused across wallets



2. Wallet Fingerprint

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

Physical Specifications

Material: Stainless steel

Size: Credit‑card format

Engraving: Deep mechanical engraving

Designed to resist:

Fire

Water

Corrosion

Long‑term wear




---

Engraving Process

Overview

SEEDCARD engraving is performed using a SeedHammer engraving machine with an offline controller.

The controller operates similarly to a SeedSigner device.


---

Step‑by‑Step Process

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

SEEDCARD is stored by family or trusted group member





---

Security Model

What the SEEDCARD Protects Against

Loss of digital devices

Forgotten base passphrase

Fire or water damage

Single‑point digital failure



---

What the SEEDCARD Cannot Do Alone

Generate seed words

Reveal Word Wall positions

Reveal direction or suffix

Spend funds


Possession of a SEEDCARD does not grant wallet access.


---

Role in Wallet Recovery

To regenerate a wallet:

1. Individual provides:

Word Wall position

Direction

Passphrase suffix



2. Family provides:

SEEDCARD (base passphrase + fingerprint)



3. SeedSigner recreates wallet offline


4. Fingerprint is verified against SEEDCARD


5. Wallet access is confirmed




---

Best Practices

Use one SEEDCARD per wallet

Do not store multiple wallets on one plate

Store in separate secure locations if desired

Periodically verify fingerprint against regenerated wallet

Do not photograph or digitize the engraved text unnecessarily



---

Threat Model Notes

Theft of SEEDCARD alone is insufficient to access funds

Family custody is intentional and explicit

BYOB does not attempt to prevent betrayal by trusted parties


Trust decisions are part of self‑custody.


---

Relationship to Other BYOB Components

Component	Role

Word Wall	Public mnemonic reference
Generator	Creates high‑entropy base passphrase
SeedSigner	Wallet creation and recovery
SEEDCARD	Durable passphrase + fingerprint storage
Public Ledger	Public address visibility



---

Disclaimer

The SEEDCARD is a storage aid, not a wallet.

Loss, theft, or misuse can result in permanent loss of funds.

Use at your own risk.


---
