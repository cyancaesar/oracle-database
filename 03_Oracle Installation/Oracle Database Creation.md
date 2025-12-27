## Plan for Database Creation

You need to answer these questions:
- Capacity (number of users, storage)
- Database size: total size of all the segments
- Database name: *DB_NAME*
- Domain name: *DB_DOMAIN*
- Character set
- Block size: default 8K, 4K for OLTP, 16K for warehouses
- Multitenancy

## Oracle Database Creation Methods

- Database Configuration Assistant `dbca` (interactive or silent)
- Manually via scripts `CREATE DATABASE`

## Database Listener Component

If there is no listener already been configured, a new one can be created with the Network Configuration Assistant `netca` which provides interactive and silent mode.

```sh
netca -silent -responsefile /u01/app/oracle/product/19.0.0/db_1/assistants/netca/netca.rsp
```

## Database Creation in Silent Mode

Creating a database in a silent mode:

```sh
dbca -createDatabase -silent -responsefile dbca.rsp
```

> [!NOTE] Response File
> After running the `dbca` in interactive mode, you should save a response file.

If no response file is needed, you can create it with arguments:

```sh
dbca -createDatabase -silent -templateName .......
```

One thing to note is that the database options are not provided in the response file, hence it will install all the components. You can specify it in the command line:

```sh
dbca -createDatabase -silent -responsefile dbca.rsp \
-dbOptions JSERVER:true,DV:false,APEX:false,OMS:false,SPATIAL:false,IMEDIA:false,ORACLE_TEXT:false,CWMLITE:false
```

There is a tool called `chopt` to drop or install a database component after database creation.

## Command-line Parameters

| Command-line Parameter     | Description                                                                  |
| -------------------------- | ---------------------------------------------------------------------------- |
| createDatabase             | Creates a database                                                           |
| createDuplicateDB          | Creates a duplicate of database                                              |
| configureDatabase          | Configures a database                                                        |
| createTemplateFromDB       | Creates a database template from an existing database                        |
| createTemplateFromTemplate | Creates a database template from an existing database template               |
| createCloneTemplate        | Creates a clone database template from an existing database                  |
| deleteTemplate             | Deletes a database template                                                  |
| generateScripts            | Generates scripts                                                            |
| deleteDatabase             | Deletes a database                                                           |
| createPluggableDatabase    | Creates a pluggable database (PDB) in a multitenant container database (CDB) |
| unplugDatabase             | Unplug a pluggable database from CDB                                         |
| deletePluggableDatabase    | Deletes a PDB                                                                |
| relocatePDB                | Relocates a PDB from remote CDB to a local CDB                               |
| configurePluggableDatabase | Configures a PDB                                                             |
| addInstance                | Adds a database instance to a RAC database                                   |
| deleteInstance             | Deletes a database instance to a RAC database                                |
| executePrereqs             | Executes the prerequisites checks                                            |

## Database Deletion

```sh
dbca -silent -deleteDatabase -sourceDB ${ORACLE_SID} -sysDBAUserName sys -sysDBAPassword syspasswd

# Or by SQL .. think 100 steps ahead before doing this
DROP DATABASE;
```

## Database Creation Guideline

- For single database creation, dbca is enough
- For multiple database creations, dbca in silent mode
- When creating a database for a specific application, consult the vendor for the requirements
- Do not ignore the documentation and document everything
- Stick with the policies
