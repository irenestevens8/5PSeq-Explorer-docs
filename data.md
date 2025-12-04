# 5PSeq Explorer Data

The data input for 5PSeq Explorer is provided as a data freeze hosted on the Swedish National Data Service (DOI pending). You may run the **Standard Web version**, which accesses data on the fly hosted on our laboratory server. Alternatively, the **Local (plug-and-play) 5PSeq Explorer** can be used with the data freeze or your own 5PSeq data, processed using the FivePSeq pipeline.

This document provides essential information for dataset reuse.

---

## Data Freeze Contents

This data freeze contains raw count files of 5PSeq data in tabular format. A metadata and RNA composition file are included as Excel files.

---

## Dataset Organization

This data freeze accompanies, and is compatible with, 5PSeq Explorer.

The data freeze contains:

* Metadata and RNA composition Excel files
* 5PSeq raw counts (`.txt`) for yeast and bacteria

### Folder Structure

#### Yeast (`yeast/`)

* Contains 315 'Sample' subdirectories (e.g., `SM_AA_t30_rep1`)
* Each sample subdirectory contains a `protein_coding/` folder with 30 files of raw processed counts

#### Bacteria (`bacteria/`)

* Contains 384 'Sample' subdirectories (e.g., `acac_g30_fecul-B_5fu-t05_r1_fc_r_v13_S11_R1`)
* Each sample subdirectory contains a `protein_coding/` folder with the same 30 files

Example structure:

```
5PSeq_processed_data/
├── yeast/
│   └── CSM_AA_t30_rep1/
│       └── protein_coding/
│           ├── amino_acid_pauses.txt
│           ├── canonical_transcript_index.txt
│           ├── codon_pauses.txt
│           ├── count_distribution.txt
│           ├── counts_START.txt
│           ├── counts_TERM.txt
│           ├── data_summary.txt
│           ├── dicodon_pauses.txt
│           ├── dipeptide_pauses.txt
│           ├── fft_signals_start.txt
│           ├── fft_signals_term.txt
│           ├── fft_stats.txt
│           ├── frame_counts_START.txt
│           ├── frame_counts_TERM.txt
│           ├── frame_stats.txt
│           ├── meta_count_peaks_START.txt
│           ├── meta_count_peaks_TERM.txt
│           ├── meta_counts_START.txt
│           ├── meta_counts_TERM.txt
│           ├── outlier_lower.txt
│           ├── outliers_df.txt
│           ├── start_codon_dict.txt
│           ├── term_codon_dict.txt
│           ├── transcript_assembly.txt
│           ├── transcript_descriptors.txt
│           ├── transcript_fft.txt
│           ├── transcript_frame_prefs.txt
│           ├── tricodon_pauses.txt
│           └── tripeptide_pauses.txt
└── bacteria/
    └── acac_g30_fecul-B_5fu-t05_r1_fc_r_v13_S11_R1/
        └── protein_coding/
            ├── amino_acid_pauses.txt
            ├── canonical_transcript_index.txt
            ├── codon_pauses.txt
            ├── count_distribution.txt
            ├── counts_START.txt
            ├── counts_TERM.txt
            ├── data_summary.txt
            ├── dicodon_pauses.txt
            ├── dipeptide_pauses.txt
            ├── fft_signals_start.txt
            ├── fft_signals_term.txt
            ├── fft_stats.txt
            ├── frame_counts_START.txt
            ├── frame_counts_TERM.txt
            ├── frame_stats.txt
            ├── meta_count_peaks_START.txt
            ├── meta_count_peaks_TERM.txt
            ├── meta_counts_START.txt
            ├── meta_counts_TERM.txt
            ├── outlier_lower.txt
            ├── outliers_df.txt
            ├── start_codon_dict.txt
            ├── term_codon_dict.txt
            ├── transcript_assembly.txt
            ├── transcript_descriptors.txt
            ├── transcript_fft.txt
            ├── transcript_frame_prefs.txt
            ├── tricodon_pauses.txt
            └── tripeptide_pauses.txt
metadata_v2.xls  
rna_stats_v2.xls
```

The `metadata.xls` file contains a `Sample` column that matches the names of each 'Sample' subdirectory. Additional sample information (SRA/GEO ID, PMID, publication record) is included.

---

## Datafile Descriptions

Each 'Sample' subdirectory contains 30 files of raw counts and statistics produced using the FivePSeq pipeline (PMID: 33575643).

For detailed information about the contents of each file, please refer to the [FivePSeq documentation](https://fivepseq.readthedocs.io/en/latest/interpreting_output.html).

---

## Reference Information

1. Pelechano, V., Wei, W., & Steinmetz, L. M. (2015). *Widespread Co-translational RNA Decay Reveals Ribosome Dynamics.* Cell, 161(6), 1400–1412.
2. Nersisyan, L., Ropat, M., & Pelechano, V. (2020). *Improved computational analysis of ribosome dynamics from 5’P degradome data using fivepseq.* NAR Genomics and Bioinformatics, 2(4), lqaa099.

