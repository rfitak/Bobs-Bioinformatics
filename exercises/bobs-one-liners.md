# Bob's One Liners

### File transfer (`scp`, `rsync`)
For a small file transfer, I often use `scp`, but ___ALWAYS___ use `rsync` for large files (> a few GB)
```bash
# scp a file
scp file.tar.gz rfitak@coombs.oit.ucf.edu:~/FOLDER/

# scp a folder and its contents
scp -r rfitak@coombs.oit.ucf.edu:~/FOLDER/SOURCE LOCAL/DESTINATION

# rsync
rsync --rsh='ssh' -av --progress --partial file.tar.gz rfitak@coombs.oit.ucf.edu:~/FOLDER/
```

<br>

### SLURM Quickies
Short commands I often use to check the status or retrieve innformation about resources (e.g., memory, time) each job used
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
```

### Making and unpacking Tarballs
```bash
#TBD
```
