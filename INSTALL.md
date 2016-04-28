# INSTALLING Red Hat 7.x

## Virtual Console Screens

1. Console 1: principal, start install Console
2. Bash shell
3. /tmp/anaconda.log
4. /tmp/storage.log
5. /tmp/program.log
6. Default grahical configuration and installation console

installation generates many log files

/root  
* anaconda-ks.cfg  
* install.log  
* install.log.syslog  

/var/log  
* anaconda.log
* anaconda.syslog
* anaconda.xlog
* anaconda.packaging.log
* anaconda.program.log
* anaconda.storage.log
* anaconda.yum.log

## To install a graphical support  
> Warning: It may take 15 minutes to install
```
# yum group install "X Window System" "GNOME" -y
# systemctl set-default graphical.target
```

## Server host1.example.com
This host will be installed using libvirt and will serve as an install server for two VMs.
I wil try to install those two VMs inside this host (VMs inside VMs).
