#H&S Partition

There are now 83 compute nodes in hns, including 9 nodes with 128 CPUs/1TB RAM for a total of 2,832 CPUs. You may find that your jobs will wait for less time in the hns partition than in the normal partition.  To submit jobs to hns add the following to your sbatch scripts-

#SBATCH -p hns

Use "sinfo -Nlp hns" to see the different node types in hns.

You can use the "sh_part" command to see limits and what partitions you can run on.

You can also see detailed submission limits with scontrol:

sacctmgr show qos format=Name,MaxTRESPerUser,MaxSubmitJobsPerUser,MaxJobsPerUser,MaxWall,MaxTRESPA,MaxSubmitJobsPerAccount

If you have any questions let me know. 

Support email:

srcc-support@stanford.edu

We also have prepared Sherlock onboarding slides that you may find helpful here
( https://srcc.stanford.edu/sites/g/files/sbiybj10261/f/sherlock_onboarding-03-2022_1.pdf) 

Session can be viewed on YouTube: https://youtu.be/iqq7GGqMRg8
