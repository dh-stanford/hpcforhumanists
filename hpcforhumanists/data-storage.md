# Data storage
Data storage is a little more complex on an HPC system than on your own computer. There are three different places where you might put data, each with a different purpose and set of constraints. It's worth understanding each of them a little before you jump into data transfer.

## Home directory
Individual users and groups on HPC clusters will have a small amount of storage space called a "home directory". A user's home directory is the directory in the filesystem that they're located in when they log into the cluster. If you don't have access to, or can't pay for HPC storage for data you'll use multiple times in the HPC cluster, you can keep it in your home directory. Any scripts that you want to run are also good to put in your home directory.

## HPC storage
The name of this particular service may vary across institutions, but HPC clusters almost always have some kind of associated storage that is intended for long-term use, and is regularly backed up. There may be an annual or monthly charge, or a certain amount of storage might come free if you or your group purchase a compute node.

Depending on how large your data is, and how often you need to analyze it using the HPC cluster, it may make sense to store a copy of your data on the HPC storage. 

When working with HPC storage, you may see something in the documentation about limits on the number of *inodes* you can have, in addition to limits on total storage space. Every *inode* refers to one file, and HPC systems are often set up using assumptions about the likely number of files each group will have, based on common practice in fields like astrophysics. Humanists and social scientists can quickly blow through stated or implicit *inode* quotas with a few copies of something like the Google N-Grams data set, which isn't that large (in gigabytes) but which contains hundreds of thousands of tiny files. If you're going to store a data set on HPC storage, you should compress it into a single file (e.g. .zip or .tar.gz) and put *that* on HPC storage, not a folder full of individual files.

## Scratch storage
Scratch storage is a different kind of storage connected to an HPC cluster. Much like scratch paper, it's meant to be used temporarily while you're computing. There's no backup for things on scratch, and most scratch systems regularly delete files that haven't been accessed (or modified) recently, though the details and frequency of scratch purges varies from system to system.

The ephemerality of scratch may make you wary, but don't let it scare you away: when you're doing computation on an HPC cluster, you *need scratch*. Scratch is **fast** for the cluster to read from and write to, much faster than the HPC storage system. And even if you feel like you can be patient and aren't in any particular hurry, you should still use scratch as your primary storage location for when you're doing computation on an HPC cluster. The faster you can get your computation done, the less time you have to request for your jobs on the cluster. And the less time you have to request, the less time you have to wait for your jobs to run.

## Data storage in practice
Here's what two workflows for data storage look like, for different kinds of projects.

### New analysis, old corpus
A grad student wants to do research using the Early English Books Online (EEBO) corpus, the largest corpus of Early Modern English which has an "open source" version. Their advisor already has a copy of EEBO stored in their group's HPC storage space. EEBO is the only data source the student needs for this analysis.

The student logs into the HPC cluster using the command line, and runs a command to copy the zipped directory with the EEBO corpus from HPC storage onto scratch storage. (Copying files around is something that's fine to do on the login node.) They unzip the copy of EEBO on scratch, and set up their job to read the data from the unzipped copy of EEBO on scratch, process it through an analysis script they have stored in their home directory, then write the results back to scratch. The final step of their job copies all the result files back to their home directory.

### Corpora, corpora everywhere
A faculty member wants to run an analysis that combines the Chicago Corpus (which they have a copy of, zipped up, on HPC storage) and some books their library has given them access to on the condition that they are only used for non-consumptive analysis on the HPC cluster. They also want to use a few text files that they currently have on their own computer.

The faculty member logs into [Globus](data-transfer.md#Globus), using their computer as one endpoint and scratch storage as the other endpoint. They click around their computer, find a few files in different places, and transfer them to scratch using the Globus web interface. It's okay if those files get purged from scratch; they have a copy on their own computer, and the files don't form a coherent corpus that they'll want to reuse in the future. While they have Globus open, they change the endpoint from their computer to HPC storage, and use Globus to transfer the zipped files for the Chicago corpus and the library books to scratch as well.

They then log into the HPC cluster, and use the command line to unzip the files with the Chicago Corpus and library books. They may rearrange the particular files they need so they're all in the same directory, to make it easier to run their job. (At some point, if they need to do a lot of file rearranging, they might log into Globus and use the web interface to move files around.) Finally, they run a job using a script stored in their home directory, and output the results to scratch, copying the output files to their home directory as the last step.

## 🌲 Storage limits & Oak
At Stanford, the HPC storage service is called [Oak](https://uit.stanford.edu/service/oak-storage), which is a paid service where faculty rent space on a month-to-month basis in either 10 TB (\$45/mo) or 250 TB increments (\$587/mo). It can't be used with high-risk data (e.g. ebooks that you've decrypted as part of the DMCA exemption for text and data mining).

Your Home directory on Sherlock has a [limit of 15 GB](https://www.sherlock.stanford.edu/docs/storage/filesystems/#home); the Group Home directory for the group you belong to (i.e. your PI's group) has a [limit of 1 TB](https://www.sherlock.stanford.edu/docs/storage/filesystems/#group_home). On Scratch, you're limited to [100 TB and 20M inodes](https://www.sherlock.stanford.edu/docs/storage/filesystems/#scratch), and all data not *modified* (which doesn't include reading it, just changing it) in the last 90 days gets deleted... although any folders and subfolders do not, which can leave you with lots of nested empty folders in scratch that you need to clean up from time to time.
