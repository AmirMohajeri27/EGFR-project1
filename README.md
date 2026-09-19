# EGFR Inhibitor Binding-Pocket Analysis

## Project goal

This project explores how 2 different drugs bind the kinase domain of THE Epidermal Growth Factor Receptor (EGFR).
The initial goal is to identify and compare the binding-pocket residues surrounding two EGFR inhibitors:

- Erlotinib
- Gefitinib
  
##what i have done so far

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

This creates a list of residues that form the local binding pocket around each inhibitor.

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

###  6. Started comparing erlotinib and gefitinib pockets

I used the function to compare residues near erlotinib and gefitinib.

### Important limitation of the first comparison

At first, the analysis compared the residue numbers directly and found only a small number of shared pocket residues between erlotinib and gefitinib.

However, this result is not biologically realistic. Erlotinib and gefitinib are both known to bind the same main region of EGFR: the ATP-binding pocket. Therefore, their nearby protein residues should overlap substantially.

We found that the problem comes from the raw PDB files. The two structures do not label EGFR residues in exactly the same way: one structure starts and numbers the kinase-domain residues differently from the other. This means that two numbers that look different can still refer to the same position in the EGFR protein.

For this reason, the first raw-number comparison cannot yet be used to conclude that the two drugs bind different pockets. The next step is to align the protein structures and convert residues to one shared EGFR numbering system before comparing them again.
### 7. Fixed the numbering mismatch using sequence alignment

Rather than assuming what the numbering difference was, I confirmed it directly:

- Extracted the amino acid sequence of each structure's protein chain
- Aligned the two sequences using Biopython's `PairwiseAligner`
- The alignment showed the two structures are almost identical in sequence (one small gap where a loop wasn't resolved in one structure), confirming they represent the same region of EGFR — just numbered differently by each depositor
- Built a residue-to-residue mapping from the alignment, translating erlotinib's pocket residue numbers into gefitinib's numbering system

### Resolving the numbering mismatch

An initial direct comparison of residue numbers found only 2 shared pocket residues between erlotinib and gefitinib — biologically implausible, since both drugs are known ATP-competitive inhibitors expected to occupy the same pocket.

Investigation showed the two PDB structures label EGFR residues differently: the same physical residue can have a different number in each file. A raw-number comparison was therefore not meaningful.

This was resolved by aligning the two structures' protein sequences and using the alignment to build a mapping between their residue numbering systems (see Section 7). Comparing pocket residues through this mapping, instead of by raw number, gives a result consistent with known EGFR biology (see Results below).

## Results

After correcting for the residue numbering mismatch:

- **20 residues** form the shared binding pocket for both erlotinib and gefitinib: 718, 719, 726, 743, 745, 762, 766, 788–797, 844, 854, 855
- **0 residues** are unique to erlotinib's pocket — it is fully contained within gefitinib's
- **3 residues** (720, 744, 800) are unique to gefitinib's pocket

This large overlap is consistent with both drugs being ATP-competitive EGFR inhibitors that bind the same site. The shared residues include Thr790 and Cys797 — two of the most clinically significant positions in EGFR, associated with drug-resistance mutations (T790M and C797S) seen in lung cancer patients treated with these inhibitors.


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


## Tools used
- Python
- Biopython (`Bio.PDB`, `Bio.Align`, `Bio.SeqUtils`)
- Jupyter Notebook
- py3Dmol
- Protein Data Bank (PDB)
