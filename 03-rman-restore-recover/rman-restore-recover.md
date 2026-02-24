## RMAN Restore & Recovery

This section demonstrates restore and recovery workflows.  
(Use these steps only in a controlled lab environment.)

### Key Terms
- **RESTORE**: brings back missing/corrupted datafiles from backup
- **RECOVER**: applies redo/archivelogs to make data consistent

---

### 1) Check Database State (SQL)
```
SELECT STATUS FROM V$INSTANCE;
SELECT NAME, OPEN_MODE FROM V$DATABASE;
```

### 2) Restore Controlfile (Common Failure Scenario)

If controlfile is lost/corrupt, DB may not mount.

Steps (generic flow)

Start instance to NOMOUNT
```
STARTUP NOMOUNT;
```

Restore controlfile (if autobackup enabled)
```
RESTORE CONTROLFILE FROM AUTOBACKUP;
```

Mount database
```
ALTER DATABASE MOUNT;
```

### 3) Restore Database
```
RESTORE DATABASE;
```

### 4) Recover Database

```
RECOVER DATABASE;
```
After successful recovery:
```
ALTER DATABASE OPEN;
```
If you restored an older controlfile, you may need:
```
ALTER DATABASE OPEN RESETLOGS;
```

### 5) Restore and Recover a Single Datafile (Targeted Recovery)
Identify datafile
```
SELECT FILE#, NAME FROM V$DATAFILE;
```

Restore + recover one datafile
```
RESTORE DATAFILE 5;
RECOVER DATAFILE 5;
```

### 6) Restore Validation (Safe Practice Before Real Restore)

```
RESTORE DATABASE VALIDATE;
```

### 7) Useful Commands During Recovery
See what RMAN thinks exists
```
CROSSCHECK BACKUP;
CROSSCHECK ARCHIVELOG ALL;
```

If backups/logs are missing physically
```
DELETE EXPIRED BACKUP;
DELETE EXPIRED ARCHIVELOG ALL;
```
