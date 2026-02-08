# BYOB Guidebook

Welcome to the **BYOB Guidebook**. This manual guides you through using the WORDWALL Poster Generator, Personal Preparedness, and safety tips.

Use the table of contents to jump to any section.

---

## Contents

- [Introduction](#introduction)
- [Generator](#generator)
- [Posters](#wordwall-posters)
- [Safety Notes](#safety-notes)
- [FAQ](#faq)
- [Resources](#resources)

---

## Introduction

BYOB (**Become Your Own Bank**) is an educational project for managing and visualizing cryptographic assets safely.  
This guide helps you navigate the tools, understand how to participate, and generate posters.

---

## BYOB Zero Protocol — Layered Overview

BYOB Zero is designed for zero-risk community preparedness. Each layer builds on the previous, combining physical artifacts, private memory, and non-custodial workflows.


---

**Layer 1: Word Wall Generator**

Purpose: Create a canonical shuffled map of all 2048 BIP39 words, divided into five sheets.

Components:

Generator software hosted on GitHub (link)

BIP39 word list: bip39_words.js hosted on GitHub (link)

Community label: 4-character identifier (e.g., BOSA)


Function & Design:

1. Deterministic shuffle using SHA-256 hash + Mulberry32 PRNG.


2. Disabled cells logic ensures consistent poster layout.


3. Outputs 5 printable poster sheets with headers, row/column labels, page numbers, and the community label in the bottom-right cell.



Best Practice: Use the canonical generator and word list for reproducibility. Do not modify.


---

**Layer 2: Word Wall Poster (Physical + Digital Artifact)**

Purpose: Provide a human-readable, verifiable map of the BIP39 words for participants.

Artifacts:

Physical poster sheets: Printed from the generator PNG files.

Digital poster files: Stored in GitHub for public reference.


Function & Design:

First 4 letters of each BIP39 word are displayed for memorization.

Grid: 5 sheets × 21 rows × 20 columns.

Bottom-right cell contains community label.

Optional: SHA-256 hash of canonical seed displayed for verification.


Best Practice: Keep posters in safe, visible workshop locations. The digital version allows verification without touching the physical poster.


---

**Layer 3: Coordinator (Private Memory Layer)**

Purpose: Provide a personalized, non-custodial memory system for participants to regenerate wallets.

Components:

Coordinator: Sheet + row + column starting point in Word Wall.

Pattern: Stepwise traversal pattern to select 11 words.

Full Passphrase selection: Two extra words from specific cells to complete the deterministic process.


Best Practices:

1. Keep private — do not record online.


2. Patterns: Use easy-to-remember shapes (e.g., zig-zag, diagonal).


3. Store all words as positions in the Word Wall (not as words themselves) to reduce attack risk.




---

**Layer 4: Seedsigner Workflow**

Purpose: Turn the Word Wall memory into a deterministic wallet with verifiable public outputs.

Steps:

1. Enter first 11 words using Coordinator + Pattern.


2. Generate 12th word using deterministic checksum (0000000).


3. Add Passphrase. Enter both four letter words from Word Wall in correct order.

5. Output:

Public address QR code

Wallet fingerprint


Best Practices:

Always confirm fingerprints.

Keep private memory secure — do not store Coordinator or Pattern online.

Use PSBT and BlueWallet for spend operations to avoid leaking private keys.



---

**Layer 5: Public Ledger Website**

Purpose: Provide transparent verification of public addresses without connecting wallets to legal identities.

Components:

Public ledger page on BYOB site.

Fields: Public address QR + fingerprint + optional BYOB cardname.

Best Practices:

Only upload public outputs.

No personal identifiers.

Verify QR/fingerprint match before publishing.



---

**Summary Table: BYOB Zero Layers**

Layer	Purpose	Key Artifact	Privacy / Security Notes

1	    Generate canonical map	Generator software + BIP39 word list + label	          Open-source, public

2	    Physical/digital poster	5 Word Wall sheets	Safe to share publicly

3	    Private memory	Coordinator start + pattern + suffix	Keep secret

4	    Wallet creation	Seedsigner wallet, fingerprint, public address
      Fingerprint optional to share; private keys remain offline

5	    Public ledger	Online QR + fingerprint registry	Anonymous verification,         no personal data



---

This gives you a complete, structured BYOB Zero protocol, from canonical Word Wall generation to public verification of wallets, with privacy preserved at every layer.


---

### Appendix #1 - Word Wall Workshop Guide

This guide shows you how to create a BYOB Zero Word Wall for your community. It is designed for anyone, with no technical knowledge required.

---

**Key Terms (Glossary):**


**Word Wall:** printed grid of the first four letters of all 2048 BIP39 words,shuffled into a reproducible layout.

**Community Label:** 4-character name for your community that ensures your wall is unique.

**Canonical Files:** BYOB Zero official generator software and word list,hosted online. Using these ensures your wall is consistent and verifiable.

**Poster Sheet:** One page of the Word Wall. There are 5 sheets in total.

**Bottom-Right Label:** Your community label printed in the bottom-right corner of each sheet.



---

**Step 1: What You Need**

Equipment / Materials:

A modern web browser (Chrome, Firefox, Edge, or Safari) on a laptop, tablet, or desktop.

Internet access for the first step to open the generator page.

A 4-character community label (e.g., BOSA) to identify your community.

Optional: Printer for producing posters.



---

**Step 2: Open the Word Wall Generator** 

1. Open your web browser.


2. Go to the generator page: https://blankworker1.github.io/BYOB/ww_generator.html


3. The generator will load automatically. All necessary files (word list, software, layout) are included on the page.


4. No installation or downloads are needed — it runs entirely in your browser.




---

**Step 3: Enter Your Community Label**

1. Locate the field labeled “Community Label (max 4 chars)” at the top of the page.


2. Type your 4-character community name (for example, BOSA).


3. This label will appear in the bottom-right corner of each poster sheet.


4. Keep it short and uppercase for maximum visibility.




---

**Step 4: Generate the Seed**

1. Click the “Generate Word Wall” button.


2. A Seed string will appear on the page — this is used internally to shuffle the words deterministically.


3. You do not need to understand the seed — it guarantees your wall is reproducible.




---

**Step 5: Download Posters**

1. Click the “Download 5 Sheet Posters” button.


2. Five PNG files will download automatically, one for each Word Wall sheet.


3. Each sheet includes:

- Page number in the top-left corner

- Row and column headers

- Grid of 20 x 21 shuffled BIP39 words (first four letters)

- Community name label in the bottom-right corner


![Example Poster](Sheet1_BYOB_ZERO_WORDWALL_BOSA_V1.png)


---

**Step 6: Print and Use Your Word Wall**

Print the posters at your desired size (A1 is recommended for workshops).

Use them for:

- Personal preparedness activities
  
- Community verification exercises

- BYOB Workshops



---

**Step 7: Verify Reproducibility**

1. The Word Wall is deterministic, meaning anyone using the same generator software, word list, and community label will get the exact same layout.


2. This ensures:

Consistency across workshops

Easy verification by other community members



---

**Tips for Success**

- Do not change the community label after generating the wall — it will alter     the layout.

- All other settings and code are handled automatically on the website — no       need to touch them.

- Short, uppercase labels (4 characters) are easiest to read on printed sheets.

- Keep your printed posters safe if they are used for official community          exercises.



---

By following this guide, you will have a fully reproducible BYOB Zero Word Wall, ready to display in workshops or use in your community. Each wall is unique to your community label but can be regenerated at any time for verification.




---

## Safety Notes

- BYOB is **not a bank** and holds **no funds**.
- You can share the WORDWALL publicly as it contains no sensitive info.

---

## FAQ

**Q: Can I use the same name twice?**  
A: Yes, the generator is deterministic, but seeds will be the same if the community name matches the name on the bottom right corner of the Word Wall.

**Q: Can I edit the poster after download?**  
A: You can annotate manually, but changing BIP39 words breaks determinism.

---

## Resources

- [BYOB - GitHub](https://github.com/blankworker1/BYOB)
- [BIP39 Word List](https://github.com/bitcoin/bips/blob/master/bip-0039/english.txt)


---








