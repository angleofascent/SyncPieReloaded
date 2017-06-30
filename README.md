# SyncPieReloaded

My hobby project in python.

A self hosted dropbox-like Sync/backup tool to be used with .


##Plan

File database entry has:
1) Full relative filename
2) List of SHA + modified date (history)

Upload operation = file + database entry + last modified date must be set. before pulling from server check recycle bin (moved files) 
Delete = file moved to recycle bin and database entry marked as deleted
Rename = Delete + download


At startup:
Index all files:
check last modified date against local db.
- if same as in db, do nothing.
- If file new (no entry in db) hash and add to database
- If file existing bat changed modified date, rehash and add entry to db.

Synchronization (only after indexing)
Pull server database table (current + deleted).
Compare it with local (indexed)
If new files in local database, upload with entry
If modified files in local db, upload with entry
If not existing files in local db, download entry and check recycle bin for the file and move to correct place. Otherwise download.
If files diverge (sync conflict) rename local (as conflict + machine name) and download server version.


After sync, use file hook to detect file change or poll.


Data transport layer: Needs to be interchangeble (abstraction)
Pulling file and entry at the same time.
