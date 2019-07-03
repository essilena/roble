# Introduction
The project consists of analyzing those 4-shotgun datasets of the microbial community from the rhizospheres of the *Tabebuia heterophylla*. The main objective is to find microbial differences between the 4 soil samples of the “roble” *Tabebuia heterophylla*. The samples were collected in 2012, in 4 towns in Puerto Rico, and in 3 different soil types. The towns are Guayama; in low altitude volcanic soil, Guánica; in low altitude limestone soil, Maricao; in high altitude serpentine soil and Cabo Rojo; in low altitude serpentine soil. They collected a sample of each one, except for the serpentine we have from a high and low altitude zone. We want to analyze and establish the differences in microbial composition, of bacteria, fungi, prokaryotes and microeukaryotes between different sites and geology. Once we can identify those groups, we will establish which genes are present in each one, and what do they code. Also, we want to identify the metabolism of these genomes, see if there are groups present with nitrogen fixing capacity, also to be able to identify microbes that convert plant biomass to sugars, identify genes of Crispr/cas and genes resistant to antibiotics. In order to that we are doing the following:

# What are we doing?
In order to carry out the research, we went to GOLD, which is where the samples is available, since “Genomes Online Database”, which is a part of the World Wide Web, it is a public database that gives us access to the genome and metagenome sequencing projects uploaded there (Mukherjee, Stamatis et al.).  However, the data was extracted from SRA, which is a bioinformatics database that functions as a public repository for DNA sequencing data which provides shorts reads (Sequence Read Archive). In order to understand the first part of the experiment and be able to continue with the metagenomic part of the project that is what we are going to do this summer. For doing that we have several steps. The first step is that I will read the protocols of the programs they used to carry out the genome assembly, which in this case was SPAdes an assemble that was designed for single cell and multi-cells sequencing, providing information about genomes (Bankevich et al.). In addition, those used for gene calling methods: FragGeneScan version 1.16, which is a gene predictor, functions as predicting fragmented genes and genes with frameshifts in addition to the complex genes (Rho et al.), Prokaryotic GeneMark.hmm version 2.8, also a gene predictor, designed to improve the gene prediction quality in terms of finding exact gene boundaries (Lukashin and Borodovsky), Metagene Annotator version 1.0, which predicts all kinds of prokaryotic genes froms a single or a set of genomic sequences (Noguchi, Tanigu) (Noguchi, Park, et al.) and Prodigal version 2.5, that can identify genes in short coding sequences with a high degree of accuracy  (Hyatt et al.).

When I finally end reading all the papers, to acknowledge what and how they did the sequencing, we start working in the terminal.

In the terminal I got access to the mega-computer called “Boqueron” and in that way we then can downloaded the data from SRA to “Boqueron". Before having access to it, we downloaded miniconda3, which is a system that handles packages that contain many programs that perform different functions. 

