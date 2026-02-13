# 5PSeq Explorer Plot Descriptions
## Biological Interpretation Guide

---

## Introduction to 5PSeq

5PSeq profiles mRNA degradation intermediates by capturing the 5' phosphorylated ends of mRNAs using an oligo. During translation, enzymatic cleavage occurs immediately behind the trailing ribosome, making 5PSeq a powerful, but indirect method for studying ribosome dynamics. The method offers information about:

- Ribosome positioning along transcripts
- Translation initiation and termination dynamics
- Codon- and amino acid-specific ribosome pausing
- Reading frame usage across the transcriptome

---

## 1. Mapping Statistics

### 1.1 Read Count Barplot

**What it shows:** 
Two overlapping bars for each sample displaying total sequencing reads (light orange) and uniquely mapped positions (dark orange).

**Biological Interpretation:**

- **Sequencing Depth:** Higher total read counts provide better statistical power for detecting ribosome positions and rare pausing events. Typical high-quality libraries should have millions of reads.

- **Mapping Efficiency:** The ratio of mapped positions to total reads indicates data quality. A high mapping rate (>70-80%) suggests successful library preparation and minimal contamination. Low mapping rates may indicate:
  - Technical issues (adapter dimers, degraded RNA)
  - Increased global mRNA degradation
  - Contamination with non-targeted RNA species

- **Normalization Reference:** The number of mapped positions serves as the denominator for CPM (counts per million) normalization, enabling fair comparisons across samples with different sequencing depths.

**What to look for:**
- Consistent mapping rates across biological replicates
- Sufficient depth for downstream analyses (typically >1M mapped positions)
- Differences between conditions that might reflect biological changes in mRNA stability

---

### 1.2 RNA Composition Stacked Barplot

**What it shows:** 
Stacked bars showing the relative abundance of different RNA types (mRNA, rRNA, tRNA, etc.) as percentages of total reads.

**Biological Interpretation:**

- **Sample Quality Control:** High-quality 5PSeq libraries should be enriched for mRNA degradation products. The ideal composition shows:
  - High mRNA content (>60-70%)
  - Low rRNA content (<10-20%)
  - Minimal tRNA contamination

- **Biological Meaning of RNA Composition Changes:**
  - **Increased rRNA degradation:** May occur during nutrient starvation when ribosome biogenesis is repressed and existing ribosomes are degraded
  - **Shifts in mRNA proportions:** Can reflect stress responses, where certain mRNA classes become more or less stable
  - **tRNA degradation:** May indicate cellular stress or tRNA quality control activation

- **Technical Considerations:** High rRNA content despite depletion attempts may indicate:
  - Incomplete rRNA depletion
  - RNA degradation during library preparation
  - Biological increase in rRNA turnover

**What to look for:**
- Consistency across biological replicates
- Expected enrichment for mRNA
- Biological changes in RNA stability patterns between conditions

---

## 2. Frame Statistics

### 2.1 Frame Percentage Stacked Barplot

**What it shows:** 
Stacked bars displaying the percentage of reads in each of the three reading frames (F0, F1, F2) across all genes for each sample.

**Biological Interpretation:**

**Understanding Reading Frames:**
- **F0:** First nucleotide position of a codon
- **F1:** Second nucleotide position of a codon  
- **F2:** Third nucleotide position of a codon

**Normal Eukaryotic Pattern:**
In healthy, actively translating eukaryotic cells, **F1 should dominate (60-80%)** because:
- The ribosome protects approximately 28-30 nucleotides
- The P-site (where peptidyl-tRNA binds) positions the ribosome such that degradation occurs ~17 nucleotides upstream
- This distance corresponds to the F1 position of the codon in the P-site

**Biological Significance of Frame Distributions:**

- **Strong F1 enrichment (normal):** 
  - Indicates proper ribosome positioning
  - Reflects canonical ribosome footprint
  - Validates that 5PSeq captures true ribosome-protected positions

- **Shift toward F0 enrichment:**
  - May indicate altered ribosome conformation
  - Could reflect ribosome stalling or pausing
  - Possible activation of quality control pathways
  - May occur during specific stress conditions

