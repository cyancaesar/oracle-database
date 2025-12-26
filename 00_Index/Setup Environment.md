Download Oracle Linux 8.10 ISO from Oracle public yum repository
https://yum.oracle.com/oracle-linux-isos.html

We need this version because the database version that will be used is Oracle Database 19c. Other latest version of Linux supports it, but require Release Units (RU) patches.

Use your favorite hypervisor, mine is VMware, and setup a base VM to be used for cloning.

Update the package list:

```
dnf update -y
```

Enable root login to SSH (this is for a test environment):

```
echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
```

Now make a full clone of this VM.

SSH into the newly cloned VM and set a hostname:

```
hostnamectl set-hostname oradb01.vm
```

Add an entry on `/etc/hosts`:

```
# its a NAT network
46.101.184.162  oradb01 oradb01.vm
```

