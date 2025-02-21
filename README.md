[![install with bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg?style=flat)](http://bioconda.github.io/recipes/strainscan/README.html)
# StrainScan 
One efficient, accurate and high-resolution strain-level microbiome composition analysis tool based on reference genomes and k-mers. StrainScan takes reference database and sequencing data as input, outputs strain-level microbiome compistion analysis report.


### Contributor: Liao Herui and Ji Yongxin (Ph.D of City University of Hong Kong, EE)
### E-mail: heruiliao2-c@my.cityu.edu.hk / yxjijms@gmail.com
### Version: V1.0.10

#### *__[Update - 2022 - 05 - 01]__* :  <BR/>

* *V1.0.3: StrainScan can be installed via bioconda now! <BR/>*

#### *__[Update - 2022 - 06 - 07]__* :  <BR/>

* *V1.0.10: Add multuple threads to the reference database constrcution! <BR/>*

#### *__[Update - 2023 - 02 - 07]__* :  <BR/>

* *(only GitHub version)Two new intra-cluster searching modes are updated: plasmid_mode and extraRegion_mode.<BR/>*

#### *__[Update - 2023 - 04 - 22]__* :  <BR/>

* *(only GitHub version)StrainScan is able to take gzipped and PE FASTQs as input now!<BR/>*


---------------------------------------------------------------------------
### Overview of StrainScan:
<div align=center><img width="500" height="500" src="https://user-images.githubusercontent.com/22760266/152946273-b39c5c10-9a96-4572-b409-e7a8db53d9e4.png" alt="strainscan_overview_new"></div>

---------------------------------------------------------------------------
### Dependencies:
* Python ==3.7.x
* R
* Sibeliaz ==1.2.2 (https://github.com/medvedevgroup/SibeliaZ)
* Required python package: numpy==1.17.3, pandas==1.0.1, biopython==1.74, scipy==1.3.1, scikit-learn==0.23.1, bidict==0.21.3, treelib==1.6.1

Make sure these programs have been installed before using StrainScan. 

## Install (Linux or ubuntu only)
Option 1 - The first way to install StrainScan, is to use [bioconda](https://bioconda.github.io/).
Once you have bioconda environment installed, install package strainscan:

	conda install -c bioconda strainscan
 
 It should be noted that some commands have been replaced if you install StrainScan using bioconda. (See below)

Command (Not bioconda)    |	Command (bioconda)
------------ | ------------- 
python StrainScan.py -h | strainscan -h
python StrainScan_build.py -h | strainscan_build -h

Option 2 - Also, yon can install StrainScan via [Anaconda](https://anaconda.org/) using the commands below:<BR/>
####
`git clone https://github.com/liaoherui/StrainScan.git`<BR/>
`cd StrainScan`<BR/>

`conda env create -f environment.yaml`<BR/>
`conda activate strainscan`<BR/>

`chmod 755 library/jellyfish-linux`<BR/>
`chmod 755 library/dashing_s128`<BR/>

Note: if the command `conda env create -f environment.yaml` outputs an error (which is most likely caused by your machine): `ResolvePackageNotFound...`, then you can try the command `conda env create -f environment_candidate.yaml`

####

Option 3 - Or, you can install all dependencies of StrainScan mannually and then run the commands below.

`git clone https://github.com/liaoherui/StrainScan.git`<BR/>
`cd StrainScan`<BR/>

`chmod 755 library/jellyfish-linux`<BR/>
`chmod 755 library/dashing_s128`<BR/>

## Pre-built databases download
The table below offers information about the pre-built databases of 6 bacterial species used in the paper. Users can download these databases and use them to identify strains directly.

Species   |	Source  | Number of Strains |	Number of Clusters |	Download link
------------ | -------------| ------------- | ------------- | ------------- 
Akkermansia muciniphila |  NCBI | 157 | 53  | [Google drive](https://drive.google.com/file/d/1BAoi5u4JuPTapULjbZRgaBBW5JrNYd7P/view?usp=sharing)
Cutibacterium acnes |  NCBI | 275 | 28  | [Google drive](https://drive.google.com/file/d/15YbWWMsao8Rzqw_6PiARSwA3ahvNjW_w/view?usp=sharing)
Prevotella copri |  NCBI | 112 | 51  | [Google drive](https://drive.google.com/file/d/1qhc17ZSRop0hp5lrM2sAQiffv0PXuQ6r/view?usp=sharing)
Escherichia coli |  NCBI | 1433 | 823  | [Google drive](https://drive.google.com/file/d/1otmTt98xu8YUiTh4kQDKo3e6wMaOpWSV/view?usp=sharing)
Mycobacterium tuberculosis |  NCBI | 792 | 25  | [Google drive](https://drive.google.com/file/d/18pPGyHODRwYV_d-l-xOKiLkbj7WcHCR_/view?usp=sharing)
Staphylococcus epidermidis |  NCBI | 995 | 378  | [Google drive](https://drive.google.com/file/d/1wwGubVO9F4r0pwHqQWQ2_NC151WDn96I/view?usp=sharing)

You can also use bash scripts in the folder "Download_DB_script" to download the pre-built databases from Google drive. For example,
 
 `cd Download_DB_script`<BR/>
 `sh ecoli_db.sh`<BR/>





## Usage
One example about database construction and identification commands can be found in "<b>test_run.sh</b>".

### Use StrainScan to build your own custom database.<BR/>

  `python StrainScan_build.py -i <Input_Genomes> -o <Database_Dir>`<BR/>
<BR/>eg:
  `python StrainScan_build.py -i Test_genomes -o DB_Small`<BR/>

(Note: input fasta can be gzipped format)

### Use StrainScan to identify bacterial strains in short reads.
  `python StrainScan.py -i <Input_reads> -d <Database_Dir> -o <Output_Dir>`<BR/>
<BR/>eg:
  `python StrainScan.py -i Sim_Data/GCF_003812785.fq -d DB_Small -o Test_Sim/GCF_003812785`<BR/>
 or
  `python StrainScan.py -i Sim_Data_mul/GCA_000144385_5X_GCF_008868325_5X.fq -d  DB_Small -o Test_Sim/GCA_000144385_5X_GCF_008868325_5X `<BR/>
  
  PE reads (can be gzipped FASTQ format)<BR/>
   `python StrainScan.py -i GCF_003812785_1.fq.gz -j GCF_003812785_2.fq.gz -d DB_Small -o Test_Sim/GCF_003812785`<BR/>

### Use StrainScan to identify plasmids of bacterial strains in short reads.
  option-1: identify possible plasmids by using contigs <100000 bp:<BR/>
  `python StrainScan.py -i <Input_reads> -d <Database_Dir> -p 1 -r <Ref_genome_Dir> -o <Output_Dir>`<BR/>
  
  option-2: identify possible plasmids (or possible strains) using reference genomes provided by "-r".<BR/>
  `python StrainScan.py -i <Input_reads> -d <Database_Dir> -p 2 -r <Ref_genome_Dir> -o <Output_Dir>`<BR/>
  
 `<Ref_genome_Dir>` refer to the dir of reference genomes of identified clusters or all strains used to build the database.

### Use StrainScan to identify bacterial strains in short reads under extraRegion_mode.
This mode will search possible strains and return strains with extra regions (could be different genes, SNVs or SVs to the possible strains) covered. If there is a novel strain not in the database, then its closest relative can be one specific strain while its partial regions (we call them "extraRegion" ) in the genome can be similar to other strains. In this case, this mode can search its closest relative and return strains with "extraRegion" covered for downstream analysis. <BR/>

   `python StrainScan.py -i <Input_reads> -d <Database_Dir> -e 1 -o <Output_Dir>`<BR/>

### Full command-line options
<!---(Note: The initial idea of development of StrainScan is "Simpler is better". We do not want to burden users due to complicated usage of StrainScan. So the default parameters (some are inside the program) are simple but have good performance in our test, however, more useful parameters will be added for users who need them.)-->

Identification - StrainScan.py (Default k-mer size: 31)
```
StrainScan - A kmer-based strain-level identification tool.

Example: python StrainScan.py -i  <Input_reads> -d <Database_Dir> -o <Output_Dir>

required arguments:
    -i, --input_fastq             Input fastq data.
    -j, --input_fastq_2		  Input fastq data (for pair-end data).
    -d, --database_dir            Path of StrainScan database.

optional arguments:
    -h, --help                    Show help message and exit.
    -o, --output_dir              The output directory. (Default: ./StrainScan_Result)
    -k, --kmer_size               The size of k-mer, should be odd number. (Default: k=31)
    -l, --low_dep                 This parameter can be set to "1" if the sequencing depth of input data is very low (e.g. < 5x). For super low depth ( < 1x ), you can use "-l 2" (default: -l 0)
    -p,	--plasmid_mode		  If this parameter is set to 1, the intra-cluster searching process will search possible plasmids using short contigs (<100000 bp) in strain genomes, which are likely to be plasmids. 
                                  If this parameter is set to 2, the intra-cluster searching process will search possible plasmids or strains using given reference genomes by "-r".
    				  Reference genome sequences (-r) are required if this mode is used. (default: -p 0)
    -r, --ref_genome		  The dir of reference genomes of identified cluster or all strains. If plasmid_mode is used, then this parameter is required.
    -e, --extraRegion_mode	  If this parameter is set to 1, the intra-cluster searching process will search possible strains and return strains with extra regions (could be different genes, SNVs or SVs to the possible strains) covered. (default: -e 0)
    -s, --minimum_snv_num         The minimum number of SNVs during the iterative matrix multiplication at Layer-2 identification. (Default: s=40)
```
Build database - StrainScan_build.py (Default k-mer size: 31)
```
StrainScan - A kmer-based strain-level identification tool.

Example:  python StrainScan_build.py -i <Input_genomes> -o <Database_Dir>

required arguments:
     -i, --input_fasta             The path of input genomes. ("fasta" format)
     
optional arguments:
     -o, --output_dir              The output directory of constructed database. (Default: ./StrainScan_DB)
     -k, --kmer_size               The size of k-mer, should be odd number. (Default: k=31)
     -t, --threads		   The threads used to build the database. (default: t=1)
     -u, --uk_num                  The maximum number of unique k-mers in each genome to extract. (Default: 100000)
     -g, --gk_ratio                The ratio of group-specific k-mers to extract. (Default: g=1.0)        
     -m, --strainest_sample        If this parameter is 1, then the program will search joint kmer sets from msa generated by Strainest. To use this parameter, you have to make sure Strainest can run normally. (Default: 0)
     -n, --mink_cutoff             Minimum k-mer number cutoff in a node of the cluster search tree (CST). (Default: n=1000)
     -x, --maxk_cutoff             Maximum k-mer number cutoff in a node of the cluster search tree (CST). (Default: x=30000)
     -r, --maxn_cutoff             Maximum cluster number for node reconstruction of the cluster search tree(CST). (Default: r=3000)
```

## Output Format
The output of StrainScan contains two parts. The first part is the final identification report file in text format. This file contains all identified strains and their predicted depth and relative abundance, etc. The second part is the strain identification report files inside each cluster.

For your reference, two output files are given as example in the folder "Output_Example" in this repository. These files contain identification results of one single-strain and one two-strain (depth: 5X and 5X) simulated datasets, respectively.

Explaination about the columns in the final identification report file (E.g. "Output_Example/GCA_000144385_5X_GCF_008868325_5X/final_report.txt") of StrainScan.
Column_name   |	Description
------------ | ------------- 
Strain_ID | The numerical id of identified strains in the ascending order.
Strain_Name | The name of identified strains. (In the example output, the name refers to the NCBI RefSeq accession id)
Cluster_ID  | The cluster id of identified strains. (For cluster information, users can check "<Database_Dir>/Cluster_Result/hclsMap_95_recls.txt")
Relative_Abundance_Inside_Cluster | The predicted relative abundance of identified strains inside the cluster.
Predicted_Depth (Enet) | The predicted sequencing depth of identified strains inside the cluster using elastic net model.
Predicted_Depth (Ab\*cls_depth) | The final predicted sequencing depth of identified strains.
Coverage  | The estimated k-mer-based coverage of identified strains.
Coverd/Total_kmr  | The number of "covered" and "total" k-mers of identified strains.
Valid_kmr | The valid k-mer refers to the k-mer belonging to the identified strain during the iterative matrix multiplication. More valid k-mers there are, more likely this strain exist.
Remain_Coverage | The coverage calculated by "covered" / "total" k-mers during the iterative matrix multiplication.
CV  | The number of "covered" and "valid" k-mers of identified strains.
Exist evidence  | By default, identified strains with "relative abundance > 0.02 and coverage >0.7" will be marked as "\*". Strains with "\*" are more likely to exist. However, for low-depth samples, this parameter may be not useful.

## References:

how to cite this tool:
```
Liao, H., Ji, Y., & Sun, Y. Accurate strain-level microbiome composition analysis from short reads. bioRxiv. 2022. https://doi.org/10.1101/2022.01.26.477962
```
