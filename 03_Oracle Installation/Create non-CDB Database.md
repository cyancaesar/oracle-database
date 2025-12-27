Check the environment variables exists:

```sh
echo $ORACLE_BASE
echo $ORACLE_HOME
echo $ORACLE_SID
```

Check if no listener exists:

```sh
lsnrctl status
```

Create a listener if no listener exists:

```sh
netca -silent -responsefile /u01/app/oracle/product/19.0.0/db_1/assistants/netca/netca.rsp
```

Run the Database Configuration Assistant:

```sh
dbca
```

![[Create non-CDB Database.png]]
![[Create non-CDB Database-1.png]]![[Create non-CDB Database-2.png]]
![[Create non-CDB Database-3.png]]
![[Create non-CDB Database-4.png]]![[Create non-CDB Database-5.png]]
![[Create non-CDB Database-6.png]]
![[Create non-CDB Database-7.png]]
![[Create non-CDB Database-8.png]]
![[Create non-CDB Database-9.png]]
![[Create non-CDB Database-10.png]]
![[Create non-CDB Database-11.png]]![[Create non-CDB Database-12.png]]
![[Create non-CDB Database-13.png]]
![[Create non-CDB Database-14.png]]
![[Create non-CDB Database-15.png]]![[Create non-CDB Database-16.png]]

Save the response file (the database options are not saved).

Run the dbca in a silent mode:

```sh
dbca -createDatabase -silent -responseFile dbca.rsp -dbOptions JSERVER:true,DV:false,APEX:false,OMS:false,SPATIAL:false,IMEDIA:false,ORACLE_TEXT:false,CWMLITE:false -sampleSchema true
```

Verify that Database process monitor is running:

```sh
ps -ef | grep pmon
```

Verify with SQL\*PLUS:

```sh
sqlplus / as sysdba
```

Verify the installed components:

```sql
SET LINESIZE 180;
COL COMP_NAME FOR a40;
COL STATUS FOR a15;
COL VERSION FOR a10;
SELECT COMP_NAME, STATUS, VERSION FROM DBA_REGISTRY ORDER BY 1;
```

Verify the HR schema is created:

```sql
SELECT USERNAME FROM DBA_USERS WHERE USERNAME = 'HR';
```

Create the HR schema:

```sql
@ $ORACLE_HOME/demo/schema/human_resources/hr_main_new.sql
```

View the content of `tnsnames.ora`:

```sh
cat $ORACLE_HOME/network/admin/tnsnames.ora
```

Enterprise Manager Express is up and listen on port 5500.

## Auto Startup

When Oracle Restart is not configured, the database does not automatically started when the machine boots up.

```sh
vim /etc/oratab
# Change from N to Y
# oradb:/u01/app/oracle/product/19.0.0/db_1:Y
```

Create a systemd service unit to enable automatic dbstart and dbshut.