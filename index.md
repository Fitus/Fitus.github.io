# What are synchronization programs good for ?

For <b>Backups</b>, for <b>Transferring of Huge Data</b>, for <b>Taking Your Work With You</b>

## Backups

Backups are additional copies of your data. If something goes wrong with your primary data
(e.g. a disk failure, fire, flood, a cyber-attack or just an accidental deletion),
the backups will save you.

### What storage media are used for backups today ?

The times when backup data has been written to magnetic tapes are gone.
Even the times when backup data has been burned to CDROM's and DVD's are gone.
Today, HDD's, SSD's and USB flash drives have such immense capacities and access speeds
and are so cheap that they are the backup media of choice.

Also, having a second HDD/SSD as backup media in a PC is now a frequent solution.
This protects against failure of the primary HDD/SSD, but not against other risks like fire,
flood, or a cyber attack. These risks are addressed by media that are connected only
when backups are made, and otherwise are disconnected and (ideally) stored in a different location.

### The role of a synchronization program

Let's first clarify what a synchronization program actually does:

* It takes a source directory and a backup directory.
* It goes through both directories and compares the files.
* If it finds files that differ between source and backup, it copies them to backup.
* If it finds files that exist only in the source directory, it copies them to backup too.
* If it finds files that exist only in the backup directory, it removes them from backup.

Well, this is a bit simplified, but sufficient for now.

A synchronization program makes it possible to write to backup media only the files
that have been changed (or have been newly created) since the last backup.
It is not necessary to overwrite each time the whole backup data with whole source data.

### It is not always that simple

The above said should not give the impression that backups are always that simple.
If databases are involved, and/or in high-availability IT systems, backups require complex solutions.
Synchronization programs might be parts of such solutions, but not the solutions as such, of course.

## Transferring of Huge Data

We are talking about data so huge (TeraBytes range and higher) that transferring them over a network is unfeasible.
There are tools (e.g. RSYNC) that attempt to optimize such transfers by splitting files into parts and transfer only the parts that differ.
However, in last years a new trend emerged (try to google, for instance, for AWS Snowmobile, which is an (extreme) example of this trend):
A mobile storage with sufficient capacity is connected to the source machine and data is synchronized to the mobile storage.
Then the mobile storage is physically transferred to the location of the backup machine, connected to the backup machine,
and data is synchronized from the mobile storage to the backup machine.

The mobile storage, in that case, holds third copy of the data and can be stored at a third location, which is an extra benefit.
Additionally, if the mobile storage is used repeatably, only the changed (or new) files are copied during the synchronizations,
not whole source data.

## Taking Your Work With You

Imagine you commute (regularly or irregularly) between two workplaces:
At both places, you work with a software (e.g. a CAD program) that saves its data to files and directories.
After you finish at one location, you can synchronize your work data to a mobile storage (e.g. a USB flash drive),
take that storage with you, and at the second location synchronize the data from the mobile storage to the work area of the CAD program.
This allows you to continue your work at the second location ...

# Choice of a synchronization program

Several tens of backup and synchronization programs exist.
One can choose between commercial programs and open source/free programs,
between GUI-based programs and command-line programs,
and many programs offer special features like operation over networks,
encryption of data, automatic scheduling,
special data transfer protocols and/or storage formats targeted at
minimizing network traffic and/or storage media use and so on.

This place hosts the [Zaloha.sh](https://github.com/Fitus/Zaloha.sh) synchronizer,
along with [Zaloha_Snapshot.sh](https://github.com/Fitus/Zaloha_Snapshot.sh),
an add-on script to create hardlink-based snapshots of the backup directory.

The following reasons speak for using these programs:

* The programs follow the [KISS (Keep it Stupid and Simple) principle](https://en.wikipedia.org/wiki/KISS_principle):
  simple is better than complex, small is better than big.

* The backup directory (and eventual snapshot directories too) are regular directories
  with regular files, not special forms of data that need special tools to extract.

* The programs (as such) are safe in the sense that they never do potentially destructive
  actions without user's confirmation.

* [Zaloha.sh](https://github.com/Fitus/Zaloha.sh) first analyses the directories
  and presents the prepared actions to the user for confirmation.
  Actions requiring more attention are displayed in red color (with <b>--color</b> option).
  Such human oversight is a good countermeasure against accidental mishaps
  on the source directory - the user might detect them at backup time.

* [Zaloha.sh](https://github.com/Fitus/Zaloha.sh) is able to compare files byte by byte (<b>--byteByByte</b> option).
  Although this requires a **significantly** longer analysis phase (over the standard comparisons
  of just the file sizes and modification times), it should be used from time to time to ensure
  that the backup data is indeed readable and identical to source data.

* Other features and modifiers exist, see the [online documentation of Zaloha.sh](https://github.com/Fitus/Zaloha.sh/blob/master/DOCUMENTATION.md).

* [Zaloha_Snapshot.sh](https://github.com/Fitus/Zaloha_Snapshot.sh) creates snapshots of the backup directory.
  Snapshots are directories with contents of the backup directory fixated at the times when they have been created.
  The snapshots utilize hardlinks, which must be supported by the underlying filesystem type.
  The benefit is that unchanged files are physically stored only once, independently on how many snapshots exist.
  For details, see the [online documentation of Zaloha_Snapshot.sh](https://github.com/Fitus/Zaloha_Snapshot.sh/blob/master/DOCUMENTATION.md).

* Both programs are [BASH](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) scripts that run on Linux/Unics natively,
  and use only standard system commands
  [FIND](https://en.wikipedia.org/wiki/Find_(Unix)),
  [SORT](https://en.wikipedia.org/wiki/Sort_(Unix)),
  [AWK](https://en.wikipedia.org/wiki/AWK),
  [MKDIR](https://en.wikipedia.org/wiki/Mkdir),
  [RMDIR](https://en.wikipedia.org/wiki/Rmdir),
  [CP](https://en.wikipedia.org/wiki/Cp_(Unix)),
  [RM](https://en.wikipedia.org/wiki/Rm_(Unix)) and
  [LN](https://en.wikipedia.org/wiki/Ln_(Unix)).
  They neither introduce new binary code to the system, nor do they open new ports,
  and are easily reviewable to ascertain that no malware/bloatware/spyware is contained.
  On Windows, however, [Cygwin](https://en.wikipedia.org/wiki/Cygwin) must be installed before.

* Due to being BASH scripts and not compiled binary programs, knowledgeable users are not prevented from
  using them, fully or partially, as a basis for building more complex solutions.
