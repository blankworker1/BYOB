# BYOB

**Become Your Own Bank**

BYOB is an open, decentralized community protocol for financial preparedness using self custodial Bitcoin wallets.

It enables individuals to generate and recreate a Bitcoin wallet from memory and public infrastructure, without storing seed words, without relying on a personal hardware device, and without trusting any central authority.

BYOB is a protocol, not a product.


---

## Core Idea

A Bitcoin wallet can be recreated using:

- A public, canonical Word Wall (shared by the community)

- A small amount of private individual memory

- A family‑held base passphrase

- An offline signing device


No single person or system can access funds alone.


---

## System Components

**1. Word Wall (Public, Canonical)**

A fixed table of all 2048 BIP‑39 English words, each with a permanent public position.

Exists as:

Physical installation (engraved wall / stone)

Digital mirror (website, GitHub, IPFS “wallshot”)


Contains no secrets

Can be replicated by any community


The Word Wall is shared memory, not private data.


---

**2. Individual Memory (Private)**

Each individual memorizes three things only:

1. Starting position on the Word Wall


2. Direction / traversal rule that produces 11 words


3. Passphrase suffix (short, human‑memorable)


> The 12th mnemonic word is generated automatically via the BIP‑39 checksum.



Individuals do not store seed words or private keys.


---

**3. Base Passphrase (Family / Group Custody)**

The base passphrase is the main entropy source in BYOB.

Specification:

Length: 24 characters

Generated randomly offline

Character set: uppercase, lowercase, numbers, symbols

Stored as:

Engraved steel plate and/or

QR code



The base passphrase is:

Not memorized

Not reused

Not typed manually


Family or trusted group members hold this base passphrase.

They cannot access the wallet alone.


---

**4. SeedSigner (Offline Device)**

SeedSigner (or compatible open‑source device) is used to:

Enter 11 mnemonic words (12th is computed)

Scan the base passphrase QR

Append the individual’s suffix

Derive:

Wallet fingerprint

Public address



A forked version may allow QR‑based passphrase entry for usability.


---

**5. Public Ledger (Website)**

A public website lists:

Wallet public addresses

QR codes for receiving funds


This ledger:

Contains no private information

Does not grant spending access

Exists for transparency and preparedness


Funds can be sent to these addresses at any time.


---

## Complete Workflow (End‑to‑End)

**Step 1 — Word Wall Setup**

Community publishes a canonical Word Wall using WW GENERATOR webapp.

Physical installation is ceremonially verified

Digital “wallshot” is mirrored on:

Website

GitHub

IPFS


**Step 2 — Choose Memory Pattern**

Each individual chooses:

Starting Word Wall position

Direction / traversal rule (11 words)


This information is never shared publicly.


---

**Step 3 — Generate Base Passphrase**

Each individual boots their copy of Passphrase Generator from a micro SD card on SeedSigner hardware.

Offline software:

Collects entropy

Generates a 30 character random string

Displays it as a QR code (with text option)


Base passphrase is:

Engraved on steel using SeedHammer engraving machine 

Stored as a QR image file by family/group



No wallet exists yet.


---

**Step 3 — Suffix**

Each individual chooses:

Suffix - a short password type text (letters and numbers)


This is 


---

**Step 4 — Wallet Creation**

Each individual uses SeedSigner hardware (running Earthdiver version) to create an offline single sig wallet with passphrase:

1. Select TOOLS - Calculate 12th/24th word
   
2. Enter 11 mnemonic words manually (from Wordwall)

3. Calculate 12th checksum word using dice rolls: 0000000

4. Memorize 12th word (dont write it down)

5. Scan base passphrase QR (generated on second Seedsigner device)

6. Edit and add Suffix manually

7. Wallet fingerprint is displayed

8. Public address is generated




---

**Step 5 — Verification & Registration**

Individual verifies:

Wallet fingerprint

Public address


Public address QR is added to the public ledger website 


Funds are not required at this stage.


---

## Wallet Regeneration

To regenerate the wallet:

1. Individual recalls:

- Word Wall starting position and direction 

- Passphrase Suffix (8 characters)


2. Family provides base passphrase (QR or plate)


3. SeedSigner recreates the wallet offline


4. Fingerprint is verified


5. Funds are accessible


No stored seed phrase is required.


---

## Roles & Responsibilities

**Individual**

Memorize position, direction, and suffix

Practice recovery periodically

Verify fingerprints and addresses


**Family / Group**

Secure base passphrase

Provide recovery support

Verify fingerprint correctness

Cannot access wallet alone


**Community**

Maintain Word Wall

Mirror canonical data

Host public ledger

Educate participants

Run wallet creation / recovery days



---

## Security Model

Public components are safe because they contain no secrets

Private control is split between:

- Memory

- Family custody

- Offline tools


No central recovery authority exists


Loss is possible. This is honest self‑custody.


---

## Scope & Philosophy

BYOB is about preparedness, not guarantees.

Edge cases such as death or incapacitation are left to individual trust decisions. If you do not trust your family or group, you should not store value you are unwilling to lose.


---

## Why BYOB Exists

Most self‑custody systems assume:

Perfect memory

Perfect discipline

Always‑available hardware


BYOB assumes:

Humans forget

Devices fail

Communities endure



---

## Open & Forkable

BYOB is:

Open source

Non‑custodial

Non‑authoritative


Anyone can replicate or modify the model.

There is no official instance.


---

**Disclaimer**

BYOB is an educational and preparedness framework.
It is not financial advice and not a custody service.

You are responsible for your own decisions.











