# crabscode  
This is my code for getting crabs to work (https://github.com/gjeunen/reference_database_creator)  
module load Miniconda3/4.12.0 (to load miniconda) 
(we don't need to install conda because it is already on NeSI, so this is how we get access to it)  
conda create --prefix=/nesi/nobackup/uoo00105/ryan/crabs/crabs_env  
(this is instead of GJ's instruction: conda create -n CRABS
the reason we are doing it using the "prefix" option, is that installs all the packages locally rather than in our home directory. The home directory can get jammed up with too many packages and stop working correctly
after that, to activate the environment so we can install things into it we do:)
conda activate /nesi/nobackup/uoo00105/ryan/crabs/crabs_env
(this is instead of GJ's command: conda activate CRABS
and this is because we've changed where all the packages will go, so we also have to change where the computer will look to access all of those packages)
conda config --add channels bioconda  
conda config --add channels conda-forge  
conda install -c bioconda crabs  
crabs -h (check installation)  
Now that I have installed it I will only need to run:  
module load Miniconda3/4.12.0 (to load miniconda)  
$ conda activate /nesi/nobackup/uoo00105/ryan/crabs/crabs_env  
