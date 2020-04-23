# Building and Using Hidden Markov Models (HMM) with HMMER
From the [HMMER manual](http://eddylab.org/software/hmmer3/3.1b2/Userguide.pdf):  
HMMER is used to search sequence datasets for homologs of protein or DNA sequences, and to make sequence alignments. HMMER can be used to search a sequence databases with single query sequences but it becomes particularly powerful when the query is an alignment of multiple instances of a sequence family. HMMER makes a profile of the query that assigns a position-specific scoring system for substitutions, insertions, and deletions. HMMER profiles are probabilistic models called “profile hidden Markov models” (profile HMMs).
Compared to BLAST, FASTA, and other sequence alignment and database search tools based on older scoring methodology, HMMER aims to be significantly more accurate and more able to detect remote homologs, because of the strength of its underlying probability models. In the past, this strength came at a significant computational cost, with profile HMM implementations running about 100x slower than comparable BLAST searches for protein search, and about 1000x slower than BLAST searches for DNA search. With HMMER3.1, HMMER is now essentially as fast as BLAST for protein search, and roughly 5-10x slower than BLAST in DNA search.

__Note__  
To get the manual for one of the programs in HMMER, use either `man hmmer`, or specifically for one of the tools use `man hmmsearch` or `hmmsearch -h`, or lastly refer to the [HMMER manual](http://eddylab.org/software/hmmer3/3.1b2/Userguide.pdf)

### Step 1:  Install ___hmmer v3.3___
_Install hmmer v3.3 (Mac OSX)_
```bash
# Download the program source code from hmmer.org (v3.3)
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

# Uncompress the protein sequences and change name
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
