# PASSPHRASE GENERATOR



## Overview

Passphrase Generator is a standalone [Python program](https://github.com/blankworker1/BYOB/blob/main/pp_generator.py) designed to generate a secure, 24-character alphanumeric passphrase on Raspberry Pi Zero hardware (SeedSigner) and is compatible with the BYOB protocol.

The program uses true randomness from both the operating system's cryptographically secure random generator and camera sensor noise to create high-entropy passphrases.

The passphrase is displayed in two ways:

1. QR Code (primary view, optimized for SeedSigner’s 240×240 display)


2. Text view (3 lines × 8 characters), toggleable with a button



No sensitive data is written to the SD card; all data exists only in RAM. This makes the process fully stateless and secure.

By using two separate SeedSigner devices, you can easily generate and scan passphrase QR codes, simplifying offline, air-gapped key management workflows.


---

## Features

- Generates 24-character cryptographically secure passphrases

- Combines OS randomness and camera noise for entropy

- Displays QR code in memory, no SD card storage

- Toggle passphrase text on-screen (3×8 characters)

- Optimized for SeedSigner hardware (Pi Zero + 240×240 screen + button)

- Fully offline and stateless



---

## User Flow

1. Home screen:

BYOB
PASSPHRASE
GENERATOR

Displayed for 5 seconds


2. Generating screen:

Creating 24-character random passphrase...

Automatically shown while passphrase is generated


3. Passphrase QR screen:

Displays QR code indefinitely

Press Button 1 to toggle to text view (3 lines × 8 characters)

Press again to return to QR code





---

## Hardware Requirements

- Raspberry Pi Zero (SeedSigner compatible) with 240×240 display

- Raspberry Pi Camera Module

- SeedSigner button (GPIO input)



---

## Quick Start – Build and Flash SD Card

1. Prepare the SD card:

Use Raspberry Pi OS Lite (32-bit) as base

Enable the Pi Camera in /boot/config.txt (start_x=1 and gpu_mem=128)



2. Copy script to SD card:

Clone or copy the [.py file](https://github.com/blankworker1/BYOB/blob/main/pp_generator.py) from this repository to /home/pi/ on the SD card



3. Install dependencies (offline or via pip if building the SD image before deployment):

python3 -m pip install qrcode[pil] Pillow


4. Optional: Auto-run on boot

Create a systemd service or add to .bashrc if you want the program to start automatically



5. Insert SD card into SeedSigner hardware and power on

The device will display the home screen, generate the passphrase, and show the QR code

Use Button 1 to toggle the passphrase text view





---

## Security Notes

No passphrase data is written to the SD card; all sensitive data exists only in RAM

Using two SeedSigner devices simplifies the process: one device generates the passphrase, the other scans the QR code for use in air-gapped workflows

**Disclaimer**

This software is provided for educational purposes.

BYOB base passphrase generation is critical to wallet security; users are responsible for correct usage and secure storage.


---

## References

SeedSigner Official Site – Hardware wallet interface for air-gapped offline signing

Seedsigner Fork - using this version allows the user to enter a Passphrase by scanning a QR code





---


**<<<<<< OLDER VERSION >>>>>>>>>**

---

## Purpose

The base passphrase is the primary entropy source for BYOB wallets.

Its purpose is to:

- Generate a truly random 24-character string

- Output the passphrase only as a QR code

- Avoid manual typing or memory entry

- Integrate with SeedSigner hardware for wallet creation


> Note: This generator does not create wallets or mnemonics; it only creates base passphrases.




---

## Technical Requirements

**1. Offline Operation**

Must run entirely offline

No network connectivity during generation

Generates entropy from local sources only



2. High-Quality Entropy

Use a cryptographically secure random number generator (CSPRNG), e.g. /dev/urandom on Linux

Optional entropy sources to enhance randomness:

Camera sensor noise

User input timing / button presses

System jitter or microsecond timers


Entropy sources must be mixed and hashed (SHA-256 or equivalent) before character selection



3. Passphrase Format

Length: 24 characters

Character set:

Uppercase letters: A-Z

Lowercase letters: a-z

Numbers: 0-9

Symbols: !@#$%^&*()-_=+ (avoid ambiguous symbols for engraving or scanning)


Each character selected uniformly at random from the full character set



4. Output

Display QR code only

Optional: display plaintext behind a strict warning

QR encoding must be compatible with SeedSigner fork input



5. Security Properties

Passphrase never stored on disk in plaintext

Generator must never transmit passphrase

QR can be copied to steel plate or air-gapped storage

No logs or temporary files containing passphrase





---

 ## Example Workflow

1. Boot SeedSigner with generator microSD


2. Launch generator script


3. Optional: collect extra entropy from camera / user input


4. Generate 24-character base passphrase


5. Display QR code


6. Scan QR into SeedSigner fork to create wallet


7. Securely store QR / steel plate with family


8. Power off device, discard microSD if desired




---

## Implementation Notes

Language: Python recommended (minimal dependencies)

Libraries: qrcode or similar for QR generation

Platform: Raspberry Pi (SeedSigner hardware)

Auditing: Script should be small and fully auditable by community

Open-source: Encourage peer review and reproducibility



---

## Recommended Best Practices

Verify generator firmware hash before use

Generate only one passphrase per session

Use multiple entropy sources to avoid low-entropy early boot

Do not attempt to type passphrase manually unless fully aware of risks

Store QR and steel plate securely with trusted family members



---

## Integration with BYOB

Base passphrase QR is scanned by SeedSigner fork

Combined with:

11-word mnemonic from Word Wall

Human-memorable suffix


Produces a full BIP-39 passphrase for wallet derivation


> This separation ensures:

High entropy from generator

Memorable suffix for individual recall

Split custody for family protection

Public ledger transparency for community


---

## Pi-based Generator (SeedSigner hardware) Strengths:

✅ Familiarity & trust
SeedSigner hardware is already trusted by Bitcoiners
Raspberry Pi + Linux + /dev/urandom is well understood
No “new hardware trust leap”
This matters enormously for first contact with the project.

✅ Auditability
Can be implemented as:
~100–200 lines of Python
Uses standard, well-reviewed libraries
Community reviewers can easily inspect:
Entropy sources
Hashing
Character mapping
QR output

✅ Integration simplicity
Same hardware form factor as SeedSigner
Same camera, screen, buttons
Just a different microSD image
This reinforces your two-SD-card model:
SD A: Generator
SD B: Signer
That’s a very clean mental model.

✅ Entropy story is defensible
/dev/urandom + camera noise + timing input
Explicit entropy mixing
SHA-256 or SHA-512 finalization
This is conservative, orthodox crypto engineering.

---

## Technical Specification

Purpose

The BYOB Base Passphrase Generator is a standalone, offline program that runs on SeedSigner-compatible Raspberry Pi hardware and generates a high-entropy base passphrase for use in the BYOB model.

The generator must not create wallets, keys, or mnemonics.

It produces entropy only.


---

## Target Platform

Hardware: SeedSigner-compatible Raspberry Pi (Pi Zero / Zero 2)

Boot Medium: microSD card (SD A)

Operating Mode: Fully offline / air-gapped

Networking: Disabled / not required



---

## Output Requirements

Generate a 24-character base passphrase

Character set:

A–Z a–z 0–9 !@#$%^&*()-_=+

Characters must be selected uniformly at random

Output format:

QR code only (default)

Optional plaintext display behind explicit warning




---

## Entropy Requirements (Critical)

Required entropy sources

The generator must use a cryptographically secure random number generator and must not rely on a single entropy source.

Minimum requirements:

1. OS CSPRNG

Use /dev/urandom or equivalent

Do not use language-level PRNGs (e.g. Python random)



2. Additional entropy source (at least one)

- Camera sensor noise (preferred, if available)

- User timing input (button presses, delays)

- System timing jitter


Entropy mixing

All entropy sources must be combined and hashed using:

SHA-256 or stronger


Final passphrase characters must be derived from the hashed entropy

Do not expose raw entropy directly



---

## Security Requirements

Stateless operation 

No passphrase stored on disk

No logging of sensitive data

No reuse of entropy between runs

Offline only

No network access

No USB mass storage output

Single-use session

Generate one passphrase per execution

Power-off recommended after generation




---

## QR Code Requirements

Encode the raw 24-character passphrase

QR must be:

Compatible with SeedSigner camera scanning

High contrast

No additional metadata




---

## User Interface (Minimal)

Simple, deterministic flow:

1. Boot


2. (Optional) Entropy collection step


3. Generate passphrase


4. Display QR


5. Exit / power off



Explicit warnings:

Passphrase will not be shown again

Do not photograph or transmit

This is not a wallet or private key




---

## Prohibited Behavior

The generator must not:

Derive or display:

Wallets

Private keys

Public addresses

Mnemonics


Accept user-entered phrases

Hash or transform user-created text

Use brainwallet-style derivation

Store output after display



---

## Recommended Implementation Stack

Language: Python 3

Libraries:

os / secrets (for CSPRNG access)

hashlib (SHA-256)

qrcode (QR generation)


Code size: Small, auditable (<300 LOC preferred)



---

## Verification & Testing

Developer should provide:

Deterministic test mode (non-production)

Entropy source documentation

Hash of final SD image

Reproducible build instructions



---

## Threat Model Assumptions

Attacker does not have runtime access to the device

User verifies firmware and SD image integrity

Device is air-gapped during operation



---

## Design Principles

One job only: generate entropy

Human-safe: prevent misuse or misunderstanding

Auditable: simple, readable code

Composable: integrates with SeedSigner fork via QR













