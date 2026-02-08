# BYOB Guidebook

Welcome to the **BYOB Guidebook**. This manual guides you through using the Wall ID Generator, Posters, and safety tips.  
Use the table of contents to jump to any section.

---

## Contents

- [Introduction](#introduction)
- [WW_GENERATOR](#ww_generator)
- [Wall ID Posters](#wall-id-posters)
- [Safety Notes](#safety-notes)
- [FAQ](#faq)
- [Resources](#resources)

---

## Introduction

BYOB (**Become Your Own Bank**) is an educational project for managing and visualizing cryptographic assets safely.  
This guide helps you navigate the tools, understand your Wall ID, and generate posters.

---

## WW_GENERATOR

The **WW_GENERATOR** page lets you:

- Generate a deterministic Wall ID using your name and date.
- Copy your Wall ID to clipboard.
- Download multi-sheet posters with unique BIP39 words.

### Steps:

1. Enter your **Wall ID** (max 4 characters).
2. Click **Generate Seed**.
3. Copy your seed or download the posters.

---

## Wall ID Posters

The **posters** are multi-page sheets:

- Each page has a numbered grid of words.
- The bottom-right cell always shows your Wall ID.
- Certain columns are reserved (98–100) for security and formatting.
- Keep your posters safe—they reproduce the same Wall ID.

![Example Poster](poster_example.png)

---

## Safety Notes

- BYOB is **not a bank** and holds **no funds**.
- Always **store seeds offline**.
- Never share your Wall ID publicly if it contains sensitive info.
- Loss of seed = loss of ability to reproduce your poster.

---

## FAQ

**Q: Can I use the same name twice?**  
A: Yes, the generator is deterministic, but seeds will be the same if the name and date match.

**Q: Can I edit the poster after download?**  
A: You can annotate manually, but changing BIP39 words breaks determinism.

---

## Resources

- [BYOB Project GitHub](https://github.com/your-repo)
- [BIP39 Word List](https://github.com/bitcoin/bips/blob/master/bip-0039/english.txt)
- [Markdown Guide](https://www.markdownguide.org/)

---
