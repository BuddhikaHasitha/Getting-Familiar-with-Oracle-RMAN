## RMAN Archivelog Manual Delete

Archived redo logs are essential for recovery and (if applicable) for Data Guard log apply.  
Deleting them incorrectly can break recovery or standby synchronization.

---

### 1) List Archivelogs Known to RMAN
```rman
LIST ARCHIVELOG ALL;
```

