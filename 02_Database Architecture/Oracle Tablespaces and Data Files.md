- Tablespace is a logical group of component
- Tablespaces consist of one or more data files
- Data file belongs to only one tablespace

## Segments, Extents, and Blocks

- Segments exist within a one tablespace 
- Segments are made up of collection of extents
- Extent is a collection of logically contiguous data blocks (cannot span multiple data files)
- Data blocks (8kb) are mapped to disk blocks

Segments are any data objects that contain rows:
- Tables
- Indexes
- Partitions
- Indexed organized tables
- Materialized views
- Clusters

## SYSTEM and SYSAUX Tablespaces

- The `SYSTEM` and `SYSAUX` tablespaces are mandatory tablespaces
- They are created at the time of database creation
- They must be online
- The `SYSTEM` tablespace is used for core functionality (data dictionary tables)
- The auxiliary tablespace `SYSAUX` is used for additional database components

