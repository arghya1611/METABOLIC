<img src="https://github.com/ChaoLab/METABOLIC/blob/master/METABOLIC.jpg" width="85%">


# METABOLIC
**MET**abolic **A**nd **B**ioge**O**chemistry ana**L**yses **I**n mi**C**robes  
Version 1.0

This software finds a metabolic and biogeochemical function trait profile to given genome datasets. These genome datasets can either be metagenome-assembled genomes (MAGs), single-cell amplified genomes (SAGs) or pure culture sequenced genomes. It also calculates the genome coverage. The information is parsed and diagrams for element cycling pathways (currently Nitrogen, Carbon, Sulfur and "other") are produced.  

## Copyright:  
Zhichao Zhou, zczhou2017@gmail.com  
Patricia Tran, ptran5@wisc.edu  
Karthik Anantharaman, karthik@bact.wisc.edu  
Anantharaman Microbiome Laboratory  
Department of Bacteriology, University of Wisconsin, Madison  


## Installation instruction
_scroll down the page for "Quick Start Instructions"_

1. Go to where you want the program to be and clone the github repository or click the green buttom "download ZIP" folder, and unzip.

```
    git-clone https://github.com/ChaoLab/METABOLIC.git
```

2. METABOLIC requires the following programs to be added to your system path:  

    2.1. Perl (>= v5.010)  
      ```
      Perl modules:
      (to install, use: "perl -MCPAN -e shell cpan" or "cpan -i Data::Dumper"; to check if present, use "perldoc -l Data::Dumper")    
         use Data::Dumper;    
         use POSIX qw(strftime);    
         use Excel::Writer::XLSX;    
         use Getopt::Long;    
         use Statistics::Descriptive;  
     ```    
    2.2. [HMMER](http://hmmer.org/) (>= v3.1b2)   
    2.3. [Prodigal](https://github.com/hyattpd/Prodigal) (>= v2.6.3)   
      Remarks: executable must be named prodigal and not prodigal.linux  
    2.4. [SAMtools](http://www.htslib.org/) (>= v0.1.19)  
    2.5. [BAMtools](https://github.com/pezmaster31/bamtools/wiki) (>= v2.4.0)   
    2.6. [CoverM](https://github.com/wwood/CoverM)  
    2.7. [Parallel](https://www.gnu.org/software/parallel/)  
    2.8. [R](https://www.r-project.org/) (>= 3.6.0)  
        Installing required R packages using Rscript:  
        Copy and paste the following command into your terminal window (may require super user permissions to run):  
  
```
    Rscript -e 'install.packages("diagram", repos = "http://cran.us.r-project.org")'
```
  
You can follow the install instruction of each program, or you could also  
install them via Conda and add them to your system path:  
Link: <https://anaconda.org>
  	  
  
3. METABOLIC requires the KofamKOALA hmm and METABOLIC hmm database   
  [KofamKOALA website](https://www.genome.jp/tools/kofamkoala/)
  
   3.1. Download KofamKOALA hmm database files: 

```
    mkdir kofam_database  
    cd kofam_database  
    wget -c ftp://ftp.genome.jp/pub/db/kofam/ko_list.gz  
    wget -c ftp://ftp.genome.jp/pub/db/kofam/profiles.tar.gz  
    gzip -d ko_list.gz  
    tar xzf profiles.tar.gz; rm profiles.tar.gz  
    cd profiles  
    cp ../../Accessory_scripts/batch_hmmpress.pl ./  
    perl batch_hmmpress.pl  
```

   3.2. The METABOLIC hmm database in "METABOLIC_hmm_db.tgz" contains custom hmm files, self-parsed Pfam and TIRGfam files. It needs to be decompressed to the folder "METABOLIC_hmm_db" and stays in the same directory of KofamKOALA hmm database and scripts.  

```
  tar zxvf METABOLIC_hmm_db.tgz
```
  
4. METABOLIC uses the "METABOLIC_temp_and_db" which contains the hmm result table and KEGG database information.  
Decompress the METABOLIC_temp_and_db.tgz to the folder "METABOLIC_temp_and_db" and keep it in the same directory of KofamKOALA hmm database and scripts.  
```
  tar zxvf METABOLIC_temp_and_db.tgz
```

5. This software also contains "Accessory_scripts.gz", which needs to be decompressed before use. 
```
  tar zxvf Accessory_scripts.tgz
```

## Input Files

1. The genome files should be ended with ".fasta"; The genome amino acid or protein files should be ended with ".faa" 

2. The "-o" option requires inputting a text file to show the path of where the metagenomic reads are located. The metagenomics reads refer to which are/is the metagenomic datasets that you used to generate the MAGs. A sample for this txt is like:   
```
#Reads pair name with absolute pathway: 
/slowdata/Reads/METABOLIC_test_reads/SRR3577362_sub_1.fastq,/slowdata/Reads/METABOLIC_test_reads/SRR3577362_sub_2.fastq
/slowdata/Reads/METABOLIC_test_reads/SRR3577362_sub2_1.fastq,/slowdata/Reads/METABOLIC_test_reads/SRR3577362_sub2_2.fastq
```
The paired reads are in the same line. An example file is located **HERE**.


## Result files 

After running the whole program you will obtain the following files:

- **METABOLIC result table**



- **Each hmm hit protein collection** 



- **Element cycling pathways for each genome and a summary scheme (Both files and figures)**

In the designated R output folder named "R_ouput", you will have the following files for EACH MAG:
```
  GenomeName.draw_sulfur_cycle_single.PDF
  GenomeName.draw_nitrogen_cycle_single.PDF
  GenomeName.draw_other_cycle_single.PDF
  GenomeName.draw_carbon_cycle_single.PDF
```
If you input a "metagenomic reads" txt file, the software will help to calculate the gene abundance, then you will have have the following output as the summary diagram of pathways at a community scale:
```
  draw_sulfur_cycle_total.PDF
  draw_other_cycle_total.PDF
  draw_nitrogen_cycle_total.PDF
  draw_carbon_cycle_total.PDF
```

## Software work flow 

<img src="https://github.com/ChaoLab/METABOLIC/blob/master/Software_work_flow.jpg" width="75%">

## Quick-Start
If you are at the Quick-Start, you have already installed the program and its dependencies. 
To run the analysis process as follow
```
 perl METABOLIC_v1.0.pl
```

Once you have the "R_input_files" folders, use it as input to visualize your results in the R script
```
RScript draw_biogeochemical_cycles.R R_input R_Output TRUE

```
The first argument is the name of the folder with your inputs (in this case "R_input_files"). This is where your files that end wiith "...R_input.txt" and your "Total.R_inpuut.txt" files is. **Note!: There is no forward slash after the folder name**

The second argument is the the name of the output folder where your images will be saved. The folder does not have to exist already (e.i. no need to mkdir first). **Note!: Once again there is no forward slash after the folder name**

The last argument takes the value "TRUE" or "FALSE". If it is "TRUE" it means that links to mapped reads where used in the beginning, and you have a "Total.R_input.txt" file that can be parsed to make the biogeochemical cycles summary figures. This is important because the summary figure has coverage information (the 3rd column of that file).



