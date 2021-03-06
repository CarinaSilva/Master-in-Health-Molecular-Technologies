# Exercises - Introduction to NGS Data Analysis

**Note** The exercises presented in this class are adapted from Daniel Sobral material course @ IGC from the [Git Hub](https://github.com/GTPB/ELB18F) and from the [on line course](http://www.ngscourse.org/).

> ## FASTQ files


**Exercise 1**:Consider the following read in the fastq format:

	@SRR022885.1 BI:080102_SL-XAR_0001_FC201E9AAXX:6:1:752:593/1
	CGTACCAATTATTCAACGTCGCCAGTTGCTTCATGT
	+
	IIIIIIIIII>IIIIIII@IIII.I+I>35I0I&+/
	
<br/>

**NOTE**: Phred+33 (Sanger fastq) is the current standard format. Nonetheless, with older illumina data (before 2009) preferred to start at the character '@' (ASCII: 64) instead of '!'. This Phred+64 format is the old illumina fastq. Some tools (like FastQC) can infer the format, while in others you need to specify.


<br/>

**a)** What is the probability of error of the first base of the read?
<details><summary>Click Here to see the answer</summary><p>
The base quality character is 'I', which corresponds to the decimal 73 in the ASCII table. Q = 73-33 = 40. P(40) = 10^(-40/10) = 10^-4 = 0.01% error.
</p></details>

<br/>

**b)**: What is the probability of error of the last base of the read?
<details><summary>Click Here to see the answer</summary><p>
The base quality character is '/', which corresponds to the decimal 47 in the ASCII table. Q = 47-33 = 14. P(14) = 10^(-14/10) = 10^-1.4 ~= 4% error.
</p></details>
 <br/>

**c)**: If all bases of a ficticious machine had a Q=20 (1% probability of error), what would be the probability that one 100bp read from that machine would be completely correct?
<details><summary>Click Here to see the answer</summary><p>
P(correct)=(0.99)^100 ~= 36.6%!

This serves to exemplify that most reads in current sequencing machines are likely to have at least one base incorrect.
</p></details>
<br/>

> **Exercise Sup1** 

Consider the following read in the fastq format and obtain the quality score:
<br/>


![](fig1.png)



> ## Quality Control (QC)

High Throughput Sequencing machines read thousands or millions of sequences in parallel. As you can imagine, this usually generates large fastq files, with millions of lines. Manually inspecting the quality of each read is out of the question. Specialized software has been developed to provide quality measures for fastq files generated by HTS machines. [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) is a popular program to generate quality reports on fastq data. In fact, this is usually the first thing you should do once you receive a new dataset. FastQC reports provide a series of plots that allow the user to assess the overall quality of their raw data and detect potential biases and problems. 


Some plots indicate distribution of base qualities along the length of reads. At least for illumina data, on average the quality of each base tends to decrease along the length of the read. 


Other plots indicate biases in nucleotidic content of reads, either globally (such as %GC plots), or positionally. Global bias in nucleotidic content can be useful to search for signs of contaminants. On the other hand, positional bias are useful to detect presence of artefactual sequences in your reads such as adaptors. Another insight you may obtain from this information are potential biases in the preparation of your library. For example, random hexamer priming is actually not truly random, and preferentially selects certain sequences. The currently popular transposase-based enzymatic protocol, although reasonably random, is also not completely random, and you can see this through positional bias, particularly in the beginning of reads. The presence of adaptors is a relatively common event, and therefore specific plots exist to detect the presence of the most commonly used adaptors. Finally, the presence of repetitive sequences can also suggest contaminants, pcr artifacts, or other types of bias.


**Exercise 2** Install [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) on your computer.

<br/>

