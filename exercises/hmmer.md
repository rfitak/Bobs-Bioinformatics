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
