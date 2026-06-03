# PXD000045 — MTB Hypoxia Proteomics Pipeline

End-to-end computational proteomics pipeline applied to the *Mycobacterium tuberculosis* hypoxia/reaeration dataset from PRIDE Archive.

---

## Goal

*Mycobacterium tuberculosis* (MTB) causes tuberculosis and can survive for years in a latent state inside human lungs by adapting to low-oxygen (hypoxic) environments. Understanding which proteins MTB produces under hypoxia is critical for identifying new drug targets and explaining antibiotic resistance.

This pipeline processes raw MS/MS data to identify and quantify MTB proteins expressed during hypoxia.

---

## Dataset

- **Source:** PRIDE Archive — [PXD000045](https://www.ebi.ac.uk/pride/archive/projects/PXD000045)
- **Organism:** *Mycobacterium tuberculosis* H37Rv
- **Experiment:** Hypoxia / reaeration timecourse (LC-MS/MS)
- **Raw spectra:** 17,968
- **Reference proteome:** UniProt UP000001584 (3,997 proteins)

---

## Pipeline Steps

1. **Data Download** — MGF file retrieved from PRIDE via FTP
2. **Peak Filtering** — Low-quality spectra removed (min intensity, min peaks)
3. **Trypsin Digestion** — 129,753 theoretical peptides generated from MTB FASTA
4. **Database Search** — Precursor mass matching at 10 ppm tolerance
5. **FDR Filtering** — PSMs filtered at < 5 ppm mass error
6. **Protein Identification** — Proteins with ≥ 2 unique peptides retained
7. **Quantification** — Spectral counting per protein
8. **GO Pathway Analysis** — Biological process enrichment via UniProt annotations

---

## Key Results

- **17,722 PSMs** matched (98.6% of spectra)
- **801 proteins** identified with high confidence
- **Mean mass error:** 1.35 ppm

### Top Abundant Proteins

| Protein | Name | PSM Count |
|---------|------|-----------|
| I6XD69 | Mycoketide-CoA synthase | 83 |
| P9WMJ9 | Chaperone protein DnaK | 68 |
| I6X8D2 | Polyketide synthase Pks13 | 66 |
| P9WJV5 | Trehalose monomycolate exporter MmpL3 | 59 |
| P9WPE7 | Chaperonin GroEL 2 | 58 |

### Top Biological Processes

| Biological Process | Proteins |
|-------------------|---------|
| Fatty acid biosynthetic process | 34 |
| Regulation of DNA-templated transcription | 34 |
| Response to antibiotic | 29 |
| Interaction with host | 27 |
| Response to hypoxia | 25 |
| Cell wall organization | 20 |

### Interpretation

High abundance of **DnaK** and **GroEL 2** confirms active stress response under hypoxia. Enrichment of **cell wall organization** and **fatty acid biosynthesis** reflects MTB's thick mycolic acid cell wall maintenance. Co-enrichment of **response to hypoxia** and **response to antibiotic** is consistent with the latent infection model, where hypoxia-adapted MTB exhibits intrinsic antibiotic tolerance.

---

## Repository Structure

```
PXD000045_proteomics/
├── data/                          # Raw data (not tracked by git)
│   ├── sample.mgf                 # Raw MS/MS spectra (158 MB)
│   ├── mtb.fasta                  # MTB reference proteome
│   ├── mtb_go.tsv                 # GO annotations
│   ├── spectra.pkl                # Filtered spectra
│   ├── peptide_db.pkl             # Peptide database
│   └── peptide_masses.pkl         # Theoretical masses
├── results/
│   ├── search_results.csv         # PSM-level results
│   ├── identified_proteins.csv    # Protein-level results
│   ├── quantification.csv         # Spectral counts
│   ├── protein_identification.png
│   ├── top20_proteins.png
│   └── pathway_analysis.png
├── notebook.ipynb                 # Main analysis notebook
├── .gitignore
└── README.md
```

---

## Requirements

```bash
conda create -n proteomics python=3.10
conda activate proteomics
pip install pyteomics lxml numpy pandas matplotlib seaborn requests goatools jupyter
```

---

## How to Run

```bash
# 1. Clone the repo
git clone https://github.com/zeynep-aslan1710/PXD000045_proteomics.git
cd PXD000045_proteomics

# 2. Set up environment
conda create -n proteomics python=3.10
conda activate proteomics
pip install pyteomics lxml numpy pandas matplotlib seaborn requests goatools jupyter

# 3. Open notebook and run cells in order
jupyter notebook
```

Large data files (MGF, FASTA, pickle) are excluded from git due to size. They will be downloaded automatically when running the notebook.

---

## References

- Zheng et al. (2013). LC-MS/MS of *M. tuberculosis* in a hypoxia/reaeration timecourse. PRIDE: PXD000045.
- UniProt Consortium. MTB H37Rv proteome: UP000001584.
- Gene Ontology Consortium. http://geneontology.org
