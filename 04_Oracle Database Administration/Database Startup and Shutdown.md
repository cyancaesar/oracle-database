## Oracle Database Instance Open Modes

| State    | Description                                                                               | Command                      | Next Phase                        |
| -------- | ----------------------------------------------------------------------------------------- | ---------------------------- | --------------------------------- |
| SHUTDOWN | Instance is down and none of its files is opened                                          |                              |                                   |
| NOMOUNT  | Instance and background processes started.<br>No Control Files or datafiles are opened    | `STARTUP NOMOUNT`            | `ALTER DATABASE MOUNT`            |
| MOUNT    | NOMOUNT and only Control Files are opened                                                 | `STARTUP MOUNT`              | `ALTER DATABASE OPEN [READ ONLY]` |
| OPEN     | Datafiles and Redo Log files are opened.<br>Users can connect to the database (RO or RW). | `STARTUP [OPEN] [READ ONLY]` |                                   |


> [!NOTE] More options
> There are other options for startup like `RESTRICT`, `FORCE`, `OPEN RECOVER`

## Restrict Instance Startup

Starting up the database normally, but without allowing general database users to log in.

```sql
STARTUP RESTRICT
```

Only DBA connects to the database, so no remote connections are allowed. It its useful for troubleshooting situations.

To allow normal users to connect:

```sql
ALTER SYSTEM DISABLE RESTRICTED SESSION;
```

## Shutdown Modes

```
SHUTDOWN [<shutdown mode>]
```

| Mode | Description                              |
| ---- | ---------------------------------------- |
| N    | NORMAL                                   |
| T    | TRANSACTIONAL                            |
| I    | IMMEDIATE                                |
| A    | ABORT (abnormal shutdown, unclean state) |

| Shutdown Mode                        | N   | T   | I   | A   |
| ------------------------------------ | --- | --- | --- | --- |
| Allows new connections               | ❌   | ❌   | ❌   | ❌   |
| Waits until current sessions end     | ✅   | ❌   | ❌   | ❌   |
| Waits until current transactions end | ✅   | ✅   | ❌   | ❌   |
| Forces a checkpoint and closes files | ✅   | ✅   | ✅   | ❌   |

> [!NOTE] ABORT!
> It terminates everything, every transactions, and closes the datafiles without marking a checkpoint.
> The next time the database instance starts up, it must perform Automatic Instance Recovery procedure
> 
> 


## More Notes on Startup and Shutdown

- The state of a database instance are performed by connecting as `SYSOPER`, `SYSDBA`, `SYSBACKUP`, or `SYSDG`.
- When the database instance is registered in Clusterwave (Grid Infrastructure), `srvctl` utility should be used to startup and shutdown the database instances.
- `STARTUP FORCE` is a way to restart the database in one command
	- `SHUTDOWN ABORT + STARTUP`
	- Should be used when the clean shutdown is not possible

## Best Practices for Shutting Down Databases

- Any system should eventually shutdown (planned or unplanned)
- Adhere to any documented approved procedure implemented in the work environment
- If you need to shutdown a database for a scheduled maintenance, all users should be notified in advance
- If a user or more are still connected, consult the manager

