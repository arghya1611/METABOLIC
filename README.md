<img src="https://github.com/AnantharamanLab/METABOLIC/blob/master/METABOLIC.jpg" width="85%">


# METABOLIC
**MET**abolic **A**nd **B**ioge**O**chemistry ana**L**yses **I**n mi**C**robes  
Version 2.0

This software enables the prediction of metabolic and biogeochemical functional trait profiles to any given genome datasets. These genome datasets can either be metagenome-assembled genomes (MAGs), single-cell amplified genomes (SAGs) or pure culture sequenced genomes. It can also calculate the genome coverage. The information is parsed and diagrams for elemental/biogeochemical cycling pathways (currently Nitrogen, Carbon, Sulfur and "other") are produced.  

## Copyright:  
Zhichao Zhou, zczhou2017@gmail.com  
Patricia Tran, ptran5@wisc.edu  
Karthik Anantharaman, karthik@bact.wisc.edu  
Anantharaman Microbiome Laboratory  
Department of Bacteriology, University of Wisconsin, Madison 


If you are using this program, please consider citing our preprint, available on [BioRxiv](https://www.biorxiv.org/content/10.1101/761643v1)
```
    Zhou Z, Tran P, Liu Y, Kieft K, Anantharaman K. "METABOLIC: A scalable high-throughput metabolic and biogeochemical functional trait profiler based on microbial genomes" (2019). BioRxiv doi: https://doi.org/10.1101/761643
```

## Installation instructions

1. Go to where you want the program to be and clone the github repository or click the green buttom "download ZIP" folder, and unzip. The perl and R scripts and dependent databases should be kept in the same directory.

```
    git clone https://github.com/AnantharamanLab/METABOLIC.git
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
         use Array::Split qw(split_by split_into);
         use Bio::SeqIO;
         use Bio::Perl;
         use Bio::Tools::CodonTable;
         use Carp;
     ```    
    2.2. [HMMER](http://hmmer.org/) (>= v3.1b2)   
    2.3. [Prodigal](https://github.com/hyattpd/Prodigal) (>= v2.6.3)        
    &emsp;Remarks: executable must be named prodigal and not prodigal.linux  
    2.4. [Sambamba](https://github.com/biod/sambamba) (>= v0.7.0)  
    2.5. [BAMtools](https://github.com/pezmaster31/bamtools/wiki) (>= v2.4.0)   
    2.6. [CoverM](https://github.com/wwood/CoverM)  
    2.7. [R](https://www.r-project.org/) (>= 3.6.0)  
        &emsp;Installing required R packages using Rscript:  
        &emsp;Copy and paste the following command into your terminal window (may require super user permissions to run):  
  
```
    Rscript -e 'install.packages("diagram", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("forcats", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("digest", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("htmltools", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("rmarkdown", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("reprex", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("tidyverse", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("stringi", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("ggthemes", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("ggalluvial", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("reshape2", repos = "http://cran.us.r-project.org")'    
    Rscript -e 'install.packages("ggraph", repos = "http://cran.us.r-project.org")'    
```    


   &emsp;&emsp;You can follow the install instructions of each program, or you could also    
   &emsp;&emsp;install them via Conda and add them to your system path:  
   &emsp;&emsp;Link: <https://anaconda.org>    
   
*NOTE: For steps 3-9, we provide a "run_to_setup.sh" script for easily setting up dependent databases.*
  
3. METABOLIC requires the KofamKOALA hmm and METABOLIC hmm databases   
  [KofamKOALA website](https://www.genome.jp/tools/kofamkoala/)
  
   3.1. Download KofamKOALA hmm database files: 

```
    mkdir kofam_database  
    cd kofam_database  
    wget -c ftp://ftp.genome.jp/pub/db/kofam/ko_list.gz  
    wget -c ftp://ftp.genome.jp/pub/db/kofam/profiles.tar.gz  
    gzip -d ko_list.gz  
    tar xzf profiles.tar.gz; rm profiles.tar.gz  
    mv All_Module_KO_ids.txt profiles  
    cd profiles  
    cp ../../Accessory_scripts/batch_hmmpress.pl ./  
    perl batch_hmmpress.pl  
```

   &emsp; &emsp;3.2. The METABOLIC hmm database in "METABOLIC_hmm_db.tgz" contains custom hmm files, self-parsed Pfam and TIRGfam files. It needs to be decompressed to the folder "METABOLIC_hmm_db" and stays in the same directory of KofamKOALA hmm database and scripts.  

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
6. This software also contains "Motif.tgz", which needs to be decompressed before use. 
```
  tar zxvf Motif.tgz
```
7. You will download the most recent dbCAN-fam-HMMs.txt into a directory (that is made by you) “dbCAN2”. And parse the dbCAN-HMMdb.txt by "batch_hmmpress_for_dbCAN2_HMMdb.pl".    
```
  mkdir dbCAN2
  cd dbCAN2
  wget http://bcb.unl.edu/dbCAN2/download/Databases/dbCAN-old@UGA/dbCAN-fam-HMMs.txt
  perl ../Accessory_scripts/batch_hmmpress_for_dbCAN2_HMMdb.pl
  cd ../
```
8. You will download the MEROPS Peptidase Protein Sequences (https://www.ebi.ac.uk/merops/download_list.shtml, No. 3 option). And parse the pepunit.lib by DIAMOND to make the BLASTP database.      
```
  mkdir MEROPS
  cd MEROPS
  wget ftp://ftp.ebi.ac.uk/pub/databases/merops/current_release/pepunit.lib
  perl ../Accessory_scripts/make_pepunit_db.pl
  cd ../
```
9. Finally, this software also contains "5_genomes_test.tgz", which needs to be decompressed before use. This is a set of 5 genomes that you can use to test run the program to see if it works correctly before running your real samples. (see end of page)
```
  tar zxvf 5_genomes_test.tgz
```



## Software work flow 

<img src="https://github.com/AnantharamanLab/METABOLIC/blob/master/Software_work_flow.jpg" width="75%">

## Preparing your files:

## Input Files

1. The genome files should end with ".fasta"; The protein files should end with ".faa". These files should be in one single folder, which you will use as an argument for the option "-in-gn" in the Perl script "perl METABOLIC_v1.3.pl" (See example at end of page)

2. The "-o" or "-r" option requires inputting a text file to show the path of where the metagenomic reads are located. The metagenomics reads refer to the metagenomic read datasets that you used to generate the MAGs. A sample for this txt is as follows:     
```
#Reads pair name with complete pathway: 
/slowdata/Reads/METABOLIC_test_reads/SRR3577362_sub_1.fastq,/slowdata/Reads/METABOLIC_test_reads/SRR3577362_sub_2.fastq
/slowdata/Reads/METABOLIC_test_reads/SRR3577362_sub2_1.fastq,/slowdata/Reads/METABOLIC_test_reads/SRR3577362_sub2_2.fastq
```
&emsp;Please notice that the paired reads are in the same line.


## Result files  

After running the whole program (perl script) you will obtain the following files:

- **METABOLIC result table**

This spreadsheet has 6 tabs:

    - "HMMHitNum" = Number of HMM hits. if you scroll to the right with the coloured cells you'll find the presence/absence, the number of hits, and on which scaffold it was on. 
    - "FunctionHit" = hits to custom HMM curated database. 
    - "KEGGModuleHit" = KEGG module hits with modules and modules category names. 
    - "KEGGModuleStepHit" = similar to the previous one but broken down into smaller categories (steps).    
	- "dbCAN2Hit" = the dbCAN2 searching result - the CAZys numbers and hits.    
	- "MEROPSHit" = the MEROPS peptidase searching result - the MEROPS peptidase numbers and hits.    

In all cases if you scroll down you will see what "Gn00X" colnames refer to (they are based on your fasta file names for the genomes you gave. 


- **Each hmm hit protein collection** 

- **KEGG_identifier_result**    
the KEGG identifier searching result - KEGG identifier numbers and hits of each genome that could be used to visualize the pathways in KEGG Mapper

- **Elemental/Biogeochemical cycling pathways for each genome and a summary scheme (Both files and figures)**

In the designated R output folder named "R_ouput", you will have the following files for EACH MAG: 
```
  GenomeName.draw_sulfur_cycle_single.PDF
  GenomeName.draw_nitrogen_cycle_single.PDF
  GenomeName.draw_other_cycle_single.PDF
  GenomeName.draw_carbon_cycle_single.PDF
```
A red arrow means present, and a black arrow means absent.

If you input a "metagenomic reads" txt file, the software will help to calculate the gene abundance, then you will have have the following output as the summary diagram of pathways at a community scale:
```
  draw_sulfur_cycle_total.PDF
  draw_other_cycle_total.PDF
  draw_nitrogen_cycle_total.PDF
  draw_carbon_cycle_total.PDF
```

You could also run the Rscript separately. Once you have the "R_input" folders, use it as input to visualize your results in the R script
```
Rscript draw_biogeochemical_cycles.R R_input R_Output TRUE

```
The draw_biogeochemical_cycles.R command can be given as a relative paths or full path. 

The first argument is the name of the folder with your inputs (in this case "R_input_files"). This is where your files that end with "...R_input.txt" and your "Total.R_input.txt" files are. **Note!: There is no forward slash after the folder name**. You can use full paths or relative paths to the input folder.

The second argument is the the name of the output folder where your images will be saved. The folder does not have to exist already (i.e. no need to mkdir first). **Note!: Once again there is no forward slash after the folder name**

The last argument takes the value "TRUE" or "FALSE". If it is "TRUE" it means that you have inputted mapped reads in the beginning, and you will have a "Total.R_input.txt" file that can be parsed to make the biogeochemical cycles summary figures. This is important because the summary figure has coverage information (the 3rd column of that file).    

- **Community level diagrams: Sequential transformation, Metabolic energy flow and Metabolic network**

The "Sequential transformation" diagram, "Metabolic energy flow" diagram and "Metabolic network" diagrams will be generated accordingly, including both the input txt files and the resulted Rscript-generated diagrams    
```
For Sequential transformation diagram, We have summarized and visualized the genome number and genome coverage (relative abundance of microorganism) of the microorganisms that were putatively involved in the sequential transformation of both important inorganic elements and organic compounds.    
The resulted file is "Sequential_transformation_01.pdf" and "Sequential_transformation_02.pdf".        

For Metabolic energy flow diagram, a Sankey diagram will be generated, representing the function fractions that are contributed by various microbial groups in a given community.    
The resulted file is "Metabolic_network.pdf".

For Metabolic network diagrams, diagrams representing metabolic connections of biogeochemical cycling steps at both phylum level and the whole community level will be generated.    
The resulted files are placed in the folder of "Metabolic_network".
```


## Instructions on running test files

The test files are given in the folder "5_genomes_test.tgz", which includes the input five genome files and the running results.     
One could use this to test whether you have successfully installed all the prerequisites in a proper way.   

## Instructions on running real data
Follow similar instructions for your real files.     
 
<img src="https://github.com/AnantharamanLab/METABOLIC/blob/master/METABOLIC-G.jpg" width="30%">  

METABOLIC-G.pl is specifically for users who do not have metagenomic reads and only want to get metabolic profiles and biogeochemical cycling diagram of input genomes.
```

perl your/path/to/put/METABOLIC-folder/METABOLIC-G.pl -in-gn [folder with all your genomes] -t [number of threads] -o [METABOLIC output folder] -m your/path/to/put/METABOLIC-folder

```
<img src="https://github.com/AnantharamanLab/METABOLIC/blob/master/METABOLIC-C.jpg" width="30%">  

METABOLIC-C.pl is specifically for users who have metagenomic reads and want to include them in the community analysis. 
```

perl your/path/to/put/METABOLIC-folder/METABOLIC-C.pl -in-gn [folder with all your genomes] -t [number of threads] -o [METABOLIC output folder] -m your/path/to/put/METABOLIC-folder -r [omic reads parameter file]

```
## Version updates
v1.1 -- Sep 4, 2019 --     
Fix the parallel problem, change from hmmscan to hmmsearch, and update the "METABOLIC_temp_and_db"    
v1.2 -- Sep 5, 2019 --    
Fix the prodigal parallel run, change "working-dir" to "METABOLIC-dir"    
v1.3 -- Sep 5, 2019 --     
Fix the output folder problem, the perl script could be called in another place instead of the original place  
v2.0 -- Nov 5, 2019 --     
Add more functions on visualization, add more annotations, make the software faster    
v3.0 -- Feb 18, 2020 --     
Reduce the size of Kofam Hmm profiles (only use KOs that can be found in Modules) to speed up the calculation    
Change HMMER to v3.3 to speed up the calculation


