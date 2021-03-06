# Hi-C Technology and Its Application
1. [Introduction](#1)<br>
    1.1 [Chromatin Structure](#11)<br>
2. [Hi-C protocol](#2)<br>
    2.1 [Hi-C Critical Steps](#21)<br>
    2.2 [3C Derivatives](#22)
3. [Data Analysis](#3)<br>
    3.1 [GITAR: an Open Source Tool for Analysis and Visualization of Hi-C Data](#31)<br>
    3.2 [Analytical Pipline](#32)<br>
       
    



## 1. Introduction<a name="1"></a>

The foundamental object of Hi-C, or any 3C(Chromosome Conformation Capture)-derived methods, is to understand the physical wiring diagram of the genome by identifying the physical interaction between chromosomes. 

#### 1) Chromatin Structure<a name="11"></a>

<img src="https://github.com/BEIBEICAO/BENG183/raw/master/Chromatin_Structures.png" /><br>

In the general chromatin structure, DNA wrapped around histone octamers to form nucleosomes. Nucleosomes then compacted into a chromatin fiber. Chromatin fiber can fold and interact with each other, forming the topologically associating domains (TADs) showing in the picture below. 

<img src="https://github.com/BEIBEICAO/BENG183/raw/master/Chromatin2.jpg" width="70%" height="70%" /><br>

The three-dimensional folding of chromosomes compartmentalizes the genome and can bring distant functional elements, such as promoters and enhancers, into close spatial proximity. **Deciphering the relationship between chromosome organization and genome activity will aid in understanding genomic processes, like transcription and replication.**
 
In order to study these interactions, the scientists have developed a series of chromatin conformation capture (3C) techniques since 2002. **Hi-C is An extension of 3C that is capable of identifying long range chromatin interactions in an unbiased, genome-wide fashion.** <br>

Hi-C couples proximity ligation and do massively parallel sequencing, therefore it has the following advangtages:
- **All v.s. all**
    - In 3C derived techniques, ‘1’, ‘Many’ and ‘All’ indicate how many loci are interrogated in a given experiment. For example, ‘1 versus All’ indicates that the experiment probes the interaction profile between 1 locus and all other potential loci in the genome. ‘All versus All’ means that one can detect the interaction profiles of all loci, genome-wide, and their interactions with all other genomic loci. By the time when 3C was first developed, we were only able to analysis ‘one versus one’ loci interactions.
- **Doesn’t require any prior knowledge**
- **High through-put**
    - Hi-C techniques has the highest through-put (billion reads per sample) of 3C-derived technologies. Due to the decreasing cost of 2nd generation sequencing, Hi-c is widely used. 
- **Study genomic architecture at multiple scales**
    - From initial results identified features such as chromosome territories, to the segregation of open and closed chromatin, and finally chromatin structure at the megabase scale.




## 2. Hi-C Protocol<a name="2"></a> 

#### 1) Hi-C Critical Steps<a name="21"></a>
<img src="https://github.com/BEIBEICAO/BENG183/raw/master/protocol.png">

- **Fixation: keep DNA conformed** 
> Cells are fixed with formaldehyde, causing interacting loci to be bound to one another by means of covalent DNA-protein cross-links. 
- **Digestion: enzyme frequency and penetratin**
> The genome is then cut into fragments with a restriction endonuclease while the interacting loci remain linked. The size of restriction fragments determines the resolution of interaction mapping. 
- **Fill-in: biotin for junction enrichment**
> A biotinylated residue is incorporated as the 5' overhangs are filled in. 
- **Ligation: freeze interactions in sequence**
> Blunt-end ligation is performed under dilute conditions that favor ligation events between cross-linked DNA fragments.
- **Biotin removal: junctions only**
> Remove Biotin from un-ligated ends to produce DNA fragments that will enable paired-end sequencing. This step results in a genome-wide library of ligation products, corresponding to pairs of fragments that were originally in close proximity to each other in the nucleus. 
Each ligation product is marked with biotin at the site of the junction. 
- **Fragment size and pull-down: small fragments sequence better**
> The library is sheared, and the junctions are pulled-down with streptavidin beads, allowing for ligation product enrichment prior to adapter ligation. 
- **Adapter ligation: paired-end and indexing**
> The purified junctions with adapter ligated can subsequently be analyzed using a high-throughput sequencer, resulting in a catalog of interacting fragments.

#### 2) 3C Derivatives<a name="22"></a>
<img src="https://github.com/BEIBEICAO/BENG183/raw/master/Chromosome_conformation_techniques.jpg">




## 3. Data Analysis<a name="3"></a> 

### 1) GITAR: an Open Source Tool for Analysis and Visualization of Hi-C Data<a name="31"></a>

GITAR (Genome Interaction Tools and Resources), is a software to perform a comprehensive Hi-C data analysis, including data preprocessing, normalization, visualization and topologically associated domains (TADs) analysis. GITAR is composed of two main modules: 
- **HiCtool**, a Python library to process and visualize Hi-C data, including TADs analysis and 
- **Processed data library**, a large collection of human and mouse datasets processed using HiCtool.

GITAR enables to work with Hi-C data even without any programming or bioinformatic expertise and it is available online at www.genomegitar.org as an open source software.

### 2) Analytical Pipline<a name="32"></a>

**HiCtool wrokflow:**<br>
<img src="https://github.com/BEIBEICAO/BENG183/raw/master/HiCtool.png"><br>

#### Part I: Data Pre-processing<a name="321"></a>

- **Downloading the source data from GEO**
> Source data (sra format) can be downloaded via GEO accession number using the command **fastq-dump**.<br>
More options on [SRA Toolkit](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=fastq-dump).
- **Paired-end sequencing** 
> Split paired-reads SRA data into SRRXXXXXXX_1.fastq and SRRXXXXXXX_2.fastq, and SRRXXXXXXX.fastq (if present) contains reads with no mates. Paired-end sequencing allows users to sequence both ends of a fragment and generate high-quality, alignable sequence data. Paired-end sequencing facilitates detection of genomic rearrangements and repetitive sequence elements, as well as gene fusions and novel transcripts.
- **Pre-truncation of the reads**
> Truncate read ends at the ligation site (if present) and keep the longest piece without the junction sequence to improve the mapping outcome. The rationale is to remove bases that would otherwise prevent a read mapping to the specified reference genome, or mapping but with lower quality. More instructions see link below.
- **Independent alignment of pairs (Mapping to reference genome)**
> [Bowtie 2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml) is used for mapping the read pairs, and reads are mapped independently to **avoid any proximity constraint**.<br>
- **Filtering reads and selecting reads that are paired**
> Remove unmapped or low quality mapped reads (MAPQ < 30). Create bam files that will serve as inputs for the normalization pipeline. More instructions see link below.
- **Fragment end file generation for specific species and restriction enzyme**
> Create the fragment-end (FEND) bed file, which is used to normalize the data （technical & biological biases） and contains restriction site coordinates and additional information related to fragment properties (GC content and mappability score). More instructions see link below.<br>

*[More instructions here](https://doc.genomegitar.org/preprocessing_data.html)

#### Part II: Data Analysis and Visualization<a name="322"></a>

 <table>
  <tbody>
    <tr>
        <th></td>
        <th>Step</td>
        <th>Function</td>
    </tr>
    <tr>
        <td>1</td>
        <td>Create the Fend object</td>
        <td>Fend object Transform bed file to FEND object (which containing RE information like coordinates, GC content and mappability score).</td>
    </tr>
     <tr>
        <td>2</td>
        <td>Create the HiCData object</td>
        <td>It is where the information from mapped reads and fragments merged. Unwanted paired-reads (like total distance to their respective restriction sites exceeds threshold, PCR duplicates, incomplete restriction enzyme digestion and fragment) are removed.</td>
    </tr>
    <tr>
        <td>3</td>
        <td>Create the HiC project object</td>
        <td>The HiC project object (hdf5 format) links the HiCData object with information about which fends to include in the analysis</td>
    </tr>
    <tr>
        <td>4</td>
        <td>Create the HiC project object</td>
        <td>Filter out fragments that do not have any interaction before learning correction parameters.</td>
    </tr>
    <tr>
        <td>5</td>
        <td>Estimate the HiC distance function</td>
        <td>Estimation of the distance-dependence relationship from the data prior to normalization.  Due to unevenly distributed restriction sites,  fragments surrounded by shorter ones will show higher nearby interactions than those with longer adjacent fragments</td>
    </tr>
    <tr>
        <td>6</td>
        <td>Learn the correction model</td>
        <td>Take into account of fragments length, inter-fragment distance, GC content and mappability score biases to learn the correction model for Hi-C data. (Yaffe E. and Tanay A., 2011). In addition, biological biases are considered at this step (TSSs and CTCF bound sites).</td>
    </tr>
    <tr>
        <td>7</td>
        <td>Normalize the data</td>
        <td>For the normalization, observed data and correction parameters to remove the biases to obtain the corrected read counts are required. Therefore, the observed contact matrix and the fend expected contact matrix are calculated. In addition, the enrichment expected contact matrix is calculated to compute the observed over expected enrichment values, considering also the distance between fends.</td>
    </tr>
    <tr>
        <td>8</td>
        <td>Visualize the data</td>
        <td>This part is to plot the heatmap and histogram for the normalized contact data.  </td>
    </tr>
 </tbody>
</table>

> 1. The script HiCtool_hifive.py can be used to run all the HiFive steps (1-6), whose outputs are .hdf5 files. For more information about these functions, please see [HiFive’s API documentation](https://hifive-analysis-software.readthedocs.io/en/latest/).<br>
> 2. About step 8: By bining the genome into small regions (‘’loci’’, usually 1MB) and defining the matrix entry mij to be the number of ligation products between locus i and locus j, it can be visually represented as a heatmap, with intensity indicating contact frequency (for Intra-chromosome contact matrix; Inter-chromosome contact matrix is about different chromosome interaction). After learning the correction parameters two matrices are computed per each chromosome at a specific bin size: Observed contact matrix O[i,j] where each entry contains the observed read count between the regions identified by the bins i and j; Correction matrix E[i,j] where each entry contains the sum of corrections for the read pairs between bins i and j. Then, the normalized contact matrix N[i,j] contains the corrected contact counts in each entry and it is calculated as: N[i,j] = O[i,j]/E[i,j].
> 3. *[More instructions here](https://doc.genomegitar.org/data_analysis_and_visualization.html).
<br>

**Result Sample Figure**<br>
<img src = "https://github.com/BEIBEICAO/BENG183/raw/master/result%20plot.png" width="70%" height="70%">
> A: Normalized contact matrix of chromosome 6 from 50 Mb to 54 Mb with a bin size of 40 kb and the respective histogram of the contact distribution.
This part is to plot the heatmap and histogram for the normalized contact data.<br>
> B: Enrichment normalized contact matrix of chromosome 6 from 50 Mb to 54 Mb with a bin size of 40 kb and the respective histogram.
This part is to plot the heatmap and histogram for the enrichment normalized data (“observed over expected”). The log2 of the data is plotted to quantify the positive enrichment (red) and the negative enrichment (blue). Loci (pixels) equal to zero before performing the log2 (deriving from zero observed contacts) are shown in gray. Loci (pixels) where enrichment expected contact was zero before performing the ratio (observed / expected) are shown in black.

#### Part III: Topological Domains Analysis<a name="323"></a>

Topologically associating domains (TADs): 
- Highly self-interacting regions at the level of hundreds of kilobases (~10^5 bases) to few megabases (~10^6 bases).
- Separated by boundaries that prevent interactions with the neighboring regions.
- They appear as triangles along the diagonal of the heatmap (A).<br>
<img src ="https://github.com/BEIBEICAO/BENG183/raw/master/TAD.png" width="70%" height="70%">

- [Directionality Index (DI)](https://doc.genomegitar.org/DI_calculation.html?highlight=tad) per each 40 kb bin is proportional to the difference between downstream and upstream biases of contact counts up to 2 Mb (50 bins).
- A [Hidden Markov Model (HMM)](https://en.wikipedia.org/wiki/Hidden_Markov_model) is used to determine the underlying biased state for each locus (upstream, downstream or none).
- Shifts of the “true” DI (from negative to positive) allows to identify topological domain boundaries and therefore topological domain coordinates (B: start – end):<br>
    50080000
    50600000

    50640000
    51760000

    51840000
    52000000

    52080000
    52680000

    52800000
    53040000

    53120000
    53760000<br>
<img src ="https://github.com/BEIBEICAO/BENG183/raw/master/DI.png" width="70%" height="70%">

#
**GITAR is a comprehensive framework and it consists of a standardized pipeline to analyze and visualize Hi-C data and a large collection of processed datasets.**

No installation needed, except for the Python libraries and software required:

- [Bowtie 2](http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml)
- [SAMTools](http://www.htslib.org/doc/samtools-1.2.html)
- [BEDTools](https://bedtools.readthedocs.io/en/latest/)
- [SRA Toolkit](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=fastq-dump)





























# Referrence
[1] Calandrelli, Riccardo, et al. “GITAR: an Open Source Tool for Analysis and Visualization of Hi-C Data.” Apr. 2018, doi:10.1101/259515.<br>

[2] Van Berkum, N. L., Lieberman-Aiden, E., Williams, L., Imakaev, M., Gnirke, A., Mirny, L. A., Dekker, J., Lander, E. S. Hi-C: A Method to Study the Three-dimensional Architecture of Genomes.. J. Vis. Exp. (39), e1869, doi:10.3791/1869 (2010).<br>

[3] https://github.com/hms-dbmi/hic-data-analysis-bootcamp/blob/master/HiC-Protocol.pptx<br>

[4] https://zhonglab.gitbook.io/3dgenome/chapter2-computational-analysis/3.2-higer-order-data-analysis/analytical-pipelines<br>

[5] Hoffman EA, et al. (2015) Break-seq reveals hydroxyurea-induced chromosome fragility as a result of unscheduled conflict between DNA replication and transcription. Genome Res 25(3): 402-12<br>

[6] https://doc.genomegitar.org/data_analysis_and_visualization.html<br>

[7] https://doc.genomegitar.org/data_analysis_and_visualization.html<br>

[8] https://drive.google.com/file/d/1nxP13mOIer_6yiLi7OFXHbQ4-D3Eqq4k/view




