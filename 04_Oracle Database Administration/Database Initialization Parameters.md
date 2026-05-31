Initialization parameters are a key-value pairs that controls the behavior of the database.

Initialization parameters are saved in parameter files, and when the database instance starts up, it reads it from the parameter file and load it into Instance Parameters.

Some parameters can be changed from the memory and it is called **Dynamic (Modifiable) Parameters**. Some parameters can only be changed from the parameter files and they are called **Static Parameters**.

Initialization parameters file types:
- Server parameter file (SPFILE)
	- Binary format, its content is changed by `ALTER SYSTEM` SQL statements
- Text initialization parameter file (PFILE)
	- Text format, modified by a editor

When the database starts up, it reads either **SPFILE** or **PFILE**.

## Initialization Parameter File Search Flow

1. File specified by the pfile (spfile) option passed with the startup command
2. A spfile with the name `spfile$ORACLE_SID.ora` in `$ORACLE_HOME/dbs`
3. A spfile with the name `spfile.ora`
4. A pfile with the name `init$ORACLE_SID.ora`

Some initialization parameters:


| Parameter       | Description                                         |
| --------------- | --------------------------------------------------- |
| `CONTROL_FILES` | Full path of database instance control file(s)      |
| `PROCESSES`     | Maximum number of OS user processes                 |
| `DB_BLOCK_SIZE` | Standard database block size used by all tablespace |
| `SGA_TARGET`    | Specifies the total size of all SGA components      |
| `MEMORY_TARGET` | Specifies the Oracle systemwide usable memory       |

View these parameter on SQLPLUS:

```sql
SHOW PARAMETER SGA_TARGET;
SHOW PARAMETER SGA;

SELECT * FROM V$PARAMETER WHERE NAME = 'sga_target';
```

To query the parameter for the SPFILE:

```sql
SELECT * FROM V$SPPARAMETER WHERE NAME = 'sga_target';
```

To know which SPFILE/PFILE is used:

```sql
SHOW PARAMETER SPFILE;
```

## Changing Initialization Parameter Value for SPFILE

- Static parameters
	- Changed only in the parameter file
	- Require a restart
	- `ALTER SYSTEM SET <PAR>=<VALUE> SCOPE=SPFILE;`
- Dynamic parameters
	- Changed while database is online
	- Can be altered at the system level and some session level
	- `ALTER SYSTEM SET <PAR>=<VALUE> SCOPE=BOTH;`
	- `ALTER SYSTEM SET <PAR>=<VALUE> SCOPE=MEMORY;`
	- `ALTER SESSION SET <PAR>=<VALUE>;`

Query the parameter in the memory: `V$PARAMETER`
Query the parameter in the SPFILE: `V$SPPARAMETER`