- **Shift toward F2 enrichment:**
  - Less common but can occur in specific biological contexts
  - May indicate altered ribosome positioning
  - Could reflect changes in mRNA degradation machinery

- **Bacterial vs. Eukaryotic Differences:**
  - Bacterial ribosomes may show different frame preferences
  - Differences in ribosome size and structure affect footprint
  - Different mRNA degradation machinery influences patterns

**What to look for:**
- Strong F1 enrichment as quality control metric
- Consistent patterns across biological replicates
- Biologically meaningful shifts between conditions (e.g., stress-induced changes)

---

### 2.2 Individual Frame Percentage Barplots

**What it shows:** 
Individual barplots for each sample showing the percentage of reads in F0, F1, and F2.

**Biological Interpretation:**

Same principles as the stacked plot above, but allows for:
- Detailed examination of individual samples
- Identification of outlier samples with unusual frame distributions
- Quality control for each sample independently

**What to look for:**
- F1 values in the 60-80% range for eukaryotes
- Samples with unusual distributions that might need exclusion
- Consistency within replicate groups

---

## 3. FFT Periodicity Analysis

**What it shows:** 
Fast Fourier Transform (FFT) analysis identifying periodic signals in 5PSeq read distributions around translation start sites. Peaks indicate dominant periodicities.

**Biological Interpretation:**

**3-Nucleotide Periodicity - The Gold Standard:**
A strong peak at period 3 (or near 3) is the hallmark of active translation because:
- Ribosomes move in discrete 3-nucleotide steps (codon by codon)
- This creates a repeating pattern in read density
- Strong periodicity indicates synchronized, processive translation

**FFT Signal Strength:**

- **High amplitude peaks:**
  - Strong, synchronized translation
  - Processive ribosome movement
  - High-quality 5PSeq data
  - Abundant ribosome density

- **Weak or absent periodicity:**
  - May indicate ribosome stalling or pausing
  - Could reflect asynchronous translation
  - Possible technical issues with library quality
  - Low ribosome density on transcripts

**Additional Periodicities:**

- **Peaks at 9, 12 nucleotides:** May reflect:
  - Higher-order structural features
  - Repeating amino acid motifs causing systematic pausing
  - Secondary structure effects on ribosome movement

**Integration with Frame Statistics:**
Strong F1 enrichment should correlate with strong 3-nt periodicity:
- Both reflect proper ribosome positioning
- Combined analysis provides robust quality control
- Discordance suggests technical or biological anomalies

**What to look for:**
- Clear peak at period 3
- High signal amplitude
- Consistency across biological replicates
- Changes in periodicity strength between conditions (e.g., loss during stress)

---

## 4. Metagene Profiles

### 4.1 Metagene START (Translation Initiation Sites)

**What it shows:** 
Average 5PSeq signal (CPM) aligned to translation start sites (AUG start codons) across all genes. X-axis shows nucleotide positions relative to the start codon (negative = upstream, positive = downstream).

**Biological Interpretation:**

**Key Features:**

1. **Initiation Peak:**
   - Prominent peak 12-18 nucleotides downstream of start codon
   - Reflects ribosome accumulation during initiation
   - Position corresponds to P-site of initiating ribosomes
   - Height indicates initiation rate

2. **5' UTR Signal (Negative Positions):**
   - Should be low, confirming ribosomes haven't engaged yet
   - Elevated signal may indicate:
     - uORFs (upstream open reading frames)
     - Non-canonical initiation
     - 5' UTR secondary structure effects

3. **Elongation Transition:**
   - Signal typically decreases as ribosomes enter elongation
   - Spreading indicates ribosomes dispersing along CDS
   - Persistent elevation suggests:
     - Slow elongation
     - Ribosome queuing/traffic jams
     - Rate-limiting steps early in elongation

4. **3-Nucleotide Periodicity:**
   - Visible oscillations with 3-nt spacing downstream
   - Indicates synchronized ribosome stepping
   - Strongest near start codon, fades with distance

**Biological Changes to Look For:**

- **Altered peak height:**
  - Increased: Enhanced initiation (growth, proliferation)
  - Decreased: Reduced initiation (stress, starvation)

