# GPG

The MariaDB project signs their MariaDB packages for Debian, Ubuntu, Fedora, CentOS, and Red Hat.

## New key

Our repositories for Debian "Sid" and the Ubuntu 16.04 and beyond "Xenial" use a new GPG signing key. As detailed in [MDEV-9781](https://jira.mariadb.org/browse/MDEV-9781), APT 1.2.7 (and later) prefers SHA2 GPG keys and now prints warnings when a repository is signed using a SHA1 key like our previous GPG key. We have created a new SHA2 key for use with these affected repositories.

Information about this key:

- The short Key ID is: `0xC74CD1D8`
- The long Key ID is: `0xF1656F24C74CD1D8`
- The full fingerprint of the key is: `177F 4010 FE56 CA33 3630 0305 F165 6F24 C74C D1D8`
- The key can be added on Debian-based systems using the following command:

```sql
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
```

The instructions in the repository configuration tool for Ubuntu 16.04 "Xenial" and Debian "Stretch" and higher have been updated to reference this new key. Repositories for previous versions of Debian and Ubuntu still use the old key, so no changes are needed there.

## Old key

The GPG Key ID of the MariaDB signing key is `0xCBCB082A1BB943DB`. The short form of the id is `0x1BB943DB` and the full key fingerprint is:

```sql
1993 69E5 404B D5FC 7D2F E43B CBCB 082A 1BB9 43DB
```

This key is still used by the yum/dnf/zypper repositories for RedHat, CentOS, Fedora, openSUSE, and SLES.

If you configure the mariadb.org rpm repositories using the repository configuration tool (see below) then your package manager will prompt you to import the key the first time you install a package from the repository.

You can also import the key directly using the following command:

```sql
sudo rpmkeys --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
```

## Configuring

See the [repository configuration tool](https://downloads.mariadb.org/mariadb/repositories/) for details on configuring repositories that use these keys.