**Exercise 3**: Download the file MiSeq_76bp.fastq.gz inside of the folder [data](https://github.com/CarinaSilva/Master-in-Health-Molecular-Technologies/tree/master/Exercises/NGS/Data). Look at the different plots you obtained. Next, download the file MiSeq_250bp.fastq.gz.

<br/>

**NOTE**: Given the size of fastq files (usually in the order of Gb), they are most frequently compressed as fastq.gz files. In fact, most tools (such as FastQC) work directly with fastq.gz to reduce space.


<br/>


**Exercise 4**: What are the main differences between the reports of both fastq files?

<details><summary>Click Here to see the answer</summary>
	

The MiSeq_250bp fastq file contains 10000 reads of 250bp, while the MiSeq_76bp contains 1000 reads of 76bp. The MiSeq_250bp reads have a lower per base sequence quality at their end, while the reads of the MiSeq_76bp keep a good quality throughout. The MiSeq_76bp reads contain a very noticeable nucleotide positional bias particularly after position 36. MiSeq_250bp also contain a bit of nucleotide positional bias, but less and only for the first 10bp. The MiSeq_250bp reads display an apparently bimodal GC distribution, while the MiSeq_76bp reads seem closer to a single normal distribution. Finally, MiSeq_76bp contain a clear presence of a known Illumina adaptor after position 36 (probably the reason for the nucleotide positional bias we saw before), while MiSeq_250bp contain a much smaller frequency of another Illumina adaptor towards the ends of the reads.

</p></details>
<br/>



**Exercise 5**: What is the major difference between the two paired fastq files of the paired_example?

<details><summary>Click Here to see the answer</summary>
	
The reverse read has poorer quality bases. This is usually the case, at least for illumina. This is because the reverse reads are generated after the forward reads.

</p></details>
<br/>




**Exercise 6**: Insider the folder fastq_examples, you can see fastq files from differente sequencing technologies or applications. Can you see differences between the different sequencing technologies?

<details><summary>Click Here to see the answer</summary>
	
Illumina machines generate shorter reads, all with the same length. Pacbio and nanopore generate (much) longer reads, with diverse read lengths, and of a poorer quality. Illumina generates many more reads, making both technologies complementary to each other (this will become clearer when we look at specific applications). Finally, you can also notice that, independently of the technology, the quality of base quality tends to decrease along the length of the read. 

</p></details>
<br/>


**NOTE**:  Assess how well you achieve the learning outcome. For this, make the following questions to yourself:

+Could you run FastQC on a fastq file?

+Can you broadly list types of information that a FastQC report contains?

+Can you interpret information in a FastQC report to detect potential issues with data in a fastq file?

<br/>
<br/>


> ## Filtering and Trimming
  


Most software for the analysis of HTS data is freely available to users. Nonetheless, they often require the use of the command line in a Unix-like environment (seqtk is one such case). User-friendly desktop software such as [CLC](https://www.qiagenbioinformatics.com/products/clc-genomics-workbench/) or [Ugene](http://ugene.net/) is available, but given the quick pace of developmpent in this area, they are constantly outdated. Moreover, even with better algorithms, HTS analysis must often be run in external servers due to the heavy computational requirements. One popular tool is [Galaxy](https://galaxyproject.org/), which allows even non-expert users to execute many different HTS analysis programs through a simple web interface.

**TASK**: Let's use Galaxy to run some bioinformatic tools. Open the [url](https://usegalaxy.org/) on your browser. You should see the Galaxy interface on your web browser. Click on the upload icon on the top left of the interface. Upload into Galaxy the files MiSeq_76bp.fastq.gz and MiSeq_250bp.fastq.gz. You should now seem them on your history in the right panel. You can visualize their content by pressing the view data icon (the eye icon). After you have your data, you're ready to run some tools on your data. The tools are listed on the left panel. Search for fastqc on the tool search bar on the left panel. By clicking on the tool you should have in the middle the interface to run fastQC. To run fastc you just need to select the fastq file and press "Execute". Run fastqc on the fastq files you uploaded and see the result. Still in galaxy again, search for and run "seqtk trimfq" on the file MiSeq_250bp.fastq with the same parameters as you used in the command line. 


As we saw before, sequencing machines (namely, the illumina ones) require that you add specific sequences (adaptors) to your DNA so that it can be sequenced. For many different reasons, such sequences may end up in your read, and you need to remove these artifacts from your sequences.

**Exercise 7**: How can adaptors appear in your sequences? Take the sample MiSeq_76bp.fastq.gz as an example. 
<details><summary>Click Here to see the answer</summary>
	When the fragment being read is smaller than the number of bases the sequencing machine reads, then it will start reading the bases of the adaptor that is attached to all fragments so they can be read by the machines. In the case of MiSeq_76bp, the fragments were all 36bp, and since 76bp were being read, the remaining bases belong to the illumina adaptor that was used.
</details>
<br/>

There are many programs to remove adaptors from your sequences, such as [cutadapt](https://cutadapt.readthedocs.org/en/stable/). To use them you need to know the adaptors that were used in your library preparation (eg. Illumina TruSeq). For this you need to ask the sequencing center that generated your data.

**Exercise 8**: In Galaxy, use cutadapt to remove adaptors from MiSeq_76bp.fastq.gz. In this sample, we know that we used the illumina adaptor GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT, so try to remove this from the 3' end of reads and see the impact of the procedure using FastQC. For this, you need to insert a new adapter in 3', and in the source, select "Enter a custom sequence" (you don't need to add a name, just paste the sequence). 

**Exercise 9**: What happened? To answer, look at the report from cutadapt, and use FastQC on the fastq that is output by cutadapt. 
<details><summary>Click Here to see the answer</summary>
	Almost no read was affected. This is because what you get is a readthrough, so what is actually in the read is the reverse complement of the adaptor. Now, try the same procedure but with AGATCGGAAGAGCACACGTCTGAACTCCAGTCAC (reverse complement of the previous). This time, most reads should have had the adaptor removed. This [tool](https://www.bioinformatics.org/sms/rev_comp.html) is very useful to obtain the reverse complement. 
</details>
<br/>




[Trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic) is a tool that performs both trimming of low quality reads, as well as adaptor removal. Moreover, it already contains a library of commonly used adaptors, so you don't need to know their sequence. Similar to FastQC, it is a java program, so you can use it in any operating system (such as Windows and Mac), although unlike FastQC it needs to be run only using the commandline. 


**Exercise 10**: Find and select the Trimmomatic tool in Galaxy. What different operations can you perform with Trimmomatic that use the base quality information? 
<details><summary>Click Here to see the answer</summary>
You can perform the following operations with Trimmomatic (either isolated, or in combination):

	* ILLUMINACLIP: Cut adapter and other illumina-specific sequences from the read
	
	* SLIDINGWINDOW: Perform a sliding window trimming, cutting once the average quality within the window falls below a threshold
        
	* MINLEN: Drop the read if it is below a specified length

	* LEADING: Cut bases off the start of a read, if below a threshold quality

	* TRAILING: Cut bases off the end of a read, if below a threshold quality

	* CROP: Cut the read to a specified length

	* HEADCROP: Cut the specified number of bases from the start of the read

	* AVGQUAL: Drop the read if the average quality is below a specified value

	* MAXINFO: Trim reads adaptively, balancing read length and error rate to maximise the value of each read
	
</details>
<br/>


**TASK**: Let's use Trimmomatic in Galaxy to remove low quality bases from MiSeq_250bp.fastq.gz, as well as the remainings of illumina Nextera adaptors that are still left in some of the reads. This fastq file is in the commonly used Phred score, so you can change its file type to 'fastqsanger'. Now Trimmomatic should find it. Let's perform the default operation "Sliding Window" of size 4 and average quality 20. Let's also remove the adaptors. For this, select 'Yes' on 'Perform initial ILLUMINACLIP step'. The select "Nextera (paired end)" and leave the rest of the parameters as they were. Finally, you can click on Execute.

**Exercise 11**: What happened? To answer, use FastQC on the fastq output by Trimmomatic. 
<details><summary>Click Here to see the answer</summary>
	The base quality distribution improved. Moreover, the few Nextera primers in the end of the reads also disappeared. Nonetheless, read length is now shortened, and we have fewer reads than before.
</details>
<br/>
<br/>


**Exercise 12**: Let's use Trimmomatic in Galaxy with a paired-end dataset. Upload the files paired_end_example_1.fastq.gz and paired_end_example_2.fastq.gz. Change their types to 'fastqsanger'. In Trimmomatic, select 'Paired-end (two separate input file)'. Perform the same operations as before.

<br/>

**Exercise 13**: Now, you get 4 files as output from Trimmomatic. Can you explain what these are? 
<details><summary>Click Here to see the answer</summary>
You get the following paired files: Trimmomatic on paired_end_example_1.fastq (R1 paired) and Trimmomatic on paired_end_example_2.fastq (R2 paired). These contain the paired reads that "survived" the quality operation from both the forward and the reverse and could therefore be kept as pairs. Then, you have the cases where just one of the pairs was removed because of low quality. In this case, it cannot be kept as pair, but in a separate "isolated" file, both for the forward (Trimmomatic on paired_end_example_1.fastq (R1 unpaired)) and the reverse (Trimmomatic on paired_end_example_2.fastq (R2 unpaired)).
</details>
<br/>

**Exercise 14**: From the "isolated" reads resulting from Trimmomatic, which one has more reads? Why is that?
<details><summary>Click Here to see the answer</summary>
The forward file (Trimmomatic on paired_end_example_1.fastq (R1 unpaired)) has more reads, because the reverse reads have usually less quality, and therefore are more likely to be removed in the filtering process.
</details>
<br/>
<br/>
<br/>


**NOTE**:  Assess how well you achieve the learning outcome. For this, see how well you responded to the different questions during the activities and also make the following questions to yourself.

* Could you manually remove low quality bases from the end of a read in the fastq format? 

* Did you broadly understand the challenges of removing bad quality bases from reads? 

* Could you use cutadapt to remove adaptors from reads in a fastq file? 

* Could you use trimmomatic to remove bad quality bases and remove adaptors from reads in a fastq file? 

* Did you understand the issue of manipulating paired-end fastq files? 
<br/>
<br/>


<br/>
<br/>

> ## Align HTS data against a genome

> ### Use the BWA aligner to align HTS data against a genome

One of the most common applications of NGS is resequencing, where we want to genotype an individual from a species whose genome has already been assembled (a reference genome), such as the human genome, often with the goal to identify mutations that can explain a phenotype of interest.

After obtaining millions of short reads, we need to align them to a (sometimes large) reference genome. To achieve this, novel, more efficient, alignment methods had to be developed. One popular method is based on the [burrows-wheeler transform](https://en.wikipedia.org/wiki/Burrows%E2%80%93Wheeler_transform) and the use of efficient data structures, of which [bwa](http://bio-bwa.sourceforge.net/) and [bowtie](http://bowtie-bio.sourceforge.net/index.shtml) are examples. They enable alignment of millions of reads in a few minutes, even in a laptop.

**NOTE:** Aligners based on the burrows-wheeler transform makes some assumptions to speed up the alignment process. Namely, they require the reference genome to be very similar to your sequenced DNA (less than 2-5% differences). Moreover, they are not optimal, and therefore sometimes make some mistakes.


> ###  The SAM/BAM alignment format

To store millions of alignments, researchers also had to develop new, more practical formats. The [Sequence Alignment/Map (SAM) format](https://samtools.github.io/hts-specs/SAMv1.pdf) is a tabular text file format, where each line contains information for one alignment.
 

**Exercise 15** Upload the reference genome "Escherichia coli str. K-12 substr. MG1655, complete genome" from NCBI repository as fasta file. Consider the Trimmomatic on Forward reads (R1 paired) and Trimmomatic on Reverse reads (R2 paired) that you obtained before. Next, perform an alignment with _bwa mem_ of the paired reads (you need to select the option of paired reads) against the reference genome (choose one from history). Set read groups information: Automatically assign ID.
Leave the rest of the settings on default  Next, download the bam file that was created. Also download the companion bai index file (you need to press on the download icon to have the option to download the bam and the bai files). 
<br/>
<br/>

**NOTE**:  Assess how well you achieve the learning outcome. For this, see how well you responded to the different questions during the activities and also make the following questions to yourself.

* Did you broadly understand the challenges of aligning millions of short reads to a genome? 

* Did you broadly understand the assumptions underlying the use of burrows-wheeler aligners?

* Could you use bwa to align reads to a reference genome? 

* Do you know what is the most common alignment format these aligners use? 

* Do you broadly understand the contents of a SAM/BAM file and the difference between SAM and BAM? 
<br/>
<br/>



> ## Visualize alignments

> ### Use Qualimap to assess quality of alignments

After generating alignments and obtaining a SAM/BAM file, how do I know this step went well? The same way as FastQC generates reports of fastq files to assess quality of raw data, there are programs that generate global reports on the quality of alignments. One popular tool for this is [qualimap](http://qualimap.bioinfo.cipf.es/).

Qualimap is a platform-independent application written in Java and R that provides both a Graphical User Interface (GUI) and a command-line interface to facilitate the quality control of alignment sequencing data.  


A Qualimap report includes, among other things:  

  * Number of aligned/mapped reads and other global statistics
  
  * Coverage across the genome and the histogram of coverages
  
  * Number of duplicated sequences (that align exactly to the same place)
  
  * Histogram of mapping quality (how well the reads align, in a Phred scale)
  
  * Distribution of insert size (length of fragments, only available with paired-end alignments)
  




Many of the plots produced by Qualimap are similar to the ones produced by FastQC. There are nonetheless, figures that are specific to alignments. One important figure to look at is the **alignment rate** (percentage of the total reads that align). In this case, we want it to be as close as possible to 100%. In the case of bacterial sequencing or targeted (eg. exonic) sequencing you expect >95% successful alignment, but if sequencing a full mamallian genome (with many duplicated areas) it may be normal to have as low as 70-80% alignment success. Another alignment-specific figure is the **coverage** along the genome. The coverage on a position is the number of reads whose alignment overlap with that position. Another factor to take into account is the amount of duplicated sequences. Usually, duplication levels higher than 20% are not a good sign (they're a sign of low input DNA and PCR artifacts) but again, depends on what you are sequencing and how much. In any of these factors one has to antecipate the expected "quality" for your application.

**TASK**: Open the reports example_HiSeqBGI.pdf and example_MiSeq.pdf on the [folder](https://github.com/CarinaSilva/Master-in-Health-Molecular-Technologies/tree/master/Exercises/NGS/Qualimap) qualimap from github. Both reports are from alignments to Escherichia coli. 

**Exercise 16**: What is the difference in the sequence coverage between those two files? 
<details><summary>Click Here to see the answer</summary>
	The HiSeq_BGI example displays a homogeneous coverage of ~110x, with a few noticeable drops (the largest one, at least probably due to a deletion, and a small region in the end that displays a coverage of ~170x (probably due to a duplication event). The MiSeq example displays a more heterogeneous coverage between 25-40x coverage, with a noticeable dip towards the end (likely to be due to a deletion).
</details>
<br/>



**Exercise 17**: Given what you saw in the two previous questions, can you think of reasons that may explain the more heterogeneous coverage of the MiSeq example (particularly the heterogeneity observed along the genome)?
<details><summary>Click Here to see the answer</summary>
	The lower coverage may explain a higher local variation, but not the genome-wide positional bias in coverage. Another explanation is the use of enzymatic fragmentation, which is not entirely random, but again this is unlikely to explain the positional variation. A more likely explanation is that bacteria are still in exponencial growth in the case of the MiSeq example, which would explain a greater amount of DNA fragments obtained from the region surrounding the origin of replication.
</details>
<br/>
<br/>

> ### Use IGV to visualize the content of a BAM file

You can also directly visualize the alignments using appropriate software such as [IGV](https://www.broadinstitute.org/igv/) or [Tablet](https://ics.hutton.ac.uk/tablet/). 

**TASK** Install 'igv' on your computer. First, load the reference genome used for the alignment (load genome of Escherichia coli _sequence.fasta_ as file). You should see a chromosome of ~4.5Mb appearing, which is the genome size of Escherichia coli. Next, load the bam file  you downloaded from Galaxy. You should see new tracks appearing in IGV when you load a file. 

**Exercise 18:** Paste in the interval window on the top this position: 'Chromosome:3846244-3846290'. What can you see? 
<details><summary>Click Here to see the answer</summary>
You can see an A to C SNP (Single Nucleotide Polymorphism) at position 3846267.



</details>
<br/>

**Exercise 19:** Paste in the interval window on the top these two positions separated by space: 'Chromosome:1-1000 Chromosome:4640500-4641652'. What can you see? 



<details><summary>Click Here to see the answer</summary>
You can see an A to C SNP (Single Nucleotide Polymorphism) at position 3846267.


</details>
<br/>

**Exercise 20** Paste in the interval window on the top these two positions separated by space: 'Chromosome:1-1000 Chromosome:4640500-4641652'. What can you see? 
<details><summary>Click Here to see the answer</summary>
You should see colors in some reads. These colors mean that the fragment lengths (estimated by the distances between the paired reads) are much significantly different to the mean fragment lengths. These are usually an indication of a structural variant (such as a large deletion). In this case, the estimated fragment length is the size of the genome! This is easy to understand if you realize this is a circular genome from a bacteria, and thus it is natural that a read aligning in the "beginning" of the genome may have its pair aligning in the "end" of the genome.
	

</details>
<br/>

**Exercise 21** Paste in the interval window on the top this position: 'Chromosome:3759212-3768438'. What can you see? 
<details><summary>Click Here to see the answer</summary>
You can see two regions where the reads are marked in white, both with slightly less coverage than the remaining regions marked in gray. The reads marked in white have a mapping quality of Q=0, which means the aligner does not know where these reads actually belong to. Most genomes (particularly mamallian genomes) contain areas of low complexity, composed mostly of repetitive sequences. In the case of short reads, sometimes these align to multiple regions in the genome equally well, making it impossible to know where the fragment came from. Longer reads are needed to overcome these difficulties, or in the absence of these, paired-end data can also be used. Some aligners (such as bwa) can use information on paired reads to help disambiguate some alignments. Information on paired reads is also added to the SAM file when proper aligners are used.




</details>
<br/>
<br/>

**NOTE**: Assess how well you achieve the learning outcome. For this, see how well you responded to the different questions during the activities and also make the following questions to yourself.

*: Did you broadly understand the different aspects to consider when evaluating the quality of your alignments?

* Did you broadly understand the information in a Qualimap report and use it to detect potential issues in your data? 

* Could you use IGV to visualize alignments in the SAM/BAM format? 

