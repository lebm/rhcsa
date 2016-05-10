# SELinux

## Packages

* policycoreutils (provides utilities for managing SELinux)
* policycoreutils-python (provides utilities for managing SELinux)
* selinux-policy (provides SELinux reference policy)
* selinux-policy-targeted (provides SELinux targeted policy)
* libselinux-utils (provides some tools for managing SELinux)
* setroubleshoot-server (provides tools for deciphering audit log messages)
* setools (provides tools for audit log monitoring, querying policy, and file context management)
* setools-console (provides tools for audit log monitoring, querying policy, and file context management)
* mcstrans (tools to translate different levels to easy-to-understand format)


```
yum install policycoreutils policycoreutils-python selinux-policy selinux-policy-targeted libselinux-utils setroubleshoot-server setools setools-console mcstrans
```

## Configuration file

* /etc/selinux/config

```
[root@host1 ~]# cat /etc/selinux/config

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted


[root@host1 ~]#
```


## Modes

* Enforcing
* Permissive
* Disabled


## getenforce

```
[root@host1 ~]# getenforce
Enforcing
```

## sestatus

```
[root@host1 ~]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      28
[root@host1 ~]#
```

## Checking for erros

```
cat /var/log/messages | grep "SELinux is preventing"
cat /var/log/messages | grep "SELinux"
```

## Policy

* Implements a MAC security, but if DAC denies, SELinux policy rules are not evaluated.
* a set of rules that define the security and access rights for everything in the system
* "everything" means (entities):
  * users (linux users mapped to SELinux users)
  * roles (define which users can access a processe)
  * processes (domains)
  * files (types)
* The policy defines how each of these entities are related to one another.
* A policy defines:
  * user access to roles
  * role access to domains
  * domains access to types
* Processe = subject
* object = anything that can be acted upon (file, dir, dev, port ...)
* permissions = actions a subject can perform on a object
* RBAC
  * A user can assume a role if it is allowed
  * A domain define which role can access
* domain
  * Context within which a subject can run.
  * Restricts what a subject can do on objects, can be restricted to access certain object types
    * This is called TE (Type Enforcement)

* type
  * context for objects
  * Define its purpose (web page, etc file ...)
* Policy is modular
  * semodule manages SELinux modules

```
[root@host1 users]# semodule -l
abrt    1.4.1
accountsd       1.1.0
acct    1.6.0
afs     1.9.0
aiccu   1.1.0
aide    1.7.1
(...)
```
* modules active
  * ".pp" files

```
ls -l /etc/selinux/targeted/modules/active/modules/
-rw-r--r--. 1 root root 15633 Apr 28 12:12 abrt.pp
-rw-r--r--. 1 root root  8696 Apr 28 12:12 accountsd.pp
-rw-r--r--. 1 root root  8473 Apr 28 12:12 acct.pp
-rw-r--r--. 1 root root 10398 Apr 28 12:12 afs.pp
(...)
```

* tweak settings: SELinux booleans


```
[root@host1 users]# semanage boolean -l
SELinux boolean                State  Default Description

ftp_home_dir                   (off  ,  off)  Allow ftp to home dir
smartmon_3ware                 (off  ,  off)  Allow smartmon to 3ware
mpd_enable_homedirs            (off  ,  off)  Allow mpd to enable homedirs
xdm_sysadm_login               (off  ,  off)  Allow xdm to sysadm login
xen_use_nfs                    (off  ,  off)  Allow xen to use nfs
mozilla_read_content           (off  ,  off)  Allow mozilla to read content
(...)
[root@host1 users]# getsebool ftpd_anon_write
ftpd_anon_write --> off
[root@host1 users]# setsebool ftpd_anon_write on
[root@host1 users]# getsebool ftpd_anon_write
ftpd_anon_write --> on
[root@host1 users]#
```

* setsebool -P
  * Make change permanent

## Contexts

  * A context is a label (attribute) of a Linux entity with security related information used by SELinux policy to make control decisions.
  * Every entity on Linux can have a security context
  * The meaning of a context depends on the object


```
# ls -Z /etc/passwd
-rw-r--r--. root root system_u:object_r:passwd_file_t:s0 /etc/passwd
# ls -Z /etc/locale.conf
-rw-r--r--. root root system_u:object_r:locale_t:s0    /etc/locale.conf
# ls -Z /etc/httpd/conf/
httpd.conf  magic       
# ls -Z /etc/httpd/conf/httpd.conf
-rw-r--r--. root root system_u:object_r:httpd_config_t:s0 /etc/httpd/conf/httpd.conf
# ls -Z /bin/ls
-rwxr-xr-x. root root system_u:object_r:bin_t:s0       /bin/ls
# ls -Z /etc/logrotate.conf
-rw-r--r--. root root system_u:object_r:etc_t:s0       /etc/logrotate.conf

```

