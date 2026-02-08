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

## BYOB Zero

(overview)

---

### Word Wall Workshop Guide

This guide shows you how to create a BYOB Zero Word Wall for your community. It is designed for anyone, with no technical knowledge required.

---

**Key Terms (Glossary):**

```

Term      	       Meaning

Word Wall	       A printed grid of the first four letters of all 2048 BIP39                      words, shuffled into a reproducible layout.

Community Label	   A 4-character name for your community that ensures your wall                    is unique.

Canonical Files	   BYOB Zero’s official generator software and word list,                          hosted online. Using these ensures your wall is consistent                      and verifiable.

Poster Sheet	   One page of the Word Wall. There are 5 sheets in total.

Bottom-Right Label Your community label printed in the bottom-right corner of                      each sheet.

```

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








