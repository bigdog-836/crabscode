# crabscode  
This is my code for getting crabs to work (https://github.com/gjeunen/reference_database_creator)  
1) module load Miniconda3/4.12.0 (to load miniconda) 
(we don't need to install conda because it is already on NeSI, so this is how we get access to it)  

2) conda create --prefix=/nesi/nobackup/uoo00105/ryan/crabs/crabs_env  
(this is instead of GJ's instruction: conda create -n CRABS, the reason we are doing it using the "prefix" option, is that installs all the packages locally rather than in our home directory. The home directory can get jammed up with too many packages and stop working correctly, after that, to activate the environment so we can install things into it we do:)

3) conda activate /nesi/nobackup/uoo00105/ryan/crabs/crabs_env
(this is instead of GJ's command: conda activate CRABS, and this is because we've changed where all the packages will go, so we also have to change where the computer will look to access all of those packages)

4) conda config --add channels bioconda  
5) conda config --add channels conda-forge  
6) conda install -c bioconda crabs  
7) crabs -h (check installation)  

Now that I have installed it I will only need to run:  

9) module load Miniconda3/4.12.0 (to load miniconda)  

10) conda activate /nesi/nobackup/uoo00105/ryan/crabs/crabs_env  

11) crabs db_download --source mitofish --output mitofish.fasta --keep_original yes (MitoFish)

12) crabs insilico_pcr --input complete_partial_mitogenomes.fa --output complete_partial_mitogenomes1.fa --fwd GTCGGTAAAACTCGTGCCAGC --rev CATAGTGGGGTATCTAATCCCAGTTTG --error 4.5

13) crabs db_download --source taxonomy (o assign a taxonomic lineage to each sequence in the reference database (see section 5. assign_tax), the taxonomic information needs to be downloaded. CRABS utilizes NCBI's taxonomy and downloads three specific files to your computer: (i) a file linking accession numbers to taxonomic IDs, (ii) a file containing information about the phylogenetic name associated with each taxonomic ID, and (iii) a file containing information how taxonomic IDs are linked.)

Assign taxa:

14) crabs assign_tax --input complete_partial_mitogenomes1.fa --output output.tsv --acc2tax nucl_gb.accession2taxid --taxid nodes.dmp --name names.dmp

15) crabs db_download --source ncbi --database nucleotide --query '12S[All Fields] AND txid7898[Organism:exp] AND mitochondrion[filter]' --output 12S_fish.fasta --keep_original yes --email ryan.r.easton@gmail.com --batchsize 5000
