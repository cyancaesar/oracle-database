## Oracle Database Instance Server Architecture

When the database is up and running, two major groups of components:
- Database Instance (in a memory)
- Database (in a storage)

Database Instance is a group of memory areas and processes. Each memory area is used to saves a specific data and each process is responsible for specific functionality.

Database (at the storage level) has two types of files:
- Data files (database data)
- System files (database operation)

An external client wants to connect to the database, it must communicate with an external process called **Listener** that is not part of the database process.

The listener thus initiates a connection between the client and the database, once the connection success, Oracle database spawns a new process called **Server Process** (runs inside the database instance).

![[Oracle Database Architecture.png]]

The memory part inside the database instances are divided into to areas:
- System Global Area (SGAs), shared memory area
- Program Global Area (PGAs), private session memory area

The PGA is dedicated to the client and has private area, the SGA is dedicated to the database and is divided into a few memory areas.

Each client session information is saved into the PGA, server processes can read from the SGA or write into it.

## System Global Area Primary Components

![[Oracle Database Architecture-1.png]]

Shared Pool:
- Save SQL statements and PL/SQL code executed by the client in the sub area called Library Cache
- Data Dictionary Cache holds information about the accessed database objects, it contains data _about_ the object not _inside_ (metadata)
- Server Result Cache holds the result sets, SQL query result cache and PL/SQL function result cache

Database Buffer Cache:
- Save copies of data blocks
- Client submit queries to the database to retrieve data, by default the data blocks that contain the required data are not read from the data files in the storage straight to the client process. They are saved in the database buffer cache first to optimize the I/O operation
- If later the same data is required, it read from the buffer cache

Large Pool:
- Used for buffers during backup and recovery operations
- Buffers for different inserts, message buffer for parallel executions

Redo Log Buffer:
- Used to save information about the changes made on the data by DML and DDL statements
- Internally , the changes made to the data is saved in records called redo entries
- Redo entries have the information to reconstruct or to redo the changes made to the data

Buffer (Data Blocks) States:
- Data block is the smallest storage unit
- A buffer can be in any of the following mutually exclusive states:
	- Unused: the buffer not been used or accessed
	- Clean: the buffer was used earlier and now contains a read-consistent version of a block as of a point in time
	- Dirty: the buffer contain modified data that has not yet been written to disk


## Background Processes

- ARCn
- CJQ0
- RVWR
- FBDA
- PMON
- SMON
- MMON
- CKPT
- DBWn
- LREG
- LGWR
- RECO
- MMNL


### Database Writer Process (DBWn)
- Writes the modified (dirty) blocks from the *database buffer cache* to the data files in the storage.
- Its a lazy writer
- Single writer is enough, but for high stress databases, you may need to configure more than one DB Writer process

### Log Writer Process (LGWR)
- Responsible for writing the redo entries from the Redo Log Buffer to the Redo Log Files (online)

### Checkpoint Process (CKPT)
- Performs two actions
- Updates the control files and the data file headers with checkpoint information
- To signals the DB Writer to write the modified blocks to the disk

Checkpoint information is the information that tells the database engine up to what time the data changes have been saved in the storage. 

If a database instance has crashed, and has started up, the database engine wants to know which data was saved in the data files and which modified data is missing because it was in the memory at the time of the instance crash. From checkpoint information the database engine knows that.

The checkpoint is saved in the control file the the header of data files.

### System Monitor Process (SMON)
- Instance recovery, recovering terminated transactions

### Listener Registration Process (LREG)
- Register the database instance and dispatcher processes in the Listener

### PMON group
- PMON, CLMN, CLnn
