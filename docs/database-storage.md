# How Databases Store Data (PostgreSQL)

Physically, a database is stored on disk as directories and files:


Server disk  
└── Database folder  
      └── Table files  
            └── Pages (blocks)  
                  └── Rows  


Memory (RAM) is used only as a cache.
The real source of truth is disk.

 ## Tables
 Tables are stored as files split into fixed-sized pages (8KB in PostgresSql).
 Rows are places inside pages wherever there is free space.


When querying:
- if page is in RAM -> fast
- if page is in Disk -> slower


## Writes

On INSERT/UPDATE:

- Data is written to WAL (Write-Ahead Log)
- Pages are flushed to disk later
- This guarantees durability (ACID)

## Database storage structure

## Database storage structure

```text
/var/lib/postgresql/data/
 ├── base/
 │    ├── 16384/        ← Database #1 (one directory per DB)
 │    │     ├── 24576   ← table file (routes)
 │    │     ├── 24577   ← index file (routes_driver_id_idx)
 │    │     ├── 24578   ← another table
 │    │     └── ...
 │    ├── 16385/        ← Database #2
 │    └── ...
 ├── global/           ← system catalogs
 ├── pg_wal/           ← write-ahead logs (WAL)
 ├── pg_xact/          ← transaction status
 ├── pg_multixact/
 ├── pg_tblspc/        ← tablespaces

Table file on disk
 ├── Page 1 (8KB)
 │     ├── row 1
 │     ├── row 2
 │     └── ...
 ├── Page 2 (8KB)
 │     ├── row 101
 │     ├── row 102
 │     └── ...
 ├── Page 3
 └── ...

```
## Summary

Tables live on disk.  
Memory caches pages.  
Indexes help find rows faster.
