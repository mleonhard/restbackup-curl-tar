RestBackup(tm) Incremental Backup Tool
======================================

Restbackup-curl-tar is a command-line tool for performing incremental backups to
RestBackup(tm) and restoring from any point in time.  Requires curl and tar.
Get your backup account at [www.restbackup.com](http://www.restbackup.com/)

The
[restbackup-python](https://github.com/mleonhard/restbackup-python)
package includes an improved version of this tool called
restbackup-tar, which can do encryption.

Usage
-----

    Usage: restbackup-curl-tar [OPTIONS] COMMAND [args]
    
    Commands:
     --full FILE1 FILE2 ...        Perform a full backup
     --incremental FILE1 ...       Perform an incremental backup
     --list                        List backup archives
     --restore ARCHIVE [FILE1 ...] Restore files from archive
     --help                        Show this message
     --example                     Show example usage
    
    Options:
     -u ACCESS_URL       a RestBackup(tm) backup account access url such as
                         https://Z2J3BB:R0GaTKS0vM3l3FgY@us.restbackup.com/
     -b ACCESS_URL_FILE  file with backup account access url, default
                         ~/.restbackup-backup-api-access-url
     -n NAME             name for the set of backups, eg. leonhard-email, git-repos.
                         Default: backup
     -s SNAPSHOT_FILE    Tar incremental snapshot file, default
                         ~/.restbackup-curl-tar/NAME.snapshot

Example Usage
-------------

Setup:

     $ echo https://WPJXX3:INzIsdEE77vZgih7@us.restbackup.com/ >~/.restbackup-backup-api-access-url
     $ chmod 600 ~/.restbackup-backup-api-access-url
     $ mkdir data
     $ echo "initial data" >data/file1
    
Full Backup:

     $ restbackup-curl-tar -n data --full data/
     Performing full backup to 'data-20110620T153328Z-full.tar.gz'
     Writing archive to temporary file /tmp/fileDBJwoW.restbackup-curl-tar.tar.gz
     Uploading to https://us.restbackup.com/data-20110620T153328Z-full.tar.gz
     Removing temporary file
     Done.
    
Incremental Backups:

     $ echo "new data" >data/file2
     $ restbackup-curl-tar -n data --incremental data/
     Performing incremental backup to 'data-20110620T153328Z-inc1.tar.gz'
     Writing archive to temporary file /tmp/filelUxTkH.restbackup-curl-tar.tar.gz
     Uploading to https://us.restbackup.com/data-20110620T153328Z-inc1.tar.gz
     Removing temporary file
     Done.
     $ echo "a modification" >>data/file1
     $ echo "more new data" >data/file3
     $ restbackup-curl-tar -n data --incremental data/
     ...
    
Restore:

     $ restbackup-curl-tar --list
     2011-06-20T15:33:29Z   183     /data-20110620T153328Z-full.tar.gz
     2011-06-20T15:34:00Z   187     /data-20110620T153328Z-inc1.tar.gz
     2011-06-20T15:34:31Z   241     /data-20110620T153328Z-inc2.tar.gz
     $ restbackup-curl-tar --restore /data-20110620T153328Z
     Restoring to data-20110620T153328Z/
     Retrieving https://us.restbackup.com/data-20110620T153328Z-full.tar.gz
     data/
     data/file1
     Retrieving https://us.restbackup.com/data-20110620T153328Z-inc1.tar.gz
     data/
     data/file2
     Retrieving https://us.restbackup.com/data-20110620T153328Z-inc2.tar.gz
     data/
     data/file1
     data/file3
     Done.
     $ ls data-20110620T153328Z/data/
     file1  file2  file3
     $ cat data-20110620T153328Z/data/file1
     initial data
     a modification