- **Peak position shifts:**
  - May indicate altered start codon selection
  - Could reflect reinitiation events
  - Possible changes in ribosome scanning

- **Broader peaks:**
  - Slower elongation during early translation
  - Ribosome queuing
  - Presence of regulatory pause sites

**What to look for:**
- Clear initiation peak
- Low 5' UTR background
- 3-nt periodicity in early coding sequence
- Differences between conditions that reflect biological changes in translation control

---

### 4.2 Metagene STOP (Translation Termination Sites)

**What it shows:** 
Average 5PSeq signal aligned to translation termination codons (UAA, UAG, UGA). Negative positions = end of coding sequence, positive = 3' UTR.

**Biological Interpretation:**

**Key Features:**

1. **Termination Peak:**
   - Sharp peak immediately before/at stop codon
   - Reflects ribosome accumulation during termination
   - Results from time required for:
     - Release factor binding
     - Peptidyl-tRNA hydrolysis
     - Ribosome subunit dissociation

2. **Peak Characteristics:**
   - **Sharp peak:** Efficient termination
   - **Broad peak:** Slow termination or stalling
   - **Height:** Reflects termination efficiency

3. **3' UTR Signal (Positive Positions):**
   - Should drop rapidly after stop codon
   - Minimal signal confirms proper termination
   - Elevated 3' UTR signal may indicate:
     - **Readthrough:** Ribosomes bypassing stop codon
     - **Reinitiation:** Ribosomes restarting in 3' UTR
     - **Stop codon suppression:** Context-dependent or tRNA-mediated

4. **Ribosome Recycling:**
   - Rapid signal decline shows efficient release
   - Extended signal suggests:
     - Slow ribosome recycling
     - Ribosome stalling at termination
     - Issues with recycling factors

**Stop Codon Context Effects:**

Different stop codons (UAA, UAG, UGA) can show:
- Different termination efficiencies
- Variable peak shapes
- Context-dependent recognition by release factors
- Surrounding nucleotides influence termination speed

**Biological Changes to Look For:**

- **Increased termination signal:**
  - Slower termination
  - Changes in release factor activity
  - Altered ribosome recycling

- **3' UTR readthrough:**
  - Stop codon suppression
  - Stress-induced readthrough
  - Selenocysteine incorporation (UGA)

- **Extended ribosome dwell time:**
  - Quality control activation
  - No-go decay pathway engagement

**What to look for:**
- Clear termination peak
- Rapid signal decline after stop
- Minimal 3' UTR signal
- Changes in termination efficiency between conditions

---

## 5. Amino Acid Protection Profiles

### 5.1 Amino Acid Protection Heatmap

**What it shows:** 
Heatmap displaying 5PSeq signal intensity (CPM or row-normalized) for each amino acid across positions relative to its coding codon. Rows = amino acids, columns = positions (-20 to -3), color intensity = read counts.

**Biological Interpretation:**

**Understanding the Ribosome Footprint:**

- **Protected Region:** Typically -17 to -12 nucleotides upstream
- Represents where ribosomes pause when translating specific amino acids
- Reads in this region indicate ribosome P-site position

**Hot Spots (Bright Colors) Indicate Pausing:**

Amino acids where ribosomes frequently pause may result from:

1. **Slow Peptide Bond Formation:**
   - Proline is the classic example
   - Cyclic structure restricts conformational flexibility
   - Slows peptidyl transferase reaction

2. **tRNA Availability:**
   - Rare codons with low tRNA abundance
   - Amino acid starvation conditions
   - tRNA charging limitations

3. **Nascent Chain Interactions:**
   - Emerging peptide interacts with exit tunnel
   - Specific sequences trigger pausing (e.g., SecM, TnaC)
   - Regulatory pausing for co-translational folding

4. **Co-translational Folding:**
   - Pausing allows domain folding
   - Prevents misfolding or aggregation
   - Coordinated with chaperone binding

**Known Pause-Inducing Amino Acids:**

- **Proline:** 
  - Well-established pause site
  - Especially strong in polyproline stretches
  - Pro-Pro or Pro-Gly particularly slow

