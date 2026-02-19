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


## Summary

Tables live on disk.  
Memory caches pages.  
Indexes help find rows faster.
