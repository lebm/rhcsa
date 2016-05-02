# LAB 2 - Getting some info

## logname
* Show real user
* Even after su or sudo, logname still shows the real user, the user who logins
```
[bravo@host1 ~]$ logname
bravo
[bravo@host1 ~]$ sudo -i
[root@host1 ~]# whoami
root
[root@host1 ~]# logname
bravo
[root@host1 ~]#
```

## whoami
* Effective user ID.  Same as id -un

```
[bravo@host1 ~]$ whoami
bravo
[bravo@host1 ~]$ id -un
bravo
[bravo@host1 ~]$ sudo -i
[root@host1 ~]# whoami
root
[root@host1 ~]# id -un
root
[root@host1 ~]#
```
## who
* Real logged in user
* Many options (see --help)

```
[bravo@host1 ~]$ who
bravo    pts/0        2016-05-02 14:52 (192.168.0.10)
[bravo@host1 ~]$ who -r
         run-level 3  2016-05-02 14:52
[bravo@host1 ~]$ who -a
           system boot  2016-05-02 14:51
           run-level 3  2016-05-02 14:52
LOGIN      ttyS0        2016-05-02 14:52              1343 id=tyS0
LOGIN      tty1         2016-05-02 14:52              1344 id=tty1
bravo    + pts/0        2016-05-02 14:52   .          1534 (192.168.0.10)
[bravo@host1 ~]$ who -l
LOGIN    ttyS0        2016-05-02 14:52              1343 id=tyS0
LOGIN    tty1         2016-05-02 14:52              1344 id=tty1
[bravo@host1 ~]$ who -b
         system boot  2016-05-02 14:51
[bravo@host1 ~]$
```

## w
* Show who is logged on and what they are doing.

```
[bravo@host1 ~]$ w
 15:10:14 up 18 min,  1 user,  load average: 0.04, 0.04, 0.08
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
bravo    pts/0    192.168.0.10     14:52    6.00s  0.32s  0.01s w
[bravo@host1 ~]$
```

## last
* show listing of last logged in users
* man last for more options

```
[bravo@host1 ~]$ last -4
bravo    pts/0        192.168.0.10     Mon May  2 14:52   still logged in
reboot   system boot  3.10.0-327.13.1. Mon May  2 14:51 - 15:16  (00:24)
bravo    pts/0        192.168.0.10     Thu Apr 28 15:19 - 15:28  (00:09)
reboot   system boot  3.10.0-327.13.1. Thu Apr 28 15:18 - 15:16 (3+23:57)

```

## Commands to check host information

```
[bravo@host1 ~]$ hostname
host1
[bravo@host1 ~]$ hostnamectl
   Static hostname: host1
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 79a3935258bcd24397b5610b23faaf9f
           Boot ID: 389a2f36cbf14e408ce1d9c91e8baec5
    Virtualization: kvm
  Operating System: Red Hat Enterprise Linux
       CPE OS Name: cpe:/o:redhat:enterprise_linux:7.2:GA:server
            Kernel: Linux 3.10.0-327.13.1.el7.x86_64
      Architecture: x86-64
[bravo@host1 ~]$ uname -a
Linux host1 3.10.0-327.13.1.el7.x86_64 #1 SMP Mon Feb 29 13:22:02 EST 2016 x86_64 x86_64 x86_64 GNU/Linux
[bravo@host1 ~]$ uname -snrvmpio
Linux host1 3.10.0-327.13.1.el7.x86_64 #1 SMP Mon Feb 29 13:22:02 EST 2016 x86_64 x86_64 x86_64 GNU/Linux
[bravo@host1 ~]$ uname -s
Linux
[bravo@host1 ~]$ uname -n
host1
[bravo@host1 ~]$ uname -r
3.10.0-327.13.1.el7.x86_64
[bravo@host1 ~]$ uname -v
#1 SMP Mon Feb 29 13:22:02 EST 2016
[bravo@host1 ~]$ uname -m
x86_64
[bravo@host1 ~]$ uname -p
x86_64
[bravo@host1 ~]$ uname -i
x86_64
[bravo@host1 ~]$ uname -o
GNU/Linux
[bravo@host1 ~]$
```

## Checking date and time
* See man page for changing operations

```
[bravo@host1 ~]$ date
Mon May  2 15:24:37 BRT 2016
[bravo@host1 ~]$ timedatectl
      Local time: Mon 2016-05-02 15:24:40 BRT
  Universal time: Mon 2016-05-02 18:24:40 UTC
        RTC time: Mon 2016-05-02 18:24:40
       Time zone: America/Sao_Paulo (BRT, -0300)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: no
      DST active: no
 Last DST change: DST ended at
                  Sat 2016-02-20 23:59:59 BRST
                  Sat 2016-02-20 23:00:00 BRT
 Next DST change: DST begins (the clock jumps one hour forward) at
                  Sat 2016-10-15 23:59:59 BRT
                  Sun 2016-10-16 01:00:00 BRST
[bravo@host1 ~]$ timedatectl help
timedatectl [OPTIONS...] COMMAND ...

Query or change system time and date settings.

  -h --help                Show this help message
     --version             Show package version
     --no-pager            Do not pipe output into a pager
     --no-ask-password     Do not prompt for password
  -H --host=[USER@]HOST    Operate on remote host
  -M --machine=CONTAINER   Operate on local container
     --adjust-system-clock Adjust system clock when changing local RTC mode

Commands:
  status                   Show current time settings
  set-time TIME            Set system time
  set-timezone ZONE        Set system time zone
  list-timezones           Show known time zones
  set-local-rtc BOOL       Control whether RTC is in local time
  set-ntp BOOL             Control whether NTP is enabled
```

## How long the machine is up ?

```
[bravo@host1 ~]$ uptime
 15:28:28 up 36 min,  1 user,  load average: 0.11, 0.20, 0.13
[bravo@host1 ~]$ uptime -s
2016-05-02 14:51:35
[bravo@host1 ~]$ uptime -p
up 37 minutes
[bravo@host1 ~]$
```