- **Charged amino acids (Arg, Lys, Asp, Glu):**
  - Can interact with tunnel walls
  - Context-dependent effects
  - May regulate expression

- **Aromatic amino acids (Trp, Phe, Tyr):**
  - Bulky side chains
  - Potential tunnel interactions
  - Sometimes associated with pausing

**Normalization Effects:**

- **With row normalization:** 
  - Shows preferred position for each amino acid
  - Independent of amino acid frequency
  - Reveals position-specific patterns

- **Without normalization:**
  - Abundant amino acids dominate signal
  - Reflects both abundance and pausing
  - May mask patterns for rare amino acids

**What to look for:**
- Proline enrichment at -17 position (classic pattern)
- Changes in pause patterns between conditions (e.g., amino acid starvation)
- Unexpected amino acids showing strong pausing (potential novel regulatory mechanisms)
- Consistency of patterns across biological replicates

---

### 5.2 Amino Acid Protection Lineplot

**What it shows:** 
Line plot showing 5PSeq signal profile for a selected amino acid across all positions, comparing multiple samples.

**Biological Interpretation:**

**Peak Analysis:**

1. **Peak Position:**
   - Indicates where ribosome decoding center sits relative to amino acid
   - Typical peak: -17 to -12 nucleotides
   - Shifts may indicate:
     - Altered ribosome conformation
     - Different library preparation (rare)
     - Biological changes in ribosome positioning

2. **Peak Intensity (Height):**
   - Higher peaks = stronger pausing
   - Reflects how problematic this amino acid is for translation
   - **Biological changes in peak height:**
     - Increased: Amino acid becomes limiting, tRNA scarcity
     - Decreased: Improved amino acid availability, upregulated tRNAs

3. **Peak Width:**
   - **Sharp peaks:** Single dominant pause site
   - **Broad peaks:** 
     - Heterogeneous pausing over several codons
     - Slower movement through this region
     - May indicate sequential pausing events

**Comparative Analysis:**

Comparing peaks across conditions reveals:
- **Amino acid starvation responses:** Specific amino acid limitation increases pausing
- **tRNA abundance changes:** Overexpression or depletion affects pause strength
- **Stress-induced changes:** Global or specific alterations in translation dynamics

**What to look for:**
- Peak position consistency (quality control)
- Significant height differences between conditions
- Changes specific to certain amino acids (targeted effects)
- Correlation with known biological perturbations (e.g., leucine starvation increases Leu pausing)

---

### 5.3 Amino Acid Protection Scatterplot

**What it shows:** 
Scatterplot comparing 5PSeq signal for all 20 amino acids at a specific position between two samples. Each point = one amino acid.

**Biological Interpretation:**

**Diagonal Distribution:**

- **Points near diagonal:** Similar pause behavior in both samples
- Indicates conserved translation dynamics
- Expected for most amino acids under similar conditions

**Off-Diagonal Outliers - Key Discoveries:**

Points far from diagonal reveal amino acids with differential pausing:

1. **Above diagonal:** More pausing in Sample 2
   - May indicate amino acid limitation
   - Could reflect tRNA availability changes
   - Possible stress-specific effects

2. **Below diagonal:** More pausing in Sample 1
   - Less pausing in Sample 2
   - May reflect improved conditions
   - Upregulated tRNA pools

**Common Patterns:**

- **Proline consistently high:** Expected in all conditions
- **Coordinated changes:** Multiple amino acids shift together
  - May reflect global tRNA pool changes
  - Could indicate ribosome modification states
  - Possible stress response signatures

**Discovery Applications:**

- **Identify limiting amino acids:** Condition-specific bottlenecks
- **tRNA charging defects:** Specific amino acids affected by metabolic perturbations
- **Novel regulatory mechanisms:** Unexpected amino acid pausing patterns

**What to look for:**
- Specific amino acids unique to one condition
- Clusters of related amino acids (e.g., all charged residues)
- Magnitude of changes (fold-change from diagonal)
- Biological relevance to experimental perturbation

---

## 6. Codon Protection Profiles

### 6.1 Codon Protection Heatmap

