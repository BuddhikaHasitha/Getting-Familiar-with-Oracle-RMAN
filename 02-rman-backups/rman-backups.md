## RMAN Backups

This section covers common RMAN backup operations used in real DBA work.

### 1) Open RMAN and Verify Target
```
rman target /
```
Inside RMAN:
```
SHOW ALL;
```

### 2)Basic Full Database Backup (Backupset)

```
BACKUP DATABASE;
```

Backup Database + Archivelogs (common best practice)
```
BACKUP DATABASE PLUS ARCHIVELOG;
```

### 3) Backup Controlfile and SPFILE

Control file and SPFILE are critical for restore scenarios.
```
BACKUP CURRENT CONTROLFILE;
BACKUP SPFILE;
```

Enable Controlfile Autobackup (recommended)
```
CONFIGURE CONTROLFILE AUTOBACKUP ON;
```

### 4) Incremental Backups (Level 0 and Level 1)

Level 0 (acts like a baseline full backup)
```
BACKUP INCREMENTAL LEVEL 0 DATABASE;
```

Level 1 (captures changes since last level 0/1)
```
BACKUP INCREMENTAL LEVEL 1 DATABASE;
```

### 5) Backup With Format (store in custom directory)

```
BACKUP AS BACKUPSET DATABASE
  FORMAT '/u01/backups/rman/DB_%d_%T_%U.bkp';
```

### 6) Validate Backups (Check if restore is possible)

Validate database files (without actually restoring)
```
RESTORE DATABASE VALIDATE;
```

Validate existing backupsets

```
VALIDATE BACKUPSET <backupset_key>;
```
To list backupsets:
```
LIST BACKUP;
```

### 7) List and Report Useful Information

```
LIST BACKUP SUMMARY;
LIST BACKUP OF DATABASE;
LIST BACKUP OF ARCHIVELOG ALL;
```
Report obsolete backups (based on retention policy)
```
REPORT OBSOLETE;
```

### 8) Recommended Baseline RMAN Config (Learning Setup)

```
CONFIGURE RETENTION POLICY TO RECOVERY WINDOW OF 7 DAYS;
CONFIGURE CONTROLFILE AUTOBACKUP ON;
CONFIGURE DEVICE TYPE DISK PARALLELISM 1 BACKUP TYPE TO BACKUPSET;
```

Adjust retention based on storage and recovery requirements.
