# cav1
This Colab pipeline analyzes human Caveolin-1 (178 aa) using Kyte–Doolittle hydropathy with a sliding window (w=19) to detect membrane-associated regions. It generates a hydropathy profile, exports summary features, and identifies residues 111–130 (MALIWGIYFAILSFLHIWAV) as the predicted membrane-inserting hairpin core.

# Caveolin-1 Membrane Domain Analysis (Lightweight Biophysics Pipeline)

## Overview

This project implements a lightweight, reproducible pipeline to analyze **human Caveolin-1 (CAV1)** using **sequence-based biophysics**, with an emphasis on identifying and interpreting **membrane-associated regions**. The workflow computes a **Kyte–Doolittle hydropathy profile** (sliding window smoothing), visualizes the hydrophobic landscape across the protein, exports summary features, and extracts the predicted **membrane hairpin core** sequence.

This repository is designed to:
- run cleanly in **Google Colab** (CPU-only is fine)
- stay **interpretable** (no black-box steps required)
- produce **publishable-style figures and outputs**
- serve as a base for later 3D/ML extensions


## Why This Matters

Caveolin-1 is a core structural component of **caveolae**, small invaginations of the plasma membrane that function as **mechanosensory hubs** and organizing platforms for signaling and lipid regulation. Caveolae are involved in membrane tension buffering, endocytosis, cholesterol homeostasis, and multiple signaling pathways. Disrupting Caveolin-1 structure or membrane insertion can destabilize caveolae, alter mechanotransduction, and contribute to disease phenotypes (e.g., pulmonary vascular disease, lipid disorders, cancer-associated signaling rewiring in some contexts).

The **membrane-inserting hairpin** region is central to Caveolin-1 function because it governs:
- how CAV1 embeds into the inner leaflet (topology)
- how membrane curvature is generated/maintained
- how oligomeric assemblies scaffold signaling domains
- how lipid interactions (e.g., cholesterol) are stabilized

Computationally, this project demonstrates a valuable principle: **you can recover real membrane biology from primary sequence** using classical biophysical rules (hydropathy + windowing) before reaching for heavy 3D prediction models. That makes it useful for rapid hypothesis generation, mutation screening, teaching, and research settings without large compute.

---

## Biological Background

### What is Caveolin-1?
**Caveolin-1 (CAV1)** is a 178 amino acid membrane-associated protein essential for caveolae biogenesis and function. Unlike many membrane proteins that span the lipid bilayer, Caveolin-1 forms a **re-entrant membrane hairpin**, inserting into the membrane without fully crossing it, leaving both N- and C-termini on the cytosolic side (topology simplified here).

### Functional architecture (high-level)
- N-terminal cytosolic region: regulatory/scaffolding, often more flexible
- Central hydrophobic region: membrane insertion / hairpin core
- C-terminal cytosolic region: includes functionally important motifs and lipid modifications in many contexts

---

## What the Program Does (Current Notebook)

The code you’ve implemented performs:

1. **Environment setup**
   - creates/validates `data/` and `results/`

2. **FASTA generation**
   - writes the full-length CAV1 sequence to `data/caveolae_targets.fasta`

3. **Sequence loading**
   - reads FASTA using BioPython (`SeqIO`) to ensure correct file-based workflow

4. **Hydropathy profiling**
   - maps each residue to Kyte–Doolittle hydropathy values
   - applies a **sliding window average** (default window = 19)

5. **Visualization**
   - plots hydropathy vs residue index
   - overlays a heuristic membrane threshold line at **1.6**

6. **Feature export**
   - saves `results/features_light.csv` containing:
     - protein length
     - max windowed hydropathy
     - mean windowed hydropathy

7. **Hairpin extraction**
   - prints the sequence segment **111–130** as the membrane hairpin core

---

## Expected Output (CAV1)

### Membrane hairpin core sequence (111–130)
MALIWGIYFAILSFLHIWAV

### Biological interpretation (what this implies)
This segment is:
- strongly enriched for hydrophobic residues (L/I/V/F/M)
- contains aromatic residues (W/Y/F) that often stabilize membrane interfaces
- consistent with a membrane-inserting segment (hairpin-compatible)

---

## Repository Structure
/content (or your repo root)
├── data/
│ └── caveolae_targets.fasta
├── results/
│ └── features_light.csv
└── (notebook cells in Colab)

---

## Installation

Minimal dependencies (lightweight mode):
- biopython
- numpy
- pandas
- matplotlib


## How to Run (Colab)
Step 1 — Setup folders
Run the setup cell that creates:
data/
results/
Step 2 — Save FASTA
Run the cell that writes:
data/caveolae_targets.fasta (length 178)
You should see:
Saved FASTA length: 178
Step 3 — Hydropathy plot + CSV
Run the hydropathy cell to produce:
a plot showing a strong hydrophobic peak around the membrane region
results/features_light.csv
Step 4 — Print membrane hairpin
Run:
the extraction cell for residues 111–130
Parameters & Interpretation Notes
Window size (default = 19)
A 19-residue window is commonly used as it roughly matches the scale of membrane helices and helps smooth noise.
Threshold (default = 1.6)
The 1.6 threshold is a heuristic: segments above this are typically strongly membrane-associated. Caveolin is a special case because it forms a hairpin rather than a classical spanning helix; interpret the result as “membrane-inserting/associated,” not necessarily “full transmembrane.”

Outputs
results/features_light.csv
Contains:
id
length
hydro_max_windowed
hydro_mean_windowed
Use this for:
- comparing multiple caveolae proteins
- benchmarking effects of mutations (future extension)
- reporting summary stats

## Limitations 

-Hydropathy-based membrane prediction is a heuristic (does not model lipid energetics explicitly).
-Does not predict tertiary/quaternary structure, oligomer interfaces, or membrane orientation.
-Does not incorporate post-translational modifications beyond what you manually annotate.
Despite this, the approach is extremely useful for identifying where membrane association is encoded in the sequence and for generating testable hypotheses.

## References (Links)
(Links provided in code block for easy copy/paste.)
Kyte–Doolittle hydropathy (original):
https://doi.org/10.1016/0022-2836(82)90515-0

UniProt CAV1 (Q03135):
https://www.uniprot.org/uniprotkb/Q03135

AlphaFold DB entry for CAV1:
https://alphafold.ebi.ac.uk/entry/Q03135

Caveolae biology review (mechanosensing/signaling context):
https://doi.org/10.1038/nrm3640
