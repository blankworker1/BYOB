# WORDWALL 

**BYOB Word Wall**

The Word Wall is the cornerstone of the BYOB model. It provides a canonical, public reference for all 2048 BIP‑39 English words, enabling deterministic wallet reconstruction without storing full seed phrases.


---

## Purpose

Acts as shared community memory for mnemonic word derivation

Provides canonical positions for the 11-word mnemonic in BYOB

Ensures everyone in the community uses the same reference table

Supports physical, digital, and mirrored implementations


> The Word Wall contains words only — no wallet information, no secrets, no passphrases.




---

## 1. Physical Word Wall

Description

A permanent installation (engraved wall, stone blocks, or similar)

Each word occupies a fixed row and column position

Designed to be visually and physically accessible to the community


Purpose

Acts as a ceremonial verification of the canonical word table

Allows participants to verify the official reference during wallet generation days

Provides a resilient, offline backup independent of any hardware or digital system


Best Practices

Engrave words deeply to resist wear

Include row/column markers for easy referencing

Perform a community verification ceremony upon completion

Photograph and store as a digital “wallshot” for redundancy



---

## 2. Digital Word Wall

Description

A digital mirror of the canonical Word Wall

Hosted on:

GitHub repository

IPFS (content-addressed storage)

Community website



Purpose

Provides wide access to the canonical table

Enables participants to verify positions before wallet generation

Serves as a reference for air-gapped wallets and software tools


Best Practices

Maintain version control (GitHub) for transparency

Store a hash or checksum of the Word Wall for integrity verification

Encourage multiple mirrors to ensure redundancy

Use the digital version for offline printing or offline software usage



---

## Usage in Wallet Creation

1. Individual chooses a starting position and direction on the Word Wall


2. The next 11 words are selected sequentially according to the chosen direction


3. The 12th checksum word is calculated automatically by SeedSigner fork


4. The 11-word mnemonic, combined with the base passphrase + suffix, produces the wallet



> No wallet data or private keys are ever stored on the Word Wall.




---

## Community Responsibilities

Maintain physical integrity of the Word Wall

Mirror digital versions on multiple platforms

Educate participants on:

How to reference words

How to choose starting positions and directions


Ensure canonical consistency across physical and digital forms



---

## Security Notes

The Word Wall is completely safe to be public

No secrets are stored in the wall itself

Security relies on:

Individual memory of position/direction/suffix

Family custody of the base passphrase




---

Suggested References

Physical: stone/engraved wall, permanent markers for row/column

Digital: GitHub repository, IPFS hash, public website mirror


> The Word Wall is the foundation of BYOB. Its accuracy and permanence are critical to the community’s ability to regenerate wallets.








