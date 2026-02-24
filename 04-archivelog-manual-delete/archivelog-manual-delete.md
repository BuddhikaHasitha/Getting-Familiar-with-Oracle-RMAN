## RMAN Archivelog Manual Delete

Archived redo logs are essential for recovery and (if applicable) for Data Guard log apply.  
Deleting them incorrectly can break recovery or standby synchronization.

---

### 1) List Archivelogs Known to RMAN
```
LIST ARCHIVELOG ALL;
```

### 2) Crosscheck (Sync RMAN Catalog with Disk)
```
CROSSCHECK ARCHIVELOG ALL;
```

If RMAN marks some logs as missing:
```
DELETE EXPIRED ARCHIVELOG ALL;
```

### 3) Delete Archivelogs Older Than N Days (Manual Cleanup)

Example: delete logs older than 7 days:
```
DELETE ARCHIVELOG ALL COMPLETED BEFORE 'SYSDATE-7';
```

### 4) Delete Archivelogs Only If Backed Up (Safer Practice)

```
DELETE ARCHIVELOG ALL BACKED UP 1 TIMES TO DISK;
```
This ensures at least one disk backup exists before deletion.

### 5) Delete Obsolete (Retention Policy Driven)
```
REPORT OBSOLETE;
DELETE OBSOLETE;
```

### 6) Important Note for Data Guard

If a standby database exists, DO NOT delete archivelogs that are still required for standby apply.

In Data Guard setups, use an archivelog deletion policy (covered in the Data Guard RMAN config section).