**What it shows:** 
Heatmap displaying 5PSeq signal intensity for each codon (64 total) across positions relative to the codon. Rows = codons, columns = positions (-20 to -3), color intensity = ribosome density.

**Biological Interpretation:**

**Codon-Specific Pausing vs. Amino Acid Pausing:**

- **Codon level provides finer resolution:**
  - Same amino acid encoded by different codons
  - Reveals codon usage bias effects
  - Identifies tRNA-specific limitations

**Key Biological Factors:**

1. **tRNA Abundance (Wobble Base Pairing):**
   - **Optimal codons:** Match abundant tRNAs, show less pausing
   - **Rare codons:** Limited tRNA availability, show more pausing
   - **Organism-specific:** Codon optimality varies by species
   - Examples in yeast:
     - AUA (Ile) is rare, shows strong pausing
     - CGA (Arg) is rare, shows strong pausing
     - AAA (Lys) is optimal, shows less pausing

2. **Wobble Position Effects:**
   - Third codon position (wobble) affects tRNA binding
   - G-U wobble pairs vs. perfect matches
   - Can cause different pausing for synonymous codons

3. **Codon Context Effects:**
   - Surrounding codons influence pausing
   - Some codon pairs are particularly slow
   - Codon-anticodon stability varies

4. **Regulatory Pausing:**
   - Specific codons may regulate gene expression
   - Slow elongation at specific sites allows:
     - Co-translational folding
     - Protein targeting
     - Quality control checkpoints

**Specific Codon Patterns:**

- **Proline codons (CCN):**
  - All show pausing, but CCC often strongest
  - Reflects both amino acid and codon effects

- **Rare codons:**
  - Species-specific patterns
  - Often cluster in genes requiring regulation
  - May be evolutionarily selected for control

- **Start codon (AUG):**
  - Special pattern near position 0
  - Reflects initiation dynamics

- **Stop codons (UAA, UAG, UGA):**
  - Visible in termination metagene
  - Different efficiencies and contexts

**Normalization Considerations:**

- **Row-normalized:** Shows preferred position per codon
- **Not normalized:** Reflects both usage frequency and pausing

**What to look for:**
- Rare codon hotspots
- Condition-specific changes in codon pause patterns
- Proline codon enrichment
- Organism-specific codon optimality patterns

---

### 6.2 Codon Protection Lineplot

**What it shows:** 
Line plot showing 5PSeq signal profile for a selected codon across positions, comparing multiple samples.

**Biological Interpretation:**

Similar principles to amino acid lineplot, but with codon-specific insights:

**Key Differences from Amino Acid Plots:**

1. **Synonymous Codon Comparison:**
   - Compare different codons for same amino acid
   - Identify which codons are truly problematic
   - Reveals tRNA pool limitations

2. **Codon Optimization Effects:**
   - Optimal vs. rare codon usage
   - Can test codon optimization strategies
   - Validate tRNA expression changes

**Biological Applications:**

- **tRNA availability studies:**
  - Overexpress specific tRNAs, measure pausing reduction
  - Deplete tRNAs, see increased pausing
  
- **Codon optimization validation:**
  - Compare wild-type vs. codon-optimized genes
  - Verify improved translation speed

- **Stress responses:**
  - Amino acid starvation affects specific codons
  - tRNA modifications change under stress

**What to look for:**
- Differences between synonymous codons
- Rare codon signatures
- Changes in codon-specific pausing between conditions

---

### 6.3 Codon Protection Scatterplot

**What it shows:** 
Scatterplot comparing 5PSeq signal for all 64 codons at a specific position between two samples.

**Biological Interpretation:**

**Similar to amino acid scatterplot but with codon resolution:**

**Advantages:**

1. **Identify specific tRNA limitations:**
   - Pinpoint which tRNA isoacceptors are limiting
   - Distinguish between synonymous codons
   - More precise than amino acid level

2. **Codon-specific interventions:**
   - Test tRNA overexpression effects
   - Validate codon optimization
   - Measure wobble base pairing efficiency

**Interpretation Patterns:**

- **Synonymous codons cluster:**
  - Should group together if amino acid is main determinant
  - Separation indicates tRNA-specific effects

