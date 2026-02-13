# Technical specifications

## System Requirements

### R Environment

- **R Version**: 3.6.0 or higher recommended
- **RStudio**: Optional but recommended for local deployment

Required R Packages

```r
# Core Shiny packages
- shiny
- shinydashboard 

# Deployment
- rsconnect

# File I/O
- readxl
- zip
- httr 

# Data manipulation
- dplyr
- purrr
- reshape2 

# Visualization
- ggplot2
- plotly
- viridis
- ggrepel
- DT (DataTables)
```

### Installation Command

```r
install.packages(c(
  "shiny", "ggplot2", "DT", "plotly", "viridis",
  "reshape2", "ggrepel", "rsconnect", "shinydashboard",
  "dplyr", "purrr", "readxl", "zip", "httr"
))
```

## Data Architecture

### Per-Sample Data Files

The Shinyapp uses 8 files in protein_coding/ subdirectory of each sample:

| **File** | **Content** | **Size** |
|----------|-------------|----------|
| data_summary.txt | Mapping statistics (NumOfReads, NumOfMapPositions) | 214 bytes |
| frame_stats.txt | Frame usage percentages (F0, F1, F2) | 310 bytes |
| meta_counts_START.txt | Raw counts at transcription start sites | 1 KB |
| meta_counts_TERM.txt | Raw counts at termination sites | 2 KB |
| amino_acid_pauses.txt | Reads over amino acids (positions -30 to +8) | 5 KB |
| codon_pauses.txt | Reads over codons (positions -30 to +8) | 13 KB |
| frame_counts_START.txt | Frame usage per gene | 85 KB |
| transcript_assembly.txt | Gene/transcript IDs and metadata | 9.1 MB |
| fft_signals_start.txt | FFT periodicity analysis | 10 KB |

**Total per sample**: 9.2 MB  
**Total dataset (773 samples)**: 6.9 GB

### Data Sources

All data is hosted remotely at: http://data.pelechanolab.com/software/5PSeq_explorer/

### Data Transfer

- All data is fetched via HTTP from remote server
- **Network dependency**: App requires stable internet connection
- **Latency impact**: Slow connections will delay plot rendering

| **Plot Type** | **Files Loaded** | **Estimated Transfer** |
|---------------|------------------|------------------------|
| Mapping stats | data_summary.txt | ~200 bytes |
| Frame stats | frame_stats.txt | ~300 bytes |
| Metagene plots | meta_counts_START/TERM.txt | ~3 KB |
| Heatmaps | amino_acid_pauses.txt, codon_pauses.txt | ~18 KB |
| **Violin plots** | frame_counts_START.txt, transcript_assembly.txt | **~9.2 MB** |
| **Ternary plots** | frame_counts_START.txt, transcript_assembly.txt | **~9.2 MB** |

## Performance Characteristics

### Loading times

#### Fast Operations (< 1 second)

- Metadata table rendering
- Checkbox selections
- Filter updates

#### Medium Operations (1-5 seconds per sample)

- Mapping statistics plots
- RNA composition plots
- Frame statistics plots
- Metagene START/STOP profiles
- FFT periodicity plots
- Amino acid/codon heatmaps
- Line plots for amino acid/codon stalls

#### Slow Operations (5-30+ seconds per sample)

**1. Violin Plots (Gene Frame Preferences)**

**Violin Plots are slow because they:**

- Load 2 large files per sample (~9.2 MB)
- Performs calculations (9 metrics) for thousands of genes
- Scales linearly: 5-10 sec/sample, thus 5 samples = 30-60+ seconds

- **Recommendations**:
  - Limit to 3-4 samples at a time
  - Use "Merged" replicate mode to reduce processing

**2. Ternary Plots (Gene Frame Distribution)**

**Ternary Plots are the slowest. The reasons are because**

- Load two large files per sample
- Perform geometric coordinate transformations for each gene

To make performance acceptable, we limited to max 2,000 genes (min: 100 counts per gene)

**Scaling:** ~10-15 sec/sample, thus 5 samples = 60+ seconds

## Memory Requirements

### Server-Side Memory

- **Base app memory**: ~50-100 MB
- **Per active user session**: ~200-500 MB (depending on selected samples)
- **Peak memory** (multiple users, many samples): 2-4 GB

### Client-Side (Browser)

- **Minimal requirements**: Modern browser with JavaScript enabled
- **Recommended RAM**: 4+ GB for smooth interactive plots
- **Plotly rendering**: May consume 200-500 MB for complex heatmaps


## Download Functionality Limitations

### Download Constraints

The app includes a **limit** on downloads because the ZIP file generation with remote file fetching can timeout on server.

### Download Options

1. **Raw Count Files** (8 files per sample)
   - Consider limiting to 4 samples (up to ~6 MB total)

2. **CPM Normalized Files** (5 files per sample)
   - Generated on-the-fly from raw counts
   - Consider limiting to 4 samples

3. **Transcript-level Frame Proportions** (1 file per sample)
   - Most computationally intensive
   - Processes frame calculations for all genes
   - Consider limiting to 4 samples

## Error Handling

### Common Errors and Solutions

| **Error** | **Cause** | **Solution** |
|-----------|-----------|--------------|
| "No data available" | Sample data not found on server | Check sample name, verify server availability |
| "Timeout error" | Slow network or server | Reduce number of samples, retry |
| "Download failed" | Too many samples selected | Select ≤4 samples |
| Blank plots | All genes filtered out | Check filter criteria (e.g., min read threshold) |
| Memory errors | Too many samples/plots | Refresh browser, select fewer samples |

## Performance Benchmarks

### Sample Processing Times (Single Sample, Fast Connection)

| **Operation** | **Time** | **Notes** |
|---------------|----------|-----------|
| Load metadata | <1s | One-time on app start |
| Render metadata table | 1-2s | Interactive filtering |
| Mapping stats plot | 1-2s | Per sample |
| RNA composition | 1-2s | Per sample |
| Frame stats | 2-3s | Per sample |
| Metagene plots | 2-4s | Per sample |
| Heatmaps | 3-5s | Larger matrices |
| **Violin plot** | **8-15s** | **Slow** |
| **Ternary plot** | **10-20s** | **Slowest** |

*Times measured on medium-spec server (2 cores, 4 GB RAM, 100 Mbps connection)*

## Contact & Support

For technical issues or questions:

- Contact: irene.stevens@ki.se
- Lab: Pelechano Lab

**Document Version**: 1.0  
**Last Updated**: February 2026  
