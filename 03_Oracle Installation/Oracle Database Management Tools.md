## SQL\*Plus

- Command-line tool, shipped with Oracle Database
- Formatting commands
- SQLcl is better

Start with no logging in:

```sh
sqlplus /nolog
SQL> connect user/password
```

With logging in to the local database:

```sh
sqlplus user/password
```

As `SYS` user:

```sh
# If the logged on OS user is a member of dba group (OS Authentication)
sqlplus / as sysdba

# If not
sqlplus sys/password as sysdba
```

Start SQLPlus to run scripts:

```sh
cat list-employees.sql

sqlplus user/pass @list-employees
```

You could run the script from the prompt:

```sh
SQL> @list-employees
SQL> start list-employees.sql
```

## SQL Developer

A Java-based GUI interface.

## SQLcl

A Java-based CLI, shipped with SQL Developer.

## EM Express

A web-based GUI interface to monitor local databases.

It needs the Diagnostics Pack and Tuning Pack licenses.

## Oracle Enterprise Manager Cloud Control (OEM)

- System management software for monitoring, administration, and management
- Agent must be installed on the host
- Needs a license