- **Rare codons as outliers:**
  - Far from diagonal in tRNA limitation conditions
  - Return to diagonal when tRNA is supplied

**Discovery Opportunities:**

- **Condition-specific codon bottlenecks**
- **tRNA charging defects** (specific codon not amino acid)
- **Novel codon context effects**

**What to look for:**
- Specific rare codons showing strong effects
- Synonymous codon behavior (similar or different?)
- Magnitude of codon-specific changes
- Biological relevance to experimental manipulation

---

## 7. Gene Frame Preferences

### 7.1 Violin Plots (Frame Proportions)

**What it shows:** 
Violin plots displaying the distribution of frame proportions across all genes (with ≥50 total reads). Each plot shows one metric (e.g., F1_to_Fsum) for one sample.

**Biological Interpretation:**

**Available Metrics:**

1. **Frame-to-Total Ratios (F0_to_Fsum, F1_to_Fsum, F2_to_Fsum):**
   - Shows what proportion of reads fall in each frame
   - **F1_to_Fsum most important:** Should be ~0.6-0.8 in eukaryotes
   - Distribution width shows gene-to-gene variation

2. **Pairwise Frame Ratios:**
   - F1_to_F0, F1_to_F2: How much more F1 than others
   - F0_to_F1, F2_to_F1: Inverse ratios
   - F0_to_F2, F2_to_F0: Comparing minor frames

**Normal Eukaryotic Pattern:**

- **F1_to_Fsum violin:**
  - Peak around 0.7-0.8
  - Relatively narrow distribution
  - Few genes with very low F1_to_Fsum

**Biological Interpretation of Distribution Shape:**

1. **Narrow distribution:**
   - Homogeneous ribosome behavior across genes
   - Consistent translation dynamics
   - High-quality data

2. **Broad distribution:**
   - Heterogeneous frame usage
   - May reflect:
     - Diverse gene expression levels (low coverage genes noisier)
     - Different regulatory states across genes
     - Biological heterogeneity in ribosome positioning

3. **Bimodal distribution:**
   - Two populations of genes
   - May indicate:
     - Subset with altered translation (e.g., stress-responsive)
     - Technical artifacts in specific gene classes
     - Biological subgroups (highly vs. poorly translated)

**Gene-Level Insights:**

Unlike aggregate frame stats, violin plots reveal:
- **Individual gene behavior**
- **Population heterogeneity**
- **Outlier genes** with unusual frame preferences

**Biological Changes Between Conditions:**

- **Distribution shift:**
  - Entire population moves (global effect)
  - May reflect ribosome modifications
  - Could indicate stress response

- **Distribution broadening:**
  - Increased heterogeneity
  - Some genes more affected than others
  - Differential stress sensitivity

- **Appearance of new populations:**
  - Subset of genes change frame preference
  - May identify stress-responsive genes
  - Could reflect quality control activation

**What to look for:**
- F1_to_Fsum peaked around 0.7-0.8 (eukaryotes)
- Narrow, unimodal distributions (quality control)
- Consistent patterns across biological replicates
- Meaningful shifts between conditions

---

### 7.2 Ternary Plot (Frame Distribution Triangle)

**What it shows:** 
Triangular plot where each gene is a point positioned based on its F0, F1, and F2 proportions. The three corners represent 100% F0 (bottom left), 100% F1 (top), and 100% F2 (bottom right).

**Biological Interpretation:**

**Reading the Plot:**

- **Top of triangle (F1 corner):** Genes with strong F1 enrichment (normal)
- **Bottom left (F0 corner):** Genes with F0 preference (unusual)
- **Bottom right (F2 corner):** Genes with F2 preference (unusual)
- **Center:** Equal distribution across frames (poor coverage or noise)

**Normal Eukaryotic Pattern:**

- **Tight cluster near F1 corner**
- Most genes show 60-80% F1
- Few outliers toward F0 or F2

**Biological Interpretation of Patterns:**

1. **Tight cluster at F1:**
   - Healthy, actively translating genes
   - Consistent ribosome footprint
   - Normal translation dynamics

