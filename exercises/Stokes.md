# Getting Started on Stokes at UCF
Stokes is the high-performance computing (HPC) cluster for the Genomics and Bioinformatics Cluster at UCF. You can find a brief description and mini documentation of Stokes here: https://rci.research.ucf.edu/resource/stokes/.

```bash
# Login to stokes (NOTE: must be connected to the VPN)
ssh username@stokes.ist.ucf.edu

# Login and enable X-windows forwarding (e.g., to open R plot windows)
ssh -Y username@stokes.ist.ucf.edu

# I recommend making an alias like this:
alias stokes='ssh -Y username@stokes.ist.ucf.edu'

# Then just enter:
stokes

# Save your alias permanently on your personal computer:
echo "alias stokes='ssh -Y username@stokes.ist.ucf.edu'" >> ~/.bash_profile
```
---
### File transfer to and from Stokes (`scp`, `rsync`)
For a small file transfer, I often use `scp`, but ___ALWAYS___ use `rsync` for large files (> a few GB)
```bash
# Copy a file securely to Coombs
scp file.tar.gz username@stokes.ist.ucf.edu:~/PATH/TO/FOLDER/

# Copy a folder and its contents securely to Coombs
scp -r LOCAL/DESTINATION username@stokes.ist.ucf.edu:~/PATH/TO/FOLDER/

# Copy a file securely from Coombs to local computer
scp username@stokes.ist.ucf.edu:~/PATH/TO/FOLDER/file.tar.gz /LOCAL/FOLDER/

# Copy a folder and its contents securely from Coombs to local computer
scp -r username@stokes.ist.ucf.edu:~/PATH/TO/FOLDER/ LOCAL/FOLDER

# Using rsync for copying a file to Coombs
rsync --rsh='ssh' -av --progress --partial file.tar.gz username@stokes.ist.ucf.edu:~/FOLDER/

# Using rsync for copying a file from Coombs to local computer
rsync --rsh='ssh' -av --progress --partial username@stokes.ist.ucf.edu:~/FOLDER/file /LOCAL/FOLDER
```
---
### Checking and confirming file transfers
It is good practice when transferring files, especially when sharing with colleagues, to confirm that the file was transferred perfectly. This is done using an MD5 hash. MD5 (Message-Digest algorithm 5) is a cryptographic hash function that takes any input data and produces a unique, fixed-size 128-bit hash value or fingerprint. Its primary purpose is to verify data integrity by creating a unique "fingerprint" of a file or message, allowing users to check if the data has been altered or corrupted during transmission or storage.

```bash
# Print MD5 for a file
md5sum file

# Save MD5 for all files in a folder in a new file:
md5sum * > MD5.txt

# If someone sent you the folder above and the MD5.txt file,
# You can confirm each MD5 like this:
md5sum -c MD5.txt
   # the "-c" means to check all the tags listed in the following file, 1 tag per line.
```
---
### Setting up your "profile"
```bash
# View all files, including hidden ones
ls -a

# Assuming no file called ".bashrc" exists, make one
touch .bashrc
   # We will added customizations, aliases, etc to this file
   # which is read everytime you log in to the cluster.

# Make a hidden folder in your HOME folder (i.e., $HOME or ~) called "local"
   # in the future, this is where you should install software.
mkdir .local

# Add it permanently to your path
echo "export PATH=${HOME}/.local/bin:${PATH}" >> .bashrc
```
---
### SLURM Quickies
Short commands I often use to check the status or retrieve information about resources (e.g., memory, time) each job used
```bash
# Submit a job
sbatch bobjob1.sh

# Submit a job with SLURM options, and arguments to the actual job script
sbatch -J bobjob1 -e bobjob1.err -o bobjob1.out bobjob1.sh file1 file1

# Get list of completed jobs since (-S) March 31, 2020 for user (-r) username
sacct -S 3.31.20 -u username

# Get all the gory details (-l) for a specific job ID (-j)
sacct -j 179506 -u username -l

# Same as above but only certain columns of information, change memory to (G)igabytes
sacct -j 179506 -u username --format="NCPUS,CPUTime,MaxRSS,JobID" --units=G

# Set a new default format for job info using ann environmental variable
export SACCT_FORMAT="JobId,JobName,User,Account,NCPUS,Elapsed,MaxRSS"
sacct -j 179506 --units=G

# Add the new format permanently to your environment
echo "export SACCT_FORMAT=\"JobId,JobName,User,Account,NCPUS,Elapsed,MaxRSS\"" >> .bashrc

# Run an interactive job with bash (log into a node just like a normal ssh session:
srun --pty bash          # default is 1 cpu
srun -c 4 --pty bash     # run interactive job with 4 cpus
srun -c 4 --nodelist=ec28 --pty bash     # run interactive job with 4 cpus on node "ec28"
srun --mem=64G --nodelist=ec28 --pty bash     # run interactive job with a total of 64G memory on node "ec28"
srun --time=10:00:00 --mem=64G --nodelist=ec28 --pty bash  # run interactive job with 10-hr time limit with 64G memory on node "ec28"

# When finished with an interactive session:
exit
```

