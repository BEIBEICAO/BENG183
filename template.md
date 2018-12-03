# Hi-C Technology and Its Application
1. [Introduction](#1)<br>
    1.1 [Chromatin Structure](#11)<br>
2. [Hi-C protocol](#2)
3. [Data Analysis](#233)
4. [Application](#234)




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
    - In 3C derived techniques, ‘1’, ‘Many’ and ‘All’ indicate how many loci are interrogated in a given experiment. For example, ‘1 versus All’ indicates that the experiment probes the interaction profile between 1 locus and all other potential loci in the genome. ‘All versus All’ means that one can detect the interaction profiles of all loci, genome-wide, and their interactions with all other genomic loci [1]. By the time when 3C was first developed, we were only able to analysis ‘one versus one’ loci interactions.
- **Doesn’t require any prior knowledge**
- **High through-put**
    - Hi-C techniques has the highest through-put (billion reads per sample) of 3C-derived technologies. Due to the decreasing cost of 2nd generation sequencing, Hi-c is widely used. 
- **Study genomic architecture at multiple scales**
    - From initial results identified features such as chromosome territories, to the segregation of open and closed chromatin, and finally chromatin structure at the megabase scale.




## 2. Hi-C protocol<a name="2"></a>

<img src="https://github.com/BEIBEICAO/BENG183/raw/master/protocol.png">






https://en.wikipedia.org/wiki/File:Chromosome_conformation_techniques.jpg
##### Hi-C critical steps [8] 
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
> The library is sheared, and the junctions are pulled-down with streptavidin beads,allowing for ligation product enrichment prior to adapter ligation. 
- **Adapter ligation: paired-end and indexing**
> The purified junctions with adapter ligated can subsequently be analyzed using a high-throughput sequencer, resulting in a catalog of interacting fragments.

##### Hi-C derived techniques 
- Hi-C original: [Lieberman-Aiden et al., Science 2010](doi: 10.1126/science.1181369)
- Hi-C 1.0: [Belton-JM et al., Methods 2012](doi: 10.1016/j.ymeth.2012.05.001)
- In situ Hi-C: [Rao et al., Cell 2014](doi: 10.1016/j.cell.2014.11.021)
- Single cell Hi-C: [Nagano et al., Genome Biology 2015](https://doi.org/10.1186/s13059-015-0753-7)
- DNase Hi-C [Ma, Wenxiu, Methods et al](https://www.ncbi.nlm.nih.gov/pubmed/25437436)
- Hi-C 2.0: [Belaghzal et al., Methods 2017](https://www.ncbi.nlm.nih.gov/pubmed/28435001)
- DLO-Hi-C: [Lin et al., Nature Genetics 2018](https://doi.org/10.1038/s41588-018-0111-2)
- Hi-C improving: [Golloshi et al., Methods 2018](https://www.biorxiv.org/content/biorxiv/early/2018/02/13/264515.full.pdf)
- Arima 1-day Hi-C: [Ghurye et al., BioRxiv 2018](https://www.biorxiv.org/content/early/2018/02/07/261149)

## 2.3.4 ChIA-PET<a name="234"></a> 
ChIA-PET is another method that combines ChIP and pair-end sequencing to analysis the chromtin interaction. It allows for targeted binding factors such as: estrogen receptor alpha, CTCF-mediated loops, RNA polymerase II, and a combination of key architectural factors. on the one hand, it has the benefit of achieving a higher resolution compared to Hi-C, as only ligation products involving the immunoprecipitated molecule are sequenced, on the other hand, ChIA-PET has systematic biases due to ChIP process:
- Only one type of binding factor selected
- Different antibodies
- ChIP conditions


## 2.3.5 Selected methods comparison<a name="235"></a> 
<table>
 <tbody>
    <tr>
        <th>Method</td>
        <th>Targets</td>
        <th>Resolution</td>
        <th>Notes</td>
    </tr>
    <tr>
        <td>3C <a href="http://refhub.elsevier.com/S2001-0370(17)30093-4/rf0535">[3]</a></td>
        <td>one-vs-one</td>
        <td>~1–10 kb<br></td>
        <td><ul><li>Sequence of bait locus must be known</li><li>Easy data analysis</li><li>Low throughput</li></ul></td>
    </tr>
    <tr>
    <td>4C <a href="http://refhub.elsevier.com/S2001-0370(17)30093-4/rf0545">[4]</a></td>
    <td>one-vs-all</td>
    <td>~2 kb</td>
    <td><ul><li>Sequence of bait locus must be known</li><li>Detects novel contacts</li><li>Long-range contacts</li></ul></td>
    </tr>
    <tr>
    <td>5C <a href="http://refhub.elsevier.com/S2001-0370(17)30093-4/rf0550">[5]</a></td>
    <td>many-vs-many</td>
    <td>~1 kb</td>
    <td><ul><li>High dynamic range</li><li>Complete contact map of a locus</li><li>3C with ligation-mediated amplification (LMA) of a ‘carbon copy’ library of oligos designed across restriction fragment junctions of interest
3C</li></ul></td>
    </tr>
    <tr>
    <td>Hi-C <a href="http://refhub.elsevier.com/S2001-0370(17)30093-4/rf0300">[6]</a></td>
    <td>all-vs-all</td>
    <td>0.1–1 Mb</td>
    <td><ul><li>Genome-wide nucleosome core positioning</li><li>Relative low resolution</li><li>High cost</li></ul></td>
    </tr>
    <tr>
    <td>ChIA-PET <a href="http://refhub.elsevier.com/S0168-9525(15)00063-3/sbref1405">[7]</a></td>
    <td>Interaction of whole genome mediated by protein</td>
    <td>Depends on read depth and the size of the genome region bound by the protein of interest</td>
    <td><ul><li>Lower noise with ChIP</li><li>Biased method since selected protein</li></ul></td>
    </tr>
 </tbody>
</table>

















# Referrence
[1] Schmitt, Anthony D., Ming Hu, and Bing Ren. "Genome-wide mapping and analysis of chromosome architecture." Nature reviews Molecular cell biology 17.12 (2016): 743.<br>

[2] Risca, Viviana I., and William J. Greenleaf. "Unraveling the 3D genome: genomics tools for multiscale exploration." Trends in Genetics 31.7 (2015): 357-372.<br>

[3] Dekker J, Rippe K, Dekker M, Kleckner N. Capturing chromosome conformation. Science 2002;295(5558):1306–11.<br>

[4] Simonis M, Klous P, Homminga I, Galjaard RJ, Rijkers EJ, Grosveld F, et al. High-res- olution identification of balanced and complex chromosomal rearrangements by 4C technology. Nature Methods 2009;6(11):837–42.<br>

[5] Dostie J, Richmond TA, Arnaout RA, Selzer RR, Lee WL, Honan TA, et al. Chromo- some Conformation Capture Carbon Copy (5C): a massively parallel solution for mapping interactions between genomic elements. Genome Res 2006;16(10): 1299–309.<br>

[6] Lieberman-Aiden E, van Berkum NL, Williams L, Imakaev M, Ragoczy T, Telling A, et al. Comprehensive mapping of long-range interactions reveals folding principles of the human genome. Science 2009;326(5950):289–93.<br>

[7] Fullwood, M.J. et al. (2009) An oestrogen-receptor-alpha-bound human chromatin interactome. Nature 462, 58–64.<br>

[8] https://github.com/hms-dbmi/hic-data-analysis-bootcamp/blob/master/HiC-Protocol.pptx.


