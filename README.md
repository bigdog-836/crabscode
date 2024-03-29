# crabscode  
This is my code for getting crabs to work (https://github.com/gjeunen/reference_database_creator)  
First go to https://jupyter.nesi.org.nz/
1) module load Miniconda3/22.11.1-1 (to load miniconda) 
(we don't need to install conda because it is already on NeSI, so this is how we get access to it)  

2) conda create --prefix=/nesi/nobackup/uoo03004/ryan/crabs/crabs_env  
(this is instead of GJ's instruction: conda create -n CRABS, the reason we are doing it using the "prefix" option, is that installs all the packages locally rather than in our home directory. The home directory can get jammed up with too many packages and stop working correctly, after that, to activate the environment so we can install things into it we do:)

3) conda activate /nesi/nobackup/uoo03004/ryan/crabs/crabs_env
(this is instead of GJ's command: conda activate CRABS, and this is because we've changed where all the packages will go, so we also have to change where the computer will look to access all of those packages)

4) conda config --add channels bioconda  
5) conda config --add channels conda-forge  
6) conda install -c bioconda crabs  
7) crabs -h (check installation)  

Now that I have installed it I will only need to run:  

9) module load Miniconda3/4.12.0 (to load miniconda)  

10) conda activate /nesi/nobackup/uoo03004/ryan/crabs/crabs_env  

MitoFish
11) crabs db_download --source mitofish --output mitofish.fasta --keep_original yes

12) crabs insilico_pcr --input mitofish.fasta --output mitofish_PCR.fasta --fwd GTCGGTAAAACTCGTGCCAGC --rev CATAGTGGGGTATCTAATCCCAGTTTG --error 4.5

13) crabs db_download --source taxonomy (o assign a taxonomic lineage to each sequence in the reference database (see section 5. assign_tax), the taxonomic information needs to be downloaded. CRABS utilizes NCBI's taxonomy and downloads three specific files to your computer: (i) a file linking accession numbers to taxonomic IDs, (ii) a file containing information about the phylogenetic name associated with each taxonomic ID, and (iii) a file containing information how taxonomic IDs are linked.)

Assign taxa MitoFish:

14) crabs assign_tax --input complete_partial_mitogenomes1.fa --output output.tsv --acc2tax nucl_gb.accession2taxid --taxid nodes.dmp --name names.dmp

NCBI
15) crabs db_download --source ncbi --database nucleotide --query '12S[All Fields] AND txid7898[Organism:exp] AND mitochondrion[filter]' --output 12S_fish.fasta --keep_original yes --email ryan.r.easton@gmail.com --batchsize 5000

16a) crabs insilico_pcr --input 12S_fish.fasta --output 12S_fish_PCR.fasta --fwd GTCGGTAAAACTCGTGCCAGC --rev CATAGTGGGGTATCTAATCCCAGTTTG --error 4.5

16b) crabs pga --input 12S_fish.fasta --database 12S_fish_PCR.fasta --output 12S_fish_PGA.fasta --fwd GTCGGTAAAACTCGTGCCAGC --rev CATAGTGGGGTATCTAATCCCAGTTTG --percid 0.95 --coverage 0.95 --filter_method relaxed

Assign taxa NCBI:
17a) wget ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/accession2taxid/nucl_gb.accession2taxid.gz -q --show-progress
17b) conda install wget=1.21.3
17c) crabs assign_tax --input 12S_fish_PGA.fasta --output output_12S_PGA.tsv --acc2tax nucl_gb.accession2taxid --taxid nodes.dmp --name names.dmp 
17d) gunzip nucl_gb.accession2taxid.gz 

EMBL:

18) crabs db_download --source embl --database 'VRT*' --output embl_vrt.fasta --keep_original yes

19) crabs insilico_pcr --input embl_vrt.fasta --output embl12S_vrt_PCR.fasta --fwd GTCGGTAAAACTCGTGCCAGC --rev CATAGTGGGGTATCTAATCCCAGTTTG --error 4.5

20) crabs db_merge --output Full_output12S_Total_PCR.fasta --uniq yes --input output12S_Total_PCR.fasta embl12S_vrt_PCR.fasta  

db_Merge (to merge output files)

21) crabs db_merge --output output12S_Total_PCR.fasta --uniq yes --input mitofish_PCR.fasta 12S_fish_PCR.fasta 

Merge assign taxonomy

21) crabs assign_tax --input Full_output12S_Total_PCR.fasta --output Full_output12S_Total_PCR.tsv --acc2tax nucl_gb.accession2taxid --taxid nodes.dmp --name names.dmp 

Dereplicate

23) crabs dereplicate --input Full_output12S_Total_PCR.tsv --output derep_Full_12S_Total_PCR.tsv --method uniq_species

seq_cleanup

24) crabs seq_cleanup --input derep_Full_12S_Total_PCR.tsv --output seq_cleanup_Full_12S_Total_PCR.tsv --minlen 100 --maxlen 500 --maxns 0 --enviro yes --species yes --nans 0

db_completeness
8.3

25) crabs visualization --method db_completeness --input seq_cleanup_Full_12S_Total_PCR.tsv --species species.txt --taxid nodes.dmp --name names.dmp -o test
