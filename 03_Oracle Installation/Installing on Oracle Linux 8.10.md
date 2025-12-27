Add  these entries to `/home/oracle/.bash_profile`

```sh
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/19.0.0/db_1
export ORALCE_SID=oradb
export NLS_DATE_FORMAT="DD-MON-YYYY HH24:MI:SS"
export PATH=$PATH:$ORACLE_HOME/bin
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$ORACLE_HOME/oracm/lib:/lib/:/usr/lib:/usr/local/lib
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib:$ORACLE_HOME/network/jlib
export TEMP=/tmp
export TMPDIR=/tmp

# This is used to fake the installer to assume we are at Oracle Linux 7.6
export CV_ASSUME_DISTID=OEL7.6
umask 022
```

Disable SELinux:

```sh
vim /etc/selinux/config
# Change to "SELINUX=permissive"

setenforce 0 # or reboot the system
```

Download Oracle Database 19c for Linux, its a ZIP file with the name `LINUX.X64_193000_db_home.zip`.

Create the following directories:

```sh
mkdir -p /u01/app/oracle/product/19.0.0/db_1
mkdir -p /u01/app/oraInventory
chown -R oracle:oinstall /u01
```

Extract the ZIP file to `ORACLE_HOME`:

```sh
unzip /whereever/it/is/LINUX.X64_193000_db_home.zip -d $ORACLE_HOME
```

Run the Universal Installer:

```sh
bash $ORACLE_HOME/runInstaller
```

![[Installing on Oracle Linux 8.10.png]]

![[Installing on Oracle Linux 8.10-1.png]]

![[Installing on Oracle Linux 8.10-2.png]]

![[Installing on Oracle Linux 8.10-3.png]]

![[Installing on Oracle Linux 8.10-4.png]]

![[Installing on Oracle Linux 8.10-5.png]]

![[Installing on Oracle Linux 8.10-6.png]]

![[Installing on Oracle Linux 8.10-7.png]]


> [!NOTE] Don't Hit Install
> Save the response file and cancel the operation. To install the database software as a silent mode.

Run again the installer in a silent mode:

```sh
$ORACLE_HOME/runInstaller -silent -responseFile /home/oracle/orcl.rsp
```

If the installation succeed, it will show `Successfully Setup Software.`


> [!NOTE] Release Updates (RU)
> In a production setup, it is recommended to apply Release Updates (RU) after the database software installations.

