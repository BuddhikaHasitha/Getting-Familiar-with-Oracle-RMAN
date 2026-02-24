## Environment Details

This document describes the database environment used for all RMAN backup and recovery examples in this repository.

### Database Architecture Overview
- **Database Version**: Oracle Database 23ai
- **Architecture Type**: Multitenant (CDB / PDB)
- **Container Database**: CDB
- **Pluggable Database Used**: FREEPDB1 (example PDB)

> All RMAN examples are executed at the database level.  
> In multitenant setups, RMAN typically works at CDB scope, while application objects exist inside PDBs.

---

### Required Access
You typically need one of these:
- **SYSDBA** (common for RMAN tasks)
- OR a user granted the required RMAN/backup privileges (depends on enterprise policy)

Connect examples:
```
rman target /
```

OR

```
rman target sys/<password>@<service> as sysdba
```

### Verify Multitenant Context (SQL)

```
SHOW CON_NAME;
SELECT NAME, OPEN_MODE FROM V$PDBS;
```

### Archivelog Mode & FRA (Recommended for RMAN Practice)
Check Archive Log Mode

```
ARCHIVE LOG LIST;

SHOW PARAMETER db_recovery_file_dest;
SHOW PARAMETER db_recovery_file_dest_size;
```

### Common Locations and Outputs

RMAN backups can be stored in:

FRA (Fast Recovery Area)
Custom filesystem paths (using FORMAT)
Tape/SBT (enterprise environments)

This repository focuses mainly on filesystem/FRA-based backups for learning clarity.
