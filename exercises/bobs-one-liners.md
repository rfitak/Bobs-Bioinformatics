# Bob's One Liners

```bash
# Covert fastq to fasta format (seqtk version)
seqtk seq -A input.fq > output.fa

# Covert fastq to fasta format (perl version)
perl -e '$i=0;while(<>){if(/^\@FCC/&&$i==0){s/^\@/\>/;print;}elsif($i==1){print;$i=-3}$i++;}' input.fq > output.fa

# Convert fasta to non-interleaved format (seqtk version)
seqtk seq -l0 input.fa > output.fa

# Convert fasta to non-interleaved format (perl version)
perl -ne 'chomp $_; if ($_ =~ /^>/){print "\n$_\n";}else { print "$_";}' input.fa | sed '1d' > out.fa

# Use awk to fill the SNP ID column of a vcf file
awk 'BEGIN {x=1} {OFS="\t"} !/^#/ {$3="SNP_"x++} {print}' input.vcf > output.vcf
   # replaces "." in the VCF's ID column with "SNP_X", X = a simple counter.

# Check if a particular perl module is installed (e.g., XML::Parser)
perl -MXML::Parser -e "print \"Module installed.\\n\";"

# Rotate a photo custom degrees and fill with white (not really bioinformatics???) (-r = degrees clockwise)
sips -r 352 --padColor FFFFFF infile.jpg --out outfile.jpg
```

### File transfer (`scp`, `rsync`)
For a small file transfer, I often use `scp`, but ___ALWAYS___ use `rsync` for large files (> a few GB)
```bash
# scp a file
scp file.tar.gz rfitak@coombs.cs.ucf.edu:~/FOLDER/

# scp a folder and its contents
scp -r rfitak@coombs.cs.ucf.edu:~/FOLDER/SOURCE LOCAL/DESTINATION

# rsync
rsync --rsh='ssh' -av --progress --partial file.tar.gz rfitak@coombs.cs.ucf.edu:~/FOLDER/
```

<br>

### SLURM Quickies
Short commands I often use to check the status or retrieve information about resources (e.g., memory, time) each job used
```bash
# Submit a job
sbatch bobjob1.sh

# Submit a job with SLURM options, and arguments to the actual job script
sbatch -J bobjob1 -e bobjob1.err -o bobjob1.out bobjob1.sh file1 file1

# Get list of completed jobs since (-S) March 31, 2020 for user (-r) rfitak
sacct -S 3.31.20 -u rfitak

# Get all the gory details (-l) for a specific job ID (-j)
sacct -j 179506 -u rfitak -l

# Same as above but only certain columns of information, change memory to (G)igabytes
sacct -j 179506 -u rfitak --format="CPUTime,MaxRSS,JobID" --units=G

# Set a new default format for job info using ann environmental variable
export SACCT_FORMAT="JobId,JobName,User,Account,NCPUS,Elapsed,MaxRSS"
sacct -j 179506 --units=G

# Run an interactive job with bash (log into a node just like a normal ssh session: USE SPARINGLY!)
srun --pty -I bash
```

<br>

### Making and unpacking Tarballs
More often than not, a folder of files is shared in format called a "tarball".  This tarball is often compressed with `gzip` and shared as a file in the form `<name>.tar.gz`.  Large datasets, and especially software packages, are shared in this format.  Here are a few of the most common ways to deal with them.
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
```

_Anaconda (conda) environments on COOMBS cluster_
```bash
# Load anaconda
module load anaconda3/2024.02-1

# Initialize an 'environment' (only need to do once)
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

### Working with the NCBI taxonomy database using the [ETE toolkit](http://etetoolkit.org/documentation/ete-ncbiquery/)
_Install the [ETE toolkit](http://etetoolkit.org/documentation/ete-ncbiquery/)_
```bash
# Using conda
module load anaconda3/2024.02-1

# Load/activate the environment
source activate conda-env

# Install ETE
conda install -c etetoolkit ete3 ete_toolchain
conda install -c etetoolkit ete3_external_tools
ete3 upgrade-external-tools

# Check installation
ete3 build check
```

_Examples:_
```bash
# Get full lineage info for a species name ("Canis familiaris") and taxon ID (9606 = humans)
ete3 ncbiquery --search 9606 'Canis familiaris'  --info

# Display tree in terminal for selected taxa
ete3 ncbiquery --search 9606 'Mus musculus' 'Gallus gallus' 7227 --tree | ete3 view --ncbi --text

# Save as a newick tree file
ete3 ncbiquery --search 9606 'Mus musculus' 'Gallus gallus' 7227 --tree > tree.nwk

# Once finished with analyses, deactivate conda environment
conda deactivate
```

### Working with the Singularity containers on Coombs
```bash
# Load module
module load singularity   # v1.0.3
# alternatively, just use the command 'apptainer'
apptainer version
   # 1.0.3

# download/build container from repository (example using BUSCO)
# Must be on a compute node
srun --pty -I bash  # login to a computer node as interactive job
singularity pull busco_5.8.0.sif  docker://ezlabgva/busco:v5.8.0_cv1

# Run a container (sif) once built
singularity exec -e /path/to/delly_v1.1.6.sif delly
singularity exec -e /path/to/provean\:1.1.5--h87f3376_1 provean

# Exit interactive job on the compute node
exit
```

### Make a random ID code for labeling samples in the lab using R
_For 500 samples using 3 letters_
```R
cat(
   paste0(
      sample(
         apply(
            expand.grid(LETTERS, LETTERS, LETTERS, stringsAsFactors = F),
         1, paste, collapse = ""),
      500),
   formatC(c(1:500), width = 3, flag = "0")),
sep = "\n")

# As one line:
cat(paste0(sample(apply(expand.grid(LETTERS, LETTERS, LETTERS, stringsAsFactors = F), 1, paste, collapse = ""), 500), formatC(c(1:500), width = 3, flag = "0")), sep = "\n")
```
_For 17,576 samples using 3 letters_
```R
cat(
   paste0(
      sample(
         apply(
            expand.grid(LETTERS, LETTERS, LETTERS, stringsAsFactors = F),
         1, paste, collapse = "")),
   formatC(c(1:17576), width = 5, flag = "0")),
sep = "\n")

# As one line:
cat(paste0(sample(apply(expand.grid(LETTERS, LETTERS, LETTERS, stringsAsFactors = F), 1, paste, collapse = "")), formatC(c(1:17576), width = 5, flag = "0")), sep = "\n")
```
