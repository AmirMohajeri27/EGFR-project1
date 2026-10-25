# EGFR Inhibitor Binding-Pocket Analysis

## Project goal

This project explores how two drugs bind the kinase domain of the Epidermal Growth Factor Receptor (EGFR).
The goal is to identify and compare the binding-pocket residues around two EGFR inhibitors:

- Erlotinib
- Gefitinib

## What I have done so far

### 1. Selected EGFR structures

I selected two experimentally determined EGFR kinase-domain structures from the Protein Data Bank (PDB):

| Drug | PDB ID | Ligand code |
|---|---:|---|
| Erlotinib | 1M17 | AQ4 |
| Gefitinib | 2ITY | IRE |

The PDB structure files are saved in `data/structures/`.

### 2. Loaded and explored the protein structures

Using Biopython's `Bio.PDB` tools, I loaded the EGFR structures and examined:

- Protein chain information
- Residues in chain A
- Bound inhibitor molecules
- Water molecules and other non-protein components

### 3. Identified binding-pocket residues

For each inhibitor, I used a distance-based method to find EGFR residues located within 5 Å of the bound drug.
This gives a list of residues that form the local binding pocket around each inhibitor.

### 4. Visualized the structure

I used `py3Dmol` to visualize:

- The EGFR kinase domain
- The bound ligand
- Binding-pocket residues around the ligand

### 5. Created a reusable pocket-analysis function

I created a `find_pocket()` function that:

1. Loads a PDB structure
2. Finds a ligand by its ligand code
3. Detects nearby protein residues within a chosen distance cutoff
4. Returns the residues forming the binding pocket

### 6. Compared the two pockets and fixed a numbering mismatch

A first comparison of the raw residue numbers found only 2 shared pocket residues. This result is not realistic: both drugs are ATP-competitive inhibitors that bind the same ATP-binding site, so their pockets should overlap strongly.

Investigation showed that the two PDB files do not number the EGFR residues in the same way. The same physical residue can have a different number in each file. A comparison of raw numbers is therefore not meaningful.

To fix this, I:

- Extracted the amino acid sequence of the protein chain in each structure
- Aligned the two sequences with Biopython's `PairwiseAligner`
- Confirmed that the sequences are almost identical (one small gap, where a loop was not resolved in one structure), so both structures show the same region of EGFR with different numbering
- Built a residue-to-residue mapping from the alignment, which translates the erlotinib pocket residue numbers into the gefitinib numbering

## Results

After correcting for the numbering mismatch:

- **20 residues** form the shared binding pocket for both erlotinib and gefitinib: 718, 719, 726, 743, 745, 762, 766, 788–797, 844, 854, 855
- **0 residues** are unique to the erlotinib pocket. It is fully contained in the gefitinib pocket.
- **3 residues** (720, 744, 800) are unique to the gefitinib pocket.

This large overlap is consistent with both drugs binding the same ATP site.

The shared residues include Thr790 and Cys797, two clinically important positions in EGFR:

- Thr790 is the position of the T790M mutation, the main resistance mutation to first-generation inhibitors such as erlotinib and gefitinib.
- Cys797 is the position where later-generation inhibitors such as osimertinib bind covalently. The C797S mutation causes resistance to osimertinib.

## Repository structure

```text
EGFR-project1/
├── data/
│   └── structures/
│       ├── pdb1m17.ent
│       └── pdb2ity.ent
├── notebooks/
│   └── fetch_structures.ipynb
└── README.md
```

## How to run

1. Create and activate a virtual environment:

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

2. Install the packages:

   ```bash
   python -m pip install -r requirements.txt
   ```

3. Open the notebook `notebooks/fetch_structures.ipynb` and run the cells from top to bottom.

## Tools used

- Python
- Biopython (`Bio.PDB`, `Bio.Align`, `Bio.SeqUtils`)
- Jupyter Notebook
- py3Dmol
- Protein Data Bank (PDB)

## Planned next steps

- Move the analysis functions from the notebook into a Python module
- Add more EGFR structures, including mutant structures
- Use machine learning on a larger dataset of EGFR inhibitors
