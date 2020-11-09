# Using Rclone
**2019-11-13**
*Luis*

https://mad.oxli.org/t/2019-11-13-how-to-backup-data-on-a-remote-computer/82

Presentation: https://hackmd.io/@0wHfIv81QFumMW8-6edr2w/S1S6qRKjS#/

## What to Backup?

 - raw
 - results
 - code to github

## How to Backup?

See [UCD Resource Page](https://ucdavisit.service-now.com/servicehub/?id=ucd_kb_article&sys_id=3f20e2b56f712d40bc4f8a20af3ee4b2)

 - If part of UC Davis, can use:
   - Box (unlimited, no cost)
   - Google Drive (unlimited, no cost)
   - One Drive (1TB, no cost, good for windows machine)


## `rclone`

From farm use transfer node:

```
ssh -p 2022 rapeek@farm.cse.ucdavis.edu
```

*My shortcut: `farmT`

### On Transfer node

1. Load rclone: `module load rclone`


### Setting up `rclone` on Box

On the transfer node (farm):

1. Load rclone: `module load rclone`
2. Make a remote: `rclone config`
3. Pick an option: Here we select Box (option 6)
4. Enter through everything until edit adanced config: N
5. Use auto config? N
6. Copy command in your **local** terminal: `rclone authorize "box"`
7. Log in to UCD box account
8. Copy token: and enter yes this is ok
9. Exit config
10. Check remotes setup: `rclone listremotes`

### Working with Remote/box

**Copy**

 - list files: `rclone lsd box`
 - to copy something: `rclone copy -P LOCALFILE box:remotedirectory/`
   - `-P` gives some feedback about the process, otherwise there's no indication anything is happening
   - For googledrive this looks like this:
   - `rclone copy -P bash_snippets.md gdrive:SEQS_backup`

**Sync**
Beware, sync will change your **local**
 - list files: `rclone lsd box`
 - to copy but sync as you go: `rclone sync -P LOCALFILE box:remotedirectory/`

**Delete**

Only change remote copy, not your local copy
 - `rclone delete box:genbank`
 - `rclone purge box:ncbi`

**Copying a symlink**

 - If you use `-L` you can copy the data associated with a symlink
 - If you use `-l` or `-links` it will copy the rclone link to file as a text file


### Examples

*Use a screen*: to kill a broken/hung screen: `screen -X -S 26624.pts-0.c11-42 quit`

 - For Symlinked file:
   - `rclone copy -LP file box:RADSEQ/SEQS/SOMM163b`

 - For all Files in current dir:
   - `rclone copy -P . box:RADSEQ/SEQS/SOMM163b`

 - For using wildcards:
   - `rclone copy -PL . --include "SOMM163*.sortflt.mrg.bam" gdrive:SEQS_backup/MERGED --dry-run`
