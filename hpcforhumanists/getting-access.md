# Getting access

The exact process to get access to the HPC system varies by institution. Most commonly, there's some kind of HPC system available for all faculty to use, free of charge, potentially with some limits (e.g. a certain number of "credits" or other usage caps per academic year.) Even if the primary users of the HPC cluster are grad students or staff, the faculty member needs to request an account for their group (which will share a single quota, if the HPC system has quotas), and accounts for individual users within the group.

Some institutions also have a student-oriented system, which students can use for their own projects without approval from a faculty member.

If you're not sure where to start looking for this information at your institution, search for your institution's name plus "Research Computing", which is what the IT support group that runs HPC is usually called.

## 🌲 Farmshare
At Stanford, [Farmshare](https://web.stanford.edu/group/farmshare/cgi-bin/wiki/index.php/User_Guide) is the "community computing environment", for anyone with a SUNet ID to use. It is much smaller than Sherlock: a couple hundred cores[^1] [^1]: What's a "core?" It's a processor, and on some HPC systems including Stanford's, each core of each server in the cluster can be "scheduled", or given a task to do for a certain amount of time. A recent MacBook Pro has four cores by default. 

for compute, vs. thousands on Sherlock. Given its smaller size and potentially much larger number of users, you're better off using Sherlock if you're working with a faculty member who can get you an account. (For instance, if you're a grad student, you can ask your advisor.)

Farmshare doesn't require any setup or account-creation in advance; anyone with a SUNet ID can log in. If you want to play around with some of the HPC commands, or use HPC for a class project, you can start by [logging into](logging-in) Farmshare.

## 🌲Sherlock
Sherlock is Stanford's HPC system for research, and is much larger than Farmshare, which typically means shorter wait times. You need to be working with a faculty member in order to get access. The [Sherlock documentation](https://www.sherlock.stanford.edu/docs/getting-started/?h=sherlock+request+accou) says to ask the faculty member to email `srcc-support@stanford.edu` to request a Sherlock account. They should also include the SUNet IDs of everyone who should have access to Sherlock as part of their group. You'll receive an email when your account is ready, at which point you can [log in](logging-in).
