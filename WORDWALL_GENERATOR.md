# WORDWALL GENERATOR

---

Technical Overview: Deterministic Multi-Sheet BIP39 Poster Generator

1. Purpose and Goals

This web-based tool is designed to:

1. Generate a deterministic shuffle of the BIP39 2048-word list using a user-defined seed.


2. Split the resulting 21 x 98 word grid into five separate A1 landscape sheets for poster-quality printing.


3. Preserve headers, colors, grid lines, and sequential row/column data.


4. Include a footer with the seed, date, and name on each sheet.


5. Export each sheet as a high-resolution PNG with automatic, descriptive file names.



This ensures reproducible grids that can be shared or printed for offline use, while remaining fully client-side (browser-based).


---

2. User Inputs

1. Name Input

Free text input (<input> HTML element)

Used as part of the seed.



2. Date

Automatically taken from the user’s system date.



3. Seed Generation

Seed string format:

become-your-own-bank[DD/MM/YYYY][name]

Used to deterministically shuffle the 2048 word list.





---

3. Deterministic Shuffle

Algorithm Overview:

1. Seed Hashing

Uses SHA-256 to convert the seed string into a 256-bit hash.

Provides a uniform numeric value for PRNG seeding.



2. PRNG

Uses Mulberry32, a lightweight deterministic pseudo-random number generator.

Seeded with the first 32 bits of the SHA-256 hash.



3. Fisher–Yates Shuffle

Standard in-place shuffle algorithm.

Deterministic due to the seeded PRNG.

Produces a reproducible random ordering of all 2048 words.




Outcome:

Same seed → identical shuffle.

Different seed or name → different grid.



---

4. Grid Layout

Rows: 21 (row headers A–Z, skipping some letters to match BIP39 layout)

Columns: 98 (columns numbered 01–98)

Sheets: 5 A1 landscape pages

Split: [20, 20, 20, 20, 20] columns per sheet


Headers:

Column numbers (top row, gray background)

Row letters (first column, gray background)


Cells:

Body cells: white background

Text: black

Grid lines: black




---

5. Poster Rendering via JS Canvas

Canvas setup:

A1 landscape dimensions: 841 mm × 594 mm

Resolution: 300 DPI → high-quality poster printing

Pixel conversion:

width_px  = 841 mm × 300 DPI / 25.4 ≈ 9933 px
height_px = 594 mm × 300 DPI / 25.4 ≈ 7017 px


Cell dimensions:

Height per row: 594 mm / 21 ≈ 28.3 mm

Width per column (varies by sheet):

Sheets 1–4 (20 columns): 841 / 20 ≈ 42 mm


Font scaling:

Font height ≈ 75% of cell height → ~21 mm (~60 pt)

Ensures readability on A1 posters.



---

Canvas Drawing Steps

1. Initialize Canvas

Create <canvas> element dynamically

Set width/height to A1 landscape in pixels



2. Background

Fill canvas white for print-ready base



3. Column Headers

Gray fill for top row

Draw column numbers (01–98 split per sheet)

Draw grid lines around headers



4. Row Headers

Gray fill for first column

Draw letters (A–Z)

Draw grid lines around headers



5. Body Cells

White fill for each cell

Draw word text centered in cell

Draw black grid lines around each cell



6. Footer

Seed, date, and name displayed centered at bottom of canvas

Font scaled smaller than grid (20% of cell height)



7. Export

Convert canvas to PNG using canvas.toBlob()

Filename: [seed]_SheetX.png

Example: become-your-own-bank[01-02-2026][Alice]_Sheet1.png






---

6. Multi-Sheet Handling

Loop through the 5 sheet splits

Adjust column offsets for each sheet:

Sheet 1: columns 0–19

Sheet 2: 20–39

Sheet 3: 40–59

Sheet 4: 60–79

Sheet 5: 80–99


Each sheet generates its own canvas and PNG file

Ensures consistent row headers and sequential column numbering



---

7. Grid Lines Implementation

Grid lines drawn using ctx.strokeRect(x, y, width, height)

Applied to:

Body cells

Row headers

Column headers


Visual effect: clear separation for readability on large poster



---

8. Automatic File Naming

Converts seed string to filesystem-safe format: [a-zA-Z0-9-_]

Format:

<seed>_Sheet<1-5>.png

Allows users to easily identify and print the sheets in order



---

9. Technical Highlights

Fully client-side: runs in any modern browser

No external server required

Reproducible results: deterministic shuffle ensures same seed → same grid

Poster-ready output: 300 DPI A1 landscape PNGs

Dynamic scaling: cell size and font automatically calculated based on sheet column count

Printable grids: headers, row/column numbering, colors, and footer included



---

10. Potential Extensions

1. Full 2048-word BIP39 list embedded


2. Alternate row shading for readability


3. Custom font selection


4. High-DPI export (600 DPI) for extremely large prints


5. Optional multi-page tiling for ultra-wide grids


6. Rotate column headers to save horizontal space




---

✅ Summary

This tool combines deterministic cryptographic shuffling, dynamic grid layout, and high-resolution canvas rendering to produce print-ready multi-sheet BIP39 word posters. It is fully client-side, reproducible, and ready for offline printing.


---

If you want, I can also create a diagram showing the 5-sheet split with cell dimensions, headers, and footers to include in documentation — this makes it very easy to visualize for printing.

Do you want me to do that?