2. **Spread toward center:**
   - May indicate:
     - Low-coverage genes (noisy data)
     - Poorly translated genes
     - Genes undergoing quality control

3. **Distinct populations:**
   - Separate clusters suggest:
     - Different translation states
     - Subset of genes with altered ribosomes
     - Regulatory sub-classes

**Comparative Analysis:**

Comparing ternary plots between conditions:

- **Cluster tightening/loosening:**
  - Homogeneous vs. heterogeneous translation

- **Cluster migration:**
  - Shift toward F0 or F2 in specific conditions
  - May indicate stress, ribosome modifications, or altered degradation

- **Outlier populations:**
  - Genes particularly affected by treatment
  - Potential targets for further investigation

**Outlier Genes:**

Genes far from the main cluster are interesting:
- May represent truly different translation mechanisms
- Could be subject to specific regulation
- Might indicate specialized ribosome usage
- May be artifacts (low coverage, annotation errors)

**Integration with Other Data:**

Combine ternary plot with:
- Gene annotations (which genes are outliers?)
- Expression levels (coverage-dependent patterns?)
- Functional categories (pathways enriched in outliers?)

**What to look for:**
- Tight clustering at F1 corner (quality indicator)
- Minimal genes at F0/F2 corners
- Condition-specific shifts in cluster position
- Outlier genes for hypothesis generation

---

## 8. Summary and Integration

### Combining Multiple Plot Types

**Quality Control Workflow:**

1. **Mapping stats** → Sufficient depth and quality
2. **RNA composition** → Good mRNA enrichment
3. **Frame stats** → Strong F1 enrichment
4. **FFT periodicity** → Clear 3-nt periodicity
5. **Metagene profiles** → Expected initiation/termination peaks

All should be consistent for high-quality data.

**Biological Discovery Workflow:**

1. **Start with metagene plots** → Identify global changes in initiation/termination
2. **Check frame statistics** → Look for altered ribosome positioning
3. **Examine amino acid/codon heatmaps** → Identify specific pausing changes
4. **Use scatterplots** → Pinpoint most affected amino acids/codons
5. **Violin/ternary plots** → Understand gene-level heterogeneity

**Typical Biological Scenarios:**

**Amino Acid Starvation:**
- Specific amino acid shows increased pausing (heatmap, lineplot)
- Corresponding codons show elevated signal (codon plots)
- Global translation may slow (reduced initiation peak)
- Frame distribution may remain normal

**Translation Inhibitor Treatment:**
- Initiation peak changes (metagene START)
- May see ribosome accumulation at start sites
- Frame distribution may shift
- Periodicity may weaken

**Stress Response:**
- Global changes in RNA composition
- Altered initiation and termination dynamics
- Changes in specific amino acid pausing (e.g., proline)
- Possible frame shifts or loss of periodicity

**Ribosome Mutation/Modification:**
- Altered frame preferences
- Changed footprint position (visible in metagene, amino acid plots)
- Modified amino acid pausing patterns
- FFT periodicity changes

---

## Best Practices for Interpretation

1. **Always compare biological replicates** - Technical variation should be minimal

2. **Consider coverage** - Low coverage genes are noisier

3. **Use multiple plot types** - Triangulate findings across different visualizations

4. **Think mechanistically** - Connect observations to known translation biology

5. **Validate findings** - Use orthogonal methods (e.g., ribosome profiling, western blots)

6. **Consider alternatives** - Multiple biological mechanisms can produce similar patterns

7. **Account for technical factors** - Library prep, sequencing depth, mapping parameters

---

## Glossary

**5PSeq:** 5-prime phosphorylated mRNA sequencing, captures degradation intermediates

**CPM:** Counts Per Million mapped positions, normalization method

**F0, F1, F2:** Reading frames relative to codon positions

**FFT:** Fast Fourier Transform, frequency analysis method

**Metagene:** Average signal across all genes at aligned positions

**P-site:** Ribosome position where peptidyl-tRNA binds

**TSS:** Translation Start Site (AUG start codon)

**TERM:** Translation termination site (stop codon)

**Ribosome footprint:** Region of mRNA protected by ribosome from degradation

---