<br>

### Making and unpacking Tarballs
More often than not, a folder of files is shared in format called a "tarball".  This tarball is often compressed with `gzip` and shared as a file in the form `<name>.tar.gz`.  Large datasets, and especially software packages, are shared in this format.  Sometimes, the compressed formats of `xz` and `bz2` are used. Here are a few of the most common ways to deal with them.
```bash
# Most common: Unpack a tarball into a folder of files
tar -zxvf file.tar.gz
   # -z :: uncompress using gzip (gunzip)
   # -x :: extract the contents of the tarball
   # -v :: verbose output for debugging and trackinng errors
   # -f :: input is a file

# List the contents of a tarball (sometimes you only want a few files, not the entire contents.  This saves space.
tar -tvf file.tar.gz
   # -t :: list the contents of the tar archive

# Extract just one of the files (Folder/file.1.txt) listed in the output above
tar -zxvf file.tar.gz Folder/file1.txt

# Make your own tarball from a folder of files
tar -zcvf test.tar.gz TEST-FOLDER
   # -z :: compress using gzip
   # -c :: create a new archive from a folder

# Working with xz files (slow but high compression)
tar -cvJf /path/to/FOLDER.xz FOLDER   # compress
tar -xvJf /path/to/FOLDER.xz          # extract

# Working with bz2 files
tar -cvjf /path/to/FOLDER.bz2 FOLDER   # compress
tar -xvjf /path/to/FOLDER.bz2          # extract
```

_Anaconda (conda) environments on Stokes cluster_
```bash
# Load anaconda
module load anaconda/anaconda-2024.10

# Initialize conda environment permanently
conda init   # answer questions... but this should modify your .bashrc file. check and confirm.

# Initialize an 'environment' (only need to do once), change "conda-env" to your preferred name
conda create -y -n conda-env

# Load/activate the environment
source activate conda-env

# Install a program that uses conda
conda install -c bioconda fastq_utils

# See a list of installed programs in conda
conda list

# Once finished with analyses, deactivate conda environment
conda deactivate
```

<br>

### Working with the Singularity containers on Coombs
```bash
# Load module - apptainer is the equivalent of singularity
module load apptainer/apptainer-1.3.3-go-1.22.5

# download/build container from repository (example using BUSCO)
# Must be on a compute node
srun --pty bash  # login to a computer node as interactive job
singularity pull busco_5.8.0.sif  docker://ezlabgva/busco:v5.8.0_cv1

# Run a container (sif) once built
singularity exec -e /path/to/delly_v1.1.6.sif delly
singularity exec -e /path/to/busco_5.8.0.sif busco

# Exit interactive job on the compute node
exit
```