When we saw the data, we saw that it was very large, the files had approximate 60GB, as I mentioned there are 4 samples of 60GB each, they were too big. We spent a whole week trying to download the files, because the connection between the computer “NCBI” and “Boqueron” was failing. Therefore, Humberto had to download them to a supercomputer called "Hulk" and then transfer them to "Boquerón". Then we run a command called “fastq-dump” to compress the data. We try doing “fasterq-dump” but it didn’t work. We achieve to compress each file approximate to 20GB. We open our environment called “Roble” and there we created a directory called "quality" that had 2 files one called “raw” (where we deposited the raw data” and the other one “trimmed” (where we are going to deposit the data after the quality control.
## Quality Control
- Therefore, Humberto wrote a script to be able to see a report of the samples before doing the quality control, to see how they were and compare them with the results once they went through the quality control. This script ran the program called "FASTQC".


- After about 2 hours, we obtained the reports of each sample that they told us were in poor condition. Then we ran "MULTIQC" which is another program that gives us a general report of all the samples together. We obtained the same results, but in a general way.
We realized that there were many repeated sequences and that they were due to the adapters that are put to the extracted DNA when it is processed by ILUMINA, which is what produces the sequencing. Then, in order to obtain the cleanest possible sequencing and the most compressed we decided to do quality control, which is recommended to do, even once. 

- To explore what different adapters were present in the samples we run a script called "findadapter".


- Then knowing what adapters where present, we ran a script that Humberto wrote to trim the data and eliminating the adapters. Is important to notice that we ran the trim script with a quality of 5, so that way we would eliminate all sequences less that a quality of 5.

- We make a file called “Combinados” that is run inside the script trim, that contained all the adapters, in order to eliminated them.

- We waited for a full day and got the trim, then we ran the FASTQC and MULTIQC scripts again with the data trim, to see how the samples had been given and decide if we were going to run the trim again.
Seeing the report, we decided that it was all good, the samples were in good condition, so we moved the data to the directory “quality/trimmed”.

## Assemblies
- We decided that it was time to create the assemblies, referring to aligning and merging fragments from a longer DNA sequence in order to reconstruct the original sequence. We had to install the three assemblers that we would use with Conda, creating a folder for each one. We will use Megahit, an ultra-fast single-node solution for large and complex metagenomics assembly via succinct de Bruijn graph.

- We had several problems creating the script, but with a little of work we solve in a way that the program could read it.
- Then we did the same with PLASS, which is another assembler, but it translates the sequences into protein sequences.

- Finally, we did the same with MetaSpades, which is an assembler too, which is part of Spades. We had to join all the _1 sequence and put it to getter in a folder called “left”. And then the sequences that are the reverse _2 in a folder called “right”. Since this assembler read the command/script, for 2 folders to create the assembly.

- The 3-assembler failed, because they needed more memory than the one, we give to each one. Therefore, we decided to do a "Digital Normalization" that eliminates possible errors and reduce the sequences if there are too many or too few. This process is also part of the quality control. 

## Beginning Digital Normalization

- The “Digital Normalization” (interleave-reads.py) was done, but we had to eliminate the results. First because the 3rd sample gives us an error. And secondly, because in the first script and we forgot to put --gzip and --no-reformat (so when we runned again, it doesn't change the names of the sequences, nor check if they are paired sequences. So here the new script with the corrections:

## Note
- Also, we found in the web page of IMG under the name of “Tabebuia heterophylla” the assemblies of the project made by other person.  (https://img.jgi.doe.gov/cgi-bin/m/main.cgi?section=FindGenomes&page=displayTaxonList&searchFilter=all&searchTerm=Tabebuia%20heterophylla&file=all78716&allDataFiltersFile=allGenomeDataFilters78716)

## Interleave

- Well, the command that we gave with the interleave.sh script did not work. Because when we ran the script that we wrote to do normalize, it told us that the samples were not paired.
- Therefore, we had to download a copy of the khmer tool script and modified, so that when we ran our interleave.sh script and got the results, we could then run normalize.sh script.

-To explain it better, I will use the same samples as visual. All this happened since we ran the quality control with the fastq-dump script and fasterq-dump. We ran the first 2 samples (SRR5256888 and SRR5256985) with fastq-dump. But to explore whether quality control could be simplified and much better, we used the fasterq-dump, that is the update of fastq-dump. But there was a difference between the samples. Since each program put different names to the samples. In the 3rd samples name there's an extra period and an extra number compared to the names of the other samples. We decided to run the fourth sample with fastq-dump. 

- The problem is that as mentioned previously the 3rd sample gives us an error and for this small problem we had to modify the scripts. Therefore, we order conda to install Khmer, in order to modify the script of Khmer tools, so interleave could process the data and fix the files names.  The script is called: interleave-reads.py, we modified part of the script. The modification, was a command to modify the name of the sample, in order to process and that at the end t all the names of each sample have the same format. We commanded that the program split the name as follow @ SRRxxx.yy -yy- lengh = 150 and eliminate the part after the second period. Then we include the modified script called "interleave-reads.py" in our script called: "interleve.sh". that turned the samples name SRRxxx.#/#

## Normalize Script
- Therefore, we decided to run the normalize script "normalize-by-median.py" For the script we modify the memory and the time, because we knew that it was going to take a lot of memory and time.

## Next Step

- Right now, we are waiting for those results, and we thinking that our next step should be to do a partition (which is what we will probably do) or run the assemblies again (Megahit, PLASS and MetaSPAdes).

