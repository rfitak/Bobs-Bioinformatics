# Bob's One Liners

### SLURM Quickies
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
sacct -j 179506

```
