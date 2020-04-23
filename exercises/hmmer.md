# Building and Using Hidden Markov Models (HMM) with HMMER
description

### Step 1:  Install ___hmmer v3.3___
_Install hmmer v3.3 (Mac OSX)_
```bash
# Download the program source code from hmmer.org
curl -O http://eddylab.org/software/hmmer/hmmer.tar.gz

# Unpack the tarball
tar -zxvf hmmer.tar.gz

# Move into the hmmer folder
cd hmmer

# Configure and install hmmer
    # read the INSTALL file for more information if necessary
./configure
make
make install
   # or `sudo make install` if you need to login to root
```

### Step 2:  Build a HMM from an alignment of sialin protein sequences
```bash
# Build the HMM
hmmbuild \
   -n sialin \
   --amino \
   sialin.hmm \
   sialin.aligned.fasta
```
_Parameters explained_
- -n sialin :: name of the hmm
- --amino :: input sequences are amino acid (proteinn)
- sialin.hmm :: name of the output hmm file
- sialin.aligned.fasta :: name of the input fasta alignment

### Step 3: Seach an input file of protein sequences with a HMM
Use `hmmsearch` to compare ≥1 protein sequencce against a single HMM, whereas `hmmscan` is for searching a protein database with a database of HMMs.
```bash
hmmsearch \
   -o sialin.matches \
   --tbl sialin.matches.tsv \
   sialin.hmm \
   sialin.fa
```
_Parameters explained_
- -o sialin.matches :: name of the full output generated
- --tbl sialin.matches.tsv :: a tab-separated table of the results, easier to parse in Excel or R
- sialin.hmm :: name of the hmm file to use
- sialin.fasta :: name of the input fasta file of unaligned protein sequences

### Step 4:  Compare HMM to entire O. bimaculoides proteome
```bash
# Download octopus proteome (found the URL through the NCBI Taxonomy browser
curl -O https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/001/194/135/GCF_001194135.1_Octopus_bimaculoides_v2_0/GCF_001194135.1_Octopus_bimaculoides_v2_0_protein.faa.gz

# Uncompress the protein sequences and channge name
gunzip GCF_001194135.1_Octopus_bimaculoides_v2_0_protein.faa.gz
mv GCF_001194135.1_Octopus_bimaculoides_v2_0_protein.faa Obimac.faa

# Count how many sequencnes there are
grep -c "^>" Obimac.faa
   # Result: 23,994

# Compare HMM to proteome
hmmsearch \
   -o sialin.matches \
   --tbl sialin.matches.tsv \
   sialin.hmm \
   Obimac.faa
```