* file security context example
  * system_u:object_r:etc_t:s0
    * SELinux user: system_u (root mapped to system_u)
    * role: object_r (not important for file)
    * type: etc_t (the most important for files)
    * multilevel security: s0



```# ps -efZ | grep 'httpd\|vsftpd' | grep -v grep
system_u:system_r:httpd_t:s0    root      1803     1  0 10:40 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
system_u:system_r:httpd_t:s0    apache    1804  1803  0 10:40 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
system_u:system_r:httpd_t:s0    apache    1805  1803  0 10:40 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
system_u:system_r:httpd_t:s0    apache    1806  1803  0 10:40 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
system_u:system_r:httpd_t:s0    apache    1807  1803  0 10:40 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
system_u:system_r:httpd_t:s0    apache    1808  1803  0 10:40 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
system_u:system_r:ftpd_t:s0-s0:c0.c1023 root 1905  1  0 10:40 ?        00:00:00 /usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf
```

* process context example
  * system_u:system_r:httpd_t:s0
    * SELinux user: system_u
    * role: system_r
    * domain: httpd_t
    * multilevel security: s0
  * The domain tells which object types the process has permissions on
  * naming conventions
    * \_u: users
    * \_r: roles
    * \_t: type for objects, domain for subject
  * allow rule for subject
    * allow \<domain> \<type>:\<class> { \<permissions> };
      * class: resource (file, dir, port, dev, symlink...)
      * "domain" subject can access "class" object of type "type" with "permissions"

```
# ls -Z /var/www/html/index.html
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 /var/www/html/index.html
```
* sesearch
  * Can be used to check the type of access allowed for domains

```
# sesearch --allow --source httpd_t --target httpd_sys_content_t --class file
Found 4 semantic av rules:
   allow httpd_t httpd_sys_content_t : file { ioctl read getattr lock open } ;
   allow httpd_t httpd_content_type : file { ioctl read getattr lock open } ;
   allow httpd_t httpdcontent : file { ioctl read write create getattr setattr lock append unlink link rename execute open } ;
   allow httpd_t httpd_content_type : file { ioctl read getattr lock open } ;
```

* Changing object type
  * Ex: chcon --type var_t /var/www/html/index.html
* Restoring object context
  * Ex: restorecon -v /var/www/html/index.html
* Definition of file Contexts (used by restorecon)
  * /etc/selinux/targeted/contexts/files/file_contexts
  * etc/selinux/targeted/contexts/files/file_contexts.local
  * semanage can add local file contexts

```
# semanage fcontext --add --type httpd_sys_content_t "/www(/.*)?"
# cat /etc/selinux/targeted/contexts/files/file_contexts.local
# This file is auto-generated by libsemanage
# Do not edit directly.

/www(/.*)?    system_u:object_r:httpd_sys_content_t:s0
#                 
```

* Verify context of an object
  * matchpathcon -V /www/html/index.html

```
# matchpathcon -V /www/html/
/www/html has context unconfined_u:object_r:default_t:s0, should be system_u:object_r:httpd_sys_content_t:s0
# restorecon -Rv /www
restorecon reset /www context unconfined_u:object_r:default_t:s0->unconfined_u:object_r:httpd_sys_content_t:s0
restorecon reset /www/html context unconfined_u:object_r:default_t:s0->unconfined_u:object_r:httpd_sys_content_t:s0
# matchpathcon -V /www/html/
/www/html verified.
```

## Domain transition
* method where a process changes its context from one domain to another
* Example:
  * proca in contexta ...
  * ...executes appx ...
  * ...appx starts procb in contextb
    * contexta transits to contexb through appx
    * appx is the entrypoint to contextb
* Conditions
  * source domain must have permission to exec the entrypoint (executable)
    * Ex:sesearch -s init_t -t ftpd_exec_t -c file -p execute -Ad
  * the entrypoint context must have permission to enter target domain
    * sesearch -s ftpd_t -t ftpd_exec_t -c file -p entrypoint -Ad
  * source domain must have permission to transit do target domain
    * sesearch -s init_t -t ftpd_t -c process -p transition -Ad

```
# sesearch -s init_t -t ftpd_exec_t -c file -p execute -Ad
Found 1 semantic av rules:
   allow init_t ftpd_exec_t : file { read getattr execute open } ;

# sesearch -s ftpd_t -t ftpd_exec_t -c file -p entrypoint -Ad
Found 1 semantic av rules:
   allow ftpd_t ftpd_exec_t : file { ioctl read getattr lock execute execute_no_trans entrypoint open } ;

# sesearch -s init_t -t ftpd_t -c process -p transition -Ad
Found 1 semantic av rules:
   allow init_t ftpd_t : process transition ;

#
```
