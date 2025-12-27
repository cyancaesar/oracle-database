- Download Oracle Database 19c from Oracle website:
	https://www.oracle.com/database/technologies/
- Or from Software Delivery:
	https://edelivery.oracle.com

## Oracle Database software installation methods for Linux
- Image-based
	- DBA downloads the database software and runs the setup wizard
	- DBA has more control on the options
- RPM-based
	- DBA downloads the database software RPM and installs it
	- Quick installation with less options

## Oracle Database installation procedure (High Level)

1. Setup the server machine to meet the requirements
2. Prepare the installation target machine
3. Download the required software
	1. Download Oracle Database software
	2. Some configuration downloads the _Grid Infrastructure_ software
4. (optional): Download the latest available Release Update (RU), Release Update Revision (RUR), or Patchset from Oracle Support
5. Install Oracle Database software and Grid if needed
6. (optional): Apply the patches
7. Create the database

## Requirements for Linux

| Item                                  | Required Values                                                                                                                                                                                                                                 |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RAM                                   | ~6GB for Oracle Database<br>~8GB for Grid Infras                                                                                                                                                                                                |
| Free disk space in the /tmp           | 1GB                                                                                                                                                                                                                                             |
| Swap space allocation relative to RAM | For Oracle Database:<br>1GB < RAM < 2GB: swap should be at least 1GB<br>2GB < RAM < 16GB: swap should be the same size<br>16GB < RAM: 16GB<br><br>For Oracle Restart:<br>8GB < RAM < 16GB: swap should be the same size<br>16GB < RAM: 16GB<br> |

## Prepare the Database Server Machine

- Install required missing libraries
- Perform some configurations
- Create OS users and groups

For installing the required libraries:

```sh
# Do not use this on engineered system "Exadata"
dnf install oracle-database-preinstall-19c
```

It configures kernel parameters for Linux on:

```sh
/etc/sysctl.d/99-oracle-database-preinstall-19c-sysctl.conf
```

For manually configure kernel parameters:

```sh
# /etc/sysctl.d/97-oracle-database-sysctl.conf

fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 4294967295
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmeme_max = 1048576

# /sbin/sysctl --system
```

Required resource limits are set by the preinstall in:

```
/etc/security/limits.d/oracle-database-preinstall-19c.conf
```

Disabling Transparent HugePages:

1. Check if the Transparent HugePage is enabled:
	`/sys/kernel/mm/transparent_hugepage/enabled`
2. Add the `transparent_hugepage` parameter to `GRUB_CMDLINE_LINUX` in `/etc/default/grub`
	`GRUB_CMDLINE_LINUX="crashkernel=auto rhgb quiet transparent_hugepage=never"`
3. Run the command to compile the newly GRUB config
	`grub2-mkconfig -o /boot/grub2/grub.cfg`

Configuring Users, Groups:

1. Create the `OSDBA` groups
	- `groupadd -g 54321 oinstall`
	- `groupadd -g 54322 dba`
	- `groupadd -g 54323 oper`
2. Create Oracle database software user:
	`useradd -u 54321 -g oinstall -G dba,oper oracle`

Configure Environment Variables in `.bash_profile`:

```bash
export ORACLE_SID=ORADB
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/19.0.0/db_1
export PATH=$PATH:$ORACLE_HOME/bin
umask 022
```

- `ORACLE_BASE`: the root directory of the Oracle Database directory tree
- `ORACLE_HOME`: the directory where Oracle Database software will be installed
- `ORACLE_SID`: Oracle Database instance name

---

## Universal Installer Modes

| Installation Mode | Description                                                                                                                                                                                                                                                 |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Interactive       | GUI mode                                                                                                                                                                                                                                                    |
| Response File     | When some of the required information is provided by the response file and the response file is provided to the installer                                                                                                                                   |
| Silent            | No interactive GUI<br>A response file is provided to provide information for the installer:<br>- Installing Oracle Database software with no X Windows system<br>- Unattended installations<br>- Easy installation on multiple machines<br>`-silent` option |

## Oracle Support

From Oracle support you can:
- Obtain RU, RUR, Patches and Patchsets
- Review knowledge base
- Submit Service Requests (SR)

## Oracle Inventory

Oracle Inventory is a list of Oracle software products installed in the system. In Linux, it is on `/etc/oraInst.loc`:

```sh
inventory_loc=/u01/app/oraInventory
inst_group=oinstall
```

## Uninstall Oracle Database Software

1. Stop all the services running from Oracle Database software home
2. Run `$ORACLE_HOME/deinstall/deinstall`

