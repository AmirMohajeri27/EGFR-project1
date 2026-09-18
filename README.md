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
- Biopython
- Jupyter Notebook
- py3Dmol
- Protein Data Bank (PDB)

