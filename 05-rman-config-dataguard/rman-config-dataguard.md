## RMAN Configuration in Data Guard Setup

This section documents common RMAN configuration patterns used when a Data Guard standby is present.

### Why RMAN Config Matters in Data Guard
- Standby needs archivelogs from primary to apply redo
- Incorrect deletion can cause apply gaps and manual log shipping
- RMAN policies help prevent deleting required logs

---

### 1) Recommended Core Config
```
CONFIGURE CONTROLFILE AUTOBACKUP ON;
CONFIGURE RETENTION POLICY TO RECOVERY WINDOW OF 7 DAYS;
```

### 2) Archivelog Deletion Policy (Critical for Data Guard)

A common safe option is:
```
CONFIGURE ARCHIVELOG DELETION POLICY TO APPLIED ON ALL STANDBY;
```
Meaning: primary can delete archivelogs only after they are applied on all standbys.

In some environments, policy may be SHIPPED TO ALL STANDBY depending on operations and apply strategy.

### 3) Backing Up on Primary vs Standby

Typical strategies:

Backup on Primary: simplest operationally, but uses primary resources

Backup on Standby: reduces load on primary, but requires careful coordination and connectivity

To confirm DB role:
```
SELECT DATABASE_ROLE FROM V$DATABASE;
```

### 4) Validate That Standby Apply Is Healthy (Basic Checks)

On standby:
```
SELECT PROCESS, STATUS, THREAD#, SEQUENCE#
FROM V$MANAGED_STANDBY;
```

Check archive gaps (standby):
```
SELECT * FROM V$ARCHIVE_GAP;
```

### 5) Guardrails (Practical)

Always define an archivelog deletion policy before doing aggressive cleanup

Prefer DELETE OBSOLETE instead of manual OS delete

Use CROSSCHECK regularly if manual file operations were done

Ensure controlfile autobackup is ON for faster recovery
