Cgywin: Taking back control over Windows (lab computers)
================
Philipp Baumann || <philipp.baumann@usys.ethz.ch>

-   [Overview](#overview)
-   [Updating packages](#updating-packages)
-   [Mounted drives](#mounted-drives)
-   [Backup](#backup)

Overview
--------

[Cgywin](https://cygwin.com/) is aimed at bringing Unix tools to the Windows environment. This repo contains an opinionated set of recipes, and is particularly tailored to people suffering from Windows computers of measurement devices in laboratories.

Updating packages
-----------------

The Cgywin distribution ships only base packages by default. The executable `setup-x86.exe` provides updates and serves to install new packages. You can also access setup functionality via the Cgywin shell, for example:

``` bash
# Change to the directory where `setup-x86_64.exe` is located
setup-x86_64.exe -q -P wget -P gcc-g++ -P make -P diffutils -P libmpfr-devel -P libgmp-devel -P libmpc-devel
```

Mounted drives
--------------

All system drives such as `C:` are mapped under `/cgydrive`, for example `C:` can be accessed via `/cgydrive/c`. By default, the "root" directory is in `C:\cygwin`.

Backup
------

Below is an example how to backup spektrometer sofware located in public documents to a NAS mounted as `Q:`. Copy this shell script in a text file called `backup-pdocs.sh` in the Cgywin home folder (see [here](https://www.howtogeek.com/175008/the-non-beginners-guide-to-syncing-data-with-rsync/) to get a glimpse of the magic):

``` bash
#!/bin/bash

# copy old time.txt to time2.txt
yes | cp ~/backup/time.txt ~/backup/time2.txt

# overwrite old time.txt file with new time
echo `date +”%F-%I%p”` > ~/backup/time.txt

# make the log file
echo “” > ~/backup/rsync-`date +”%F-%I%p”`.log

# rsync command
rsync -avzhPR --chmod=Du=rwx,Dgo=rx,Fu=rw,Fgo=r --delete --stats --log-file=~/backup/rsync-`date +”%F-%I%p”`.log --link-dest=/cygdrive/c/Users/Public/Documents/Bruker/`cat ~/backup/time2.txt` /cygdrive/c/Users/Public/Documents/Bruker /cygdrive/q/INSTRUMENT-PCs-BACKUP/Bruker/`date +”%F-%I%p”`/

# don’t forget to (s)cp the log file and put it with the backup
cp ~/backup/rsync-`cat ~/backup/time.txt`.log /cygdrive/q/INSTRUMENT-PCs-BACKUP/Bruker/`cat ~/backup/time.txt`/rsync-`cat ~/backup/time.txt`.log
```

Then, make it executable with

``` bash
chmod +x ~/backup-pdocs.sh
```

Last but not least, install the `dos2unix` utility to change from two-character linebreaks (DOS/Windows) to one-character (line feed; Unix):

``` bash
# change to the folder that contains the executable (in Cgywin shell)
setup-x86_64.exe -q -P dos2unix
```

Then, convert it with

``` bash
dos2unix ~/backup-pdocs.sh
```

Rock'n'roll backup is done like this:

``` bash
# Create first time file
mkdir ~/backup
touch ~/backup/time.txt
# Backup
./backup-pdocs.sh
```
