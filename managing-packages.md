# Managing Software Packages

A package name has 5 parts
1. name
2. version
3. release (revision or build)
4. EL version destination
5. proc arch

Ex:
wget-1.14-10.el7_0.1.x86_64  

name: wget  
version: 1.14  
release: 10  
EL version: el7  
proc arch: x86_64  

OBS: for platform independent proc arch is "noarch"

Package database: /var/lib/rpm (shows on man rpm "Database")

## RHSM - Red Hat Subscription Management Service  

* subscription-manager
  * subscription-manager register --auto-attach
  * subscription-manager remove --all

## rpm

* Does not check depency
* rpm --help show options
* rpm -qc - shows config files
* rpm -qd - shows docs
* rpm -qf <file> - shows pkg of files
* rpm -qi <pkg> - shows info about pkg
* rpm -ql <pkg> - list pkg files
* rpm -q --whatprovides /bin/ls
* rpm -q --whatrequires iproute
* rpm -ivh pkg
* rpm -Uvh pkg - upgrade and install if not installed
* rpm -Fvh \*.rpm - juts upgrade installed pkgs
* rpm -ev pkg - remove pkg

## rpm2cpio

* Extracts files from pkg
  * rpm2cpio pkg.rpm | cpio -ivd

## Checking pkg integrity

* rpm -K --nosignature pkg.rpm

## RHEL gpg keys

/etc/pki/rpm-gpg/

* Checking with pgp
  * rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
  * rpm -K pkg

```[root@host1 Packages]# pwd
/run/media/bravo/RHEL-7.2 Server.x86_64/Packages
[root@host1 Packages]# rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
[root@host1 Packages]# rpm -K zsh-5.0.2-14.el7.x86_64.rpm
zsh-5.0.2-14.el7.x86_64.rpm: rsa sha1 (md5) pgp md5 OK
[root@host1 Packages]#
```
* **OBS: I did not see the options "--import" or "-K" neither with --help nor with man rpm**
* rpm -K - seems to call rpmkeys
* rpm --import - seems to call rmpkeys --import

## Viewing GPG keys

```
[root@host1 Packages]# rpm -q gpg-pubkey
gpg-pubkey-fd431d51-4ae0493b
gpg-pubkey-2fa658e0-45700c69
[root@host1 Packages]# rpm -qi gpg-pubkey-fd431d51-4ae0493b
Name        : gpg-pubkey
Version     : fd431d51
Release     : 4ae0493b
Architecture: (none)
Install Date: Thu 28 Apr 2016 12:07:14 PM BRT
(...)
```

## Verifying packages

* rpm -V pkg
* rpm -v -V pkg
* rpm -vv -V pkg
* rpm -Vf file

```
# rpm -Vf README
# chown bravo:bravo README
# touch README
# chmod 600 README
[root@host1 at-3.1.13]# rpm -Vf README
SM5..UGT.  d /usr/share/doc/at-3.1.13/README
```

## yum

* **OBS: Knowing how to configure a repo using a URL is critical for the exam.**



* /etc/yum.conf
* /etc/yum.repos.d
* /etc/yum
* yum-config-manager
* yum --help
* man yum
* yum check-update
* yum clean
* yum group
  * install
  * list
  * info
  * remove
* yum list
* yum info
* yum Install
* yum localinstall
* yum provides (whatprovides)
* yum remove/erase
* yum repolist
* yum repository-packages
* yum search
* yum update
* yum history


## Create a local repository

* **OBS: Critical for the exam**

1. Create a local dir for the repository
```
#mkdir -v -p /var/local && cd /var/local
```
2. Copy pkgs to this directory

```
#cp /mnt/Packages/dcraw-9.19-6.el7.x86_64.rpm /var/local/
```

3. Install the createrepo pkg

```
#yum -y install createrepo
Loaded plugins: langpacks, product-id, search-disabled-repos, subscription-manager
Package createrepo-0.9.9-25.el7_2.noarch already installed and latest version
Nothing to do
#
```

4. Run createrepo command on /var/local


```
[root@host1 /var/local]#createrepo -v /var/local/
Spawning worker 0 with 1 pkgs
Spawning worker 1 with 0 pkgs
Worker 0: reading dcraw-9.19-6.el7.x86_64.rpm
Workers Finished
Saving Primary metadata
Saving file lists metadata
Saving other metadata
Generating sqlite DBs
Starting other db creation: Thu May  5 17:53:03 2016
Ending other db creation: Thu May  5 17:53:03 2016
Starting filelists db creation: Thu May  5 17:53:03 2016
Ending filelists db creation: Thu May  5 17:53:03 2016
Starting primary db creation: Thu May  5 17:53:03 2016
Ending primary db creation: Thu May  5 17:53:04 2016
Sqlite DBs complete

```

5. Create a definition file /etc/yum.repos.d/local.repo

* Use the contents below

```
[local]
name = local repo
baseurl = file:///var/local/
enabled = 1
gpgcheck = 0
```

6. Check the local.repo

```
yum -v repolist
```

## yum List

* yum list installed
* yum list available
* yum list (installed and available)
* yum list updates
* yum list pkg (installed or available)
* yum list installed pkg* (list pkg* installed)
