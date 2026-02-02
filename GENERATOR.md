#GENERATOR

BYOB Base Passphrase Generator is a standalone, offline tool for securely generating high-entropy base passphrases compatible with the BYOB model.

It is intended to run on air-gapped devices such as a SeedSigner Raspberry Pi.


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

Example Workflow

1. Boot SeedSigner with generator microSD


2. Launch generator script


3. Optional: collect extra entropy from camera / user input


4. Generate 24-character base passphrase


5. Display QR code


6. Scan QR into SeedSigner fork to create wallet


7. Securely store QR / steel plate with family


8. Power off device, discard microSD if desired




---

Implementation Notes

Language: Python recommended (minimal dependencies)

Libraries: qrcode or similar for QR generation

Platform: Raspberry Pi (SeedSigner hardware)

Auditing: Script should be small and fully auditable by community

Open-source: Encourage peer review and reproducibility



---

Recommended Best Practices

Verify generator firmware hash before use

Generate only one passphrase per session

Use multiple entropy sources to avoid low-entropy early boot

Do not attempt to type passphrase manually unless fully aware of risks

Store QR and steel plate securely with trusted family members



---

Integration with BYOB

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

Disclaimer

This software is provided for educational purposes.
BYOB base passphrase generation is critical to wallet security; users are responsible for correct usage and secure storage.

