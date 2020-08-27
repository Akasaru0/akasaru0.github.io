---
layout : post
title: Linux Challenges
---
It's my first tryhackme writeup... it's easy but it help!



## [Task 1] Linux Challenges Introduction

### #1 How many visible files can you see in garrys home directory?

We need to connect with ssh to the machine. We have also:

Username : **garry**

Password : **letmein**

```shell
aka@DELL-G5:~$ ssh garry@10.10.99.124
The authenticity of host '10.10.99.124 (10.10.99.124)' can't be established.
ECDSA key fingerprint is SHA256:OJXQTwo9hYTcy+0e47ioVLum5VUGCp8PuZQE/H7bTTg.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.99.124' (ECDSA) to the list of known hosts.
garry@10.10.99.124's password: 
Last login: Sun Aug 16 09:44:52 2020 from 10.100.1.212
```

ok I'm in! so now let's just do a `ls`

```shell
garry@ip-10-10-99-124:~$ ls
flag1.txt  flag24  flag29
garry@ip-10-10-99-124:~$ 
```

there is only 3 files visible.



## [Task 2] The Basics

### #1 What is flag 1?

I see the file `flag1.txt` in the home directory so :

```shell
garry@ip-10-10-99-124:~$ cat flag1.txt 
There are flags hidden around the file system, its your job to find them.

Flag 1: f40dc0cff080ad38a6ba9a1c2c038b2c

Log into bobs account to get flag 2.

Username: bob
Password: linuxrules
garry@ip-10-10-99-124:~$ 
```

flag1 : f40dc0cff080ad38a6ba9a1c2c038b2c



### #2 Log into bob's account using the credentials shown in flag 1.What is flag 2?

In `flag1.txt` we have :

**Username** : bob

**Password** linuxrules

so let's just use it!

```shell
garry@ip-10-10-99-124:~$ su bob
Password: 
bob@ip-10-10-99-124:/home/garry$ 
```

Now I'm bob. I need to go at the home directory  

```shell
bob@ip-10-10-99-124:/home/garry$ cd /home/bob/
bob@ip-10-10-99-124:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos  flag13  flag2.txt  flag21.php  flag8.tar.gz
bob@ip-10-10-99-124:~$ 
```

Now, we just need to cat flag2.txt

```shell
bob@ip-10-10-99-124:~$ cat flag2.txt 
Flag 2: 8e255dfa51c9cce67420d2386cede596
bob@ip-10-10-99-124:~$ 
```

flag2 : 8e255dfa51c9cce67420d2386cede596



### #3 Flag 3 is located where bob's bash history gets stored.

bash history is a hiden directory.

```shell
bob@ip-10-10-99-124:~$ ls -al
total 204
drwxr-xr-x 21 bob  bob   4096 Feb 20  2019 .
drwxr-xr-x  6 root root  4096 Feb 20  2019 ..
-rw-------  1 bob  bob   1388 Feb 20  2019 .ICEauthority
-rw-------  1 bob  bob    214 Feb 19  2019 .Xauthority
-rwxrwxr-x  1 bob  bob     14 Feb 19  2019 .Xclients
-rw-rw-r--  1 bob  bob    273 Feb 20  2019 .bash_history
-rw-r--r--  1 bob  bob    220 Feb 18  2019 .bash_logout
-rw-r--r--  1 bob  bob   3890 Feb 19  2019 .bashrc
drwx------ 11 bob  bob   4096 Feb 20  2019 .cache
drwx------ 12 bob  bob   4096 Feb 20  2019 .config
drwx------  3 bob  bob   4096 Feb 19  2019 .dbus
drwx------  2 bob  bob   4096 Feb 19  2019 .gconf
drwx------  3 bob  bob   4096 Feb 20  2019 .gnupg
drwxrwxr-x  3 bob  bob   4096 Feb 19  2019 .local
drwx------  5 bob  bob   4096 Feb 20  2019 .mozilla
-rw-------  1 bob  bob     18 Feb 19  2019 .mysql_history
drwxrwxr-x  2 bob  bob   4096 Feb 18  2019 .nano
-rw-r--r--  1 bob  bob    699 Feb 19  2019 .profile
-rw-rw-r--  1 bob  bob     66 Feb 18  2019 .selected_editor
drwx------  2 bob  bob   4096 Feb 18  2019 .ssh
-rw-------  1 bob  bob   4595 Feb 20  2019 .viminfo
drwx------  2 bob  bob   4096 Feb 19  2019 .vnc
-rw-rw-r--  1 bob  bob     14 Feb 19  2019 .xsession
-rw-------  1 bob  bob  55891 Feb 20  2019 .xsession-errors
drwxr-xr-x  2 bob  bob   4096 Feb 19  2019 Desktop
drwxr-xr-x  2 bob  bob   4096 Feb 19  2019 Documents
drwxr-xr-x  2 bob  bob   4096 Feb 19  2019 Downloads
drwxr-xr-x  2 bob  bob   4096 Feb 19  2019 Music
drwxr-xr-x  2 bob  bob   4096 Feb 19  2019 Pictures
drwxr-xr-x  2 bob  bob   4096 Feb 19  2019 Public
drwxr-xr-x  2 bob  bob   4096 Feb 19  2019 Templates
drwxr-xr-x  2 bob  bob   4096 Feb 19  2019 Videos
drwxrwxr-x  2 bob  bob   4096 Feb 18  2019 flag13
-rw-rw-r--  1 bob  bob     41 Feb 18  2019 flag2.txt
-rw-rw-r--  1 bob  bob     65 Feb 20  2019 flag21.php
-rw-rw-r--  1 bob  bob    149 Feb 18  2019 flag8.tar.gz
bob@ip-10-10-99-124:~$ 
```

We found it !

```shell
-rw-rw-r--  1 bob  bob    273 Feb 20  2019 .bash_history
```

Let's `cat` it !

```shell
bob@ip-10-10-99-124:~$ cat .bash_history 
9daf3281745c2d75fc6e992ccfdedfcd
cat ~/.bash_history 
rm ~/.bash_history
vim ~/.bash_history
exit
ls
crontab -e
ls
cd /home/alice/
ls
cd .ssh
ssh -i .ssh/id_rsa alice@localhost
exit
ls
cd ../alice/
cat .ssh/id_rsa
cat /home/alice/.ssh/id_rsa
exit
cat ~/.bash_history 
exit
bob@ip-10-10-99-124:~$ 
```

flag 3 : 9daf3281745c2d75fc6e992ccfdedfcd



### #4 Flag 4 is located where cron jobs are created.

I don't know what is cron so I will google it!

I find in this site (https://shorturl.at/DOY04) : crontab -l

```shell
bob@ip-10-10-99-124:~$ crontab -l
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command

0 6 * * * echo 'flag4:dcd5d1dcfac0578c99b7e7a6437827f3' > /home/bob/flag4.txt
bob@ip-10-10-99-124:~$
```

flag4 : dcd5d1dcfac0578c99b7e7a6437827f3



### #5 Find and retrieve flag 5.

We need to find flag5 so we will use `find` 

```shell
bob@ip-10-10-99-124:~$ find / -name flag5.txt -type f 2>/dev/null
/lib/terminfo/E/flag5.txt
bob@ip-10-10-99-124:~$
```

Let's `cat` it !

```shell
bob@ip-10-10-99-124:~$ cat /lib/terminfo/E/flag5.txt
bd8f33216075e5ba07c9ed41261d1703
bob@ip-10-10-99-124:~$
```

Flag5 : bd8f33216075e5ba07c9ed41261d1703



### #6 "Grep" through flag 6 and find the flag. The first 2 characters of the flag is c9.

At first we need to find flag6

```shell
bob@ip-10-10-99-124:~$ find / -type f -name flag6.txt 2>/dev/null
/home/flag6.txt
```

And now just add to the command

```shell
bob@ip-10-10-99-124:~$ grep "c9" /home/flag6.txt
Sed sollicitudin eros quis vulputate rutrum. Curabitur mauris elit, elementum quis sapien sed, ullamcorper pellentesque neque. Aliquam erat volutpat. Cras vehicula mauris vel lectus hendrerit, sed malesuada ipsum consectetur. Donec in enim id erat condimentum vestibulum c9e142a1e25b24a837b98db589b08be5 vitae eget nisi. Suspendisse eget commodo libero. Mauris eget gravida quam, a interdum orci. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Quisque eu nisi non ligula tempor efficitur. Etiam eleifend, odio vel bibendum mattis, purus metus consectetur turpis, eu dignissim elit nunc at tortor. Mauris sapien enim, elementum faucibus magna at, rutrum venenatis ipsum.
bob@ip-10-10-99-124:~$ 
```

flag6: c9e142a1e25b24a837b98db589b08be5



### #7 Look at the systems processes. What is flag 7.

I check to my RTFM Book and i found inside `ps -ef` so i try

```shell
bob@ip-10-10-61-12:~$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 16:26 ?        00:00:03 /sbin/init
root         2     0  0 16:26 ?        00:00:00 [kthreadd]
root         3     2  0 16:26 ?        00:00:00 [ksoftirqd/0]
root         4     2  0 16:26 ?        00:00:00 [kworker/0:0]
root         5     2  0 16:26 ?        00:00:00 [kworker/0:0H]
root         7     2  0 16:26 ?        00:00:00 [rcu_sched]
root         8     2  0 16:26 ?        00:00:00 [rcu_bh]
root         9     2  0 16:26 ?        00:00:00 [migration/0]
root        10     2  0 16:26 ?        00:00:00 [watchdog/0]
root        11     2  0 16:26 ?        00:00:00 [kdevtmpfs]
root        12     2  0 16:26 ?        00:00:00 [netns]
root        13     2  0 16:26 ?        00:00:00 [perf]
root        14     2  0 16:26 ?        00:00:00 [xenwatch]
root        15     2  0 16:26 ?        00:00:00 [xenbus]
root        17     2  0 16:26 ?        00:00:00 [khungtaskd]
root        18     2  0 16:26 ?        00:00:00 [writeback]
root        19     2  0 16:26 ?        00:00:00 [ksmd]
root        20     2  0 16:26 ?        00:00:00 [khugepaged]
root        21     2  0 16:26 ?        00:00:00 [crypto]
root        22     2  0 16:26 ?        00:00:00 [kintegrityd]
root        23     2  0 16:26 ?        00:00:00 [bioset]
root        24     2  0 16:26 ?        00:00:00 [kblockd]
root        25     2  0 16:26 ?        00:00:00 [ata_sff]
root        26     2  0 16:26 ?        00:00:00 [md]
root        27     2  0 16:26 ?        00:00:00 [devfreq_wq]
root        28     2  0 16:26 ?        00:00:00 [kworker/u30:1]
root        30     2  0 16:26 ?        00:00:00 [kswapd0]
root        31     2  0 16:26 ?        00:00:00 [vmstat]
root        32     2  0 16:26 ?        00:00:00 [fsnotify_mark]
root        33     2  0 16:26 ?        00:00:00 [ecryptfs-kthrea]
root        49     2  0 16:26 ?        00:00:00 [kthrotld]
root        50     2  0 16:26 ?        00:00:00 [bioset]
root        51     2  0 16:26 ?        00:00:00 [bioset]
root        52     2  0 16:26 ?        00:00:00 [bioset]
root        53     2  0 16:26 ?        00:00:00 [bioset]
root        54     2  0 16:26 ?        00:00:00 [bioset]
root        55     2  0 16:26 ?        00:00:00 [bioset]
root        56     2  0 16:26 ?        00:00:00 [bioset]
root        57     2  0 16:26 ?        00:00:00 [bioset]
root        58     2  0 16:26 ?        00:00:00 [bioset]
root        59     2  0 16:26 ?        00:00:00 [bioset]
root        60     2  0 16:26 ?        00:00:00 [bioset]
root        61     2  0 16:26 ?        00:00:00 [bioset]
root        62     2  0 16:26 ?        00:00:00 [bioset]
root        63     2  0 16:26 ?        00:00:00 [bioset]
root        64     2  0 16:26 ?        00:00:00 [bioset]
root        65     2  0 16:26 ?        00:00:00 [bioset]
root        66     2  0 16:26 ?        00:00:00 [bioset]
root        67     2  0 16:26 ?        00:00:00 [bioset]
root        68     2  0 16:26 ?        00:00:00 [bioset]
root        69     2  0 16:26 ?        00:00:00 [bioset]
root        70     2  0 16:26 ?        00:00:00 [bioset]
root        71     2  0 16:26 ?        00:00:00 [bioset]
root        72     2  0 16:26 ?        00:00:00 [bioset]
root        73     2  0 16:26 ?        00:00:00 [bioset]
root        74     2  0 16:26 ?        00:00:00 [scsi_eh_0]
root        75     2  0 16:26 ?        00:00:00 [scsi_tmf_0]
root        76     2  0 16:26 ?        00:00:00 [scsi_eh_1]
root        77     2  0 16:26 ?        00:00:00 [scsi_tmf_1]
root        79     2  0 16:26 ?        00:00:00 [bioset]
root        80     2  0 16:26 ?        00:00:00 [bioset]
root        84     2  0 16:26 ?        00:00:00 [ipv6_addrconf]
root        97     2  0 16:26 ?        00:00:00 [deferwq]
root       252     2  0 16:26 ?        00:00:00 [raid5wq]
root       284     2  0 16:26 ?        00:00:00 [bioset]
root       305     2  0 16:26 ?        00:00:00 [jbd2/xvda1-8]
root       306     2  0 16:26 ?        00:00:00 [ext4-rsv-conver]
root       368     2  0 16:26 ?        00:00:00 [kworker/0:1H]
root       372     1  0 16:26 ?        00:00:00 /lib/systemd/systemd-journald
root       382     2  0 16:26 ?        00:00:00 [kauditd]
root       398     2  0 16:26 ?        00:00:00 [iscsi_eh]
root       402     2  0 16:26 ?        00:00:00 [ib_addr]
root       404     2  0 16:26 ?        00:00:00 [ib_mcast]
root       405     2  0 16:26 ?        00:00:00 [ib_nl_sa_wq]
root       406     2  0 16:26 ?        00:00:00 [ib_cm]
root       407     2  0 16:26 ?        00:00:00 [iw_cm_wq]
root       408     2  0 16:26 ?        00:00:00 [rdma_cm]
root       442     1  0 16:26 ?        00:00:00 /sbin/lvmetad -f
root       474     1  0 16:26 ?        00:00:00 /lib/systemd/systemd-udevd
root       479     2  0 16:26 ?        00:00:00 [loop0]
root       483     2  0 16:26 ?        00:00:00 [loop1]
root       523     2  0 16:26 ?        00:00:00 [loop2]
root       536     2  0 16:26 ?        00:00:00 [loop3]
root       542     2  0 16:26 ?        00:00:00 [loop4]
systemd+   806     1  0 16:26 ?        00:00:00 /lib/systemd/systemd-timesyncd
root      1038     1  0 16:26 ?        00:00:00 /sbin/dhclient -1 -v -pf /run/dhclient.eth0.pid -lf /var/lib/dhcp/dhclient.eth0.leases -I -df /var/lib/dhcp/dhclient6.eth0.leases eth0
syslog    1204     1  0 16:26 ?        00:00:00 /usr/sbin/rsyslogd -n
root      1209     1  0 16:26 ?        00:00:00 /usr/sbin/acpid
root      1213     1  0 16:26 ?        00:00:00 /lib/systemd/systemd-logind
root      1216     1  0 16:26 ?        00:00:00 /usr/lib/snapd/snapd
root      1225     1  0 16:26 ?        00:00:00 /usr/bin/lxcfs /var/lib/lxcfs/
daemon    1237     1  0 16:26 ?        00:00:00 /usr/sbin/atd -f
avahi     1245     1  0 16:26 ?        00:00:00 avahi-daemon: running [ip-10-10-61-12.local]
root      1252     1  0 16:26 ?        00:00:00 /usr/sbin/cron -f
root      1260     1  0 16:26 ?        00:00:00 /usr/lib/accountsservice/accounts-daemon
avahi     1265  1245  0 16:26 ?        00:00:00 avahi-daemon: chroot helper
message+  1270     1  0 16:26 ?        00:00:00 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation
root      1291     1  0 16:26 ?        00:00:00 /usr/sbin/NetworkManager --no-daemon
root      1308     1  0 16:26 ?        00:00:00 /sbin/mdadm --monitor --pid-file /run/mdadm/monitor.pid --daemonise --scan --syslog
root      1320     1  0 16:26 ?        00:00:00 /snap/amazon-ssm-agent/1068/amazon-ssm-agent
root      1335     1  0 16:26 ?        00:00:00 /usr/lib/policykit-1/polkitd --no-debug
mysql     1348     1  0 16:26 ?        00:00:00 /usr/sbin/mysqld
whoopsie  1371     1  0 16:26 ?        00:00:00 /usr/bin/whoopsie -f
root      1380     1  0 16:26 ?        00:00:00 flag7:274adb75b337307bd57807c005ee6358 1000000
root      1384     1  0 16:26 ?        00:00:00 /usr/sbin/sshd -D
root      1391     1  0 16:26 ?        00:00:00 /sbin/iscsid
root      1392     1  0 16:26 ?        00:00:00 /sbin/iscsid
xrdp      1491     1  0 16:26 ?        00:00:00 /usr/sbin/xrdp
root      1504     1  0 16:26 ttyS0    00:00:00 /sbin/agetty --keep-baud 115200 38400 9600 ttyS0 vt220
root      1506     1  0 16:26 ?        00:00:00 /usr/sbin/xrdp-sesman
root      1509     1  0 16:26 tty1     00:00:00 /sbin/agetty --noclear tty1 linux
root      1514     1  0 16:26 ?        00:00:00 /usr/sbin/gdm3
root      1520  1514  0 16:26 ?        00:00:00 gdm-session-worker [pam/gdm-launch-environment]
root      1550     1  0 16:26 ?        00:00:00 /usr/sbin/apache2 -k start
gdm       1628     1  0 16:26 ?        00:00:00 /lib/systemd/systemd --user
gdm       1631  1628  0 16:26 ?        00:00:00 (sd-pam)
gdm       1634  1520  0 16:26 tty7     00:00:00 /usr/lib/gdm3/gdm-x-session /usr/bin/gnome-session --autostart /usr/share/gdm/greeter/autostart
root      1636  1634  0 16:26 tty7     00:00:00 /usr/lib/xorg/Xorg vt7 -displayfd 3 -auth /run/user/126/gdm/Xauthority -background none -noreset -keeptty -verbose 3
gdm       1731  1634  0 16:26 tty7     00:00:00 dbus-daemon --print-address 4 --session
gdm       1734  1634  0 16:26 tty7     00:00:00 /usr/lib/gnome-session/gnome-session-binary --autostart /usr/share/gdm/greeter/autostart
gdm       1745     1  0 16:26 tty7     00:00:00 /usr/lib/at-spi2-core/at-spi-bus-launcher
gdm       1750  1745  0 16:26 tty7     00:00:00 /usr/bin/dbus-daemon --config-file=/etc/at-spi2/accessibility.conf --nofork --print-address 3
gdm       1753     1  0 16:26 tty7     00:00:00 /usr/lib/at-spi2-core/at-spi2-registryd --use-gnome-session
gdm       1763  1734  0 16:26 tty7     00:00:00 /usr/lib/gnome-settings-daemon/gnome-settings-daemon
root      1770     1  0 16:26 ?        00:00:00 /usr/lib/upower/upowerd
colord    1777     1  0 16:27 ?        00:00:00 /usr/lib/colord/colord
gdm       1780  1734  0 16:27 tty7     00:00:02 gnome-shell --mode=gdm
gdm       1789     1  0 16:27 ?        00:00:00 /usr/bin/pulseaudio --start --log-target=syslog
rtkit     1790     1  0 16:27 ?        00:00:00 /usr/lib/rtkit/rtkit-daemon
gdm       1837  1780  0 16:27 tty7     00:00:00 ibus-daemon --xim --panel disable
gdm       1842  1837  0 16:27 tty7     00:00:00 /usr/lib/ibus/ibus-dconf
gdm       1845     1  0 16:27 tty7     00:00:00 /usr/lib/ibus/ibus-x11 --kill-daemon
root      1862     1  0 16:27 ?        00:00:00 /sbin/wpa_supplicant -u -s -O /run/wpa_supplicant
gdm       1882  1837  0 16:27 tty7     00:00:00 /usr/lib/ibus/ibus-engine-simple
root      1890  1384  0 16:27 ?        00:00:00 sshd: garry [priv]
garry     1892     1  0 16:27 ?        00:00:00 /lib/systemd/systemd --user
garry     1894  1892  0 16:27 ?        00:00:00 (sd-pam)
garry     1953  1890  0 16:27 ?        00:00:00 sshd: garry@pts/0
garry     1954  1953  0 16:27 pts/0    00:00:00 -bash
root      1982  1384  0 16:27 ?        00:00:00 sshd: bob [priv]
bob       1984     1  0 16:28 ?        00:00:00 /lib/systemd/systemd --user
bob       1985  1984  0 16:28 ?        00:00:00 (sd-pam)
bob       2021  1982  0 16:28 ?        00:00:04 sshd: bob@pts/1
bob       2022  2021  0 16:28 pts/1    00:00:00 -bash
www-data  2160  1550  0 16:31 ?        00:00:00 /usr/sbin/apache2 -k start
www-data  2161  1550  0 16:31 ?        00:00:00 /usr/sbin/apache2 -k start
root      2240     1  0 16:31 ?        00:00:00 /usr/sbin/cupsd -l
root      2241     1  0 16:31 ?        00:00:00 /usr/sbin/cups-browsed
root      2248     2  0 16:31 ?        00:00:00 [kworker/0:3]
root      2487     2  0 16:39 ?        00:00:00 [kworker/u30:2]
root      2533     2  0 16:52 ?        00:00:00 [kworker/u30:0]
root      2534     2  0 16:52 ?        00:00:00 [kworker/0:1]
bob       2554  2022  0 16:57 pts/1    00:00:00 ps -ef
```

Ok so I don't want to search inside my flag

```shell
bob@ip-10-10-61-12:~$ ps -ef |grep flag7
root      1380     1  0 16:26 ?        00:00:00 flag7:274adb75b337307bd57807c005ee6358 1000000
bob       2556  2022  0 16:59 pts/1    00:00:00 grep --color=auto flag7
```

Flag7 :274adb75b337307bd57807c005ee6358



### #8 De-compress and get flag 8.

Let's find flag8 at first

```shell
bob@ip-10-10-61-12:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos  flag13  flag2.txt  flag21.php  flag8.tar.gz
```

Now i need to decompress it 

```shell
bob@ip-10-10-181-90:~$ gzip -d flag8.tar.gz 
bob@ip-10-10-181-90:~$ ls
Desktop    Downloads  Pictures  Templates  flag13     flag21.php
Documents  Music      Public    Videos     flag2.txt  flag8.tar
```

ok so now let's decompress .tar

```shell
bob@ip-10-10-181-90:~$ tar xvf flag8.tar 
flag8.txt
bob@ip-10-10-181-90:~$ ls
Desktop    Downloads  Pictures  Templates  flag13     flag21.php  flag8.txt
Documents  Music      Public    Videos     flag2.txt  flag8.tar
bob@ip-10-10-181-90:~$ cat flag8.txt 
75f5edb76fe98dd5fc9f577a3f5de9bc

```

we have the flag

Flag8: 75f5edb76fe98dd5fc9f577a3f5de9bc



### #9 By look in your hosts file, locate and retrieve flag 9.

I search in google where are "host file "  and I find `/etc/hosts` 

```shell
bob@ip-10-10-232-233:~$ cat /etc/hosts
127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts

127.0.0.1	dcf50ad844f9fe06339041ccc0d6e280.com
```

flag9 : dcf50ad844f9fe06339041ccc0d6e280



### #10 Find all other users on the system. What is flag 10.

To see all the user in the system, we need to:

```shell
bob@ip-10-10-232-233:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
lxd:x:106:65534::/var/lib/lxd/:/bin/false
messagebus:x:107:111::/var/run/dbus:/bin/false
uuidd:x:108:112::/run/uuidd:/bin/false
dnsmasq:x:109:65534:dnsmasq,,,:/var/lib/misc:/bin/false
sshd:x:110:65534::/var/run/sshd:/usr/sbin/nologin
pollinate:x:111:1::/var/cache/pollinate:/bin/false
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
bob:x:1001:1001:Bob,,,:/home/bob:/bin/bash
5e23deecfe3a7292970ee48ff1b6d00c:x:1002:1002:,,,:/home/5e23deecfe3a7292970ee48ff1b6d00c:/bin/bash
alice:x:1003:1003:,,,:/home/alice:/bin/bash
mysql:x:112:117:MySQL Server,,,:/nonexistent:/bin/false
xrdp:x:113:118::/var/run/xrdp:/bin/false
whoopsie:x:114:120::/nonexistent:/bin/false
avahi:x:115:121:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/bin/false
avahi-autoipd:x:116:122:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
colord:x:117:125:colord colour management daemon,,,:/var/lib/colord:/bin/false
geoclue:x:118:126::/var/lib/geoclue:/bin/false
speech-dispatcher:x:119:29:Speech Dispatcher,,,:/var/run/speech-dispatcher:/bin/false
hplip:x:120:7:HPLIP system user,,,:/var/run/hplip:/bin/false
kernoops:x:121:65534:Kernel Oops Tracking Daemon,,,:/:/bin/false
pulse:x:122:127:PulseAudio daemon,,,:/var/run/pulse:/bin/false
rtkit:x:123:129:RealtimeKit,,,:/proc:/bin/false
saned:x:124:130::/var/lib/saned:/bin/false
usbmux:x:125:46:usbmux daemon,,,:/var/lib/usbmux:/bin/false
gdm:x:126:131:Gnome Display Manager:/var/lib/gdm3:/bin/false
garry:x:1004:1006:,,,:/home/garry:/bin/bash
```

flag10: 5e23deecfe3a7292970ee48ff1b6d00c



## [Task 3] Linux Functionality



### #1 Run the command **flag11.** Locate where your command alias are stored and get flag 11.

I run the flag11

```shell
bob@ip-10-10-232-233:~$ flag11
You need to look where the alias are created...
```

I start to tap `alias` 

```shell
bob@ip-10-10-232-233:~$ alias
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias flag11='echo "You need to look where the alias are created..."'
alias grep='grep --color=auto'
alias l='ls -CF'
alias la='ls -A'
alias ll='ls -alF'
alias ls='ls --color=auto'
```

With (https://stackoverflow.com/questions/2614403/how-to-find-out-where-alias-in-the-bash-sense-is-defined-when-running-terminal) I try :

```shell
bob@ip-10-10-232-233:~$ grep -r 'flag11' ~
/home/bob/.bashrc:alias flag11='echo "You need to look where the alias are created..."' #b4ba05d85801f62c4c0d05d3a76432e0
```

tada flag11: b4ba05d85801f62c4c0d05d3a76432e0



### #2 Flag12 is located were MOTD's are usually found on an Ubuntu OS. What is flag12?

I search in google, (https://doc.ubuntu-fr.org/motd). motd i stored in /etc/update-motd.d/

So let's go:

```shell
bob@ip-10-10-232-233:/etc/update-motd.d$ cat /etc/update-motd.d/*
#!/bin/sh
#
#    00-header - create the header of the MOTD
#    Copyright (C) 2009-2010 Canonical Ltd.
#
#    Authors: Dustin Kirkland <kirkland@canonical.com>
#
#    This program is free software; you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation; either version 2 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License along
#    with this program; if not, write to the Free Software Foundation, Inc.,
#    51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

[ -r /etc/lsb-release ] && . /etc/lsb-release

if [ -z "$DISTRIB_DESCRIPTION" ] && [ -x /usr/bin/lsb_release ]; then
	# Fall back to using the very slow lsb_release utility
	DISTRIB_DESCRIPTION=$(lsb_release -s -d)
fi

# Flag12: 01687f0c5e63382f1c9cc783ad44ff7f

cat logo.txt

#!/bin/sh
#
#    10-help-text - print the help text associated with the distro
#    Copyright (C) 2009-2010 Canonical Ltd.
#
#    Authors: Dustin Kirkland <kirkland@canonical.com>,
#             Brian Murray <brian@canonical.com>
#
#    This program is free software; you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation; either version 2 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License along
#    with this program; if not, write to the Free Software Foundation, Inc.,
#    51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

printf "\n"
printf " * Documentation:  https://help.ubuntu.com\n"
printf " * Management:     https://landscape.canonical.com\n"
printf " * Support:        https://ubuntu.com/advantage\n"
#!/bin/sh
#
# CLOUD_IMG: This file was created/modified by the Cloud Image build process
# This file is not managed by a package.  If you no longer want to
# see this message you can safely remove the file.
echo ""
echo "  Get cloud support with Ubuntu Advantage Cloud Guest:"
echo "    http://www.ubuntu.com/business/services/cloud"
#!/bin/sh

stamp="/var/lib/update-notifier/updates-available"

[ ! -r "$stamp" ] || cat "$stamp"
#!/bin/sh

# if the current release is under development there won't be a new one
if [ "$(lsb_release -sd | cut -d' ' -f4)" = "(development" ]; then
    exit 0
fi
if [ -x /usr/lib/ubuntu-release-upgrader/release-upgrade-motd ]; then
    exec /usr/lib/ubuntu-release-upgrader/release-upgrade-motd
fi
#!/bin/sh

(egrep "overlayroot|/media/root-ro|/media/root-rw" /proc/mounts 2>/dev/null | sort -r) || true
echo
#!/bin/sh

if [ -x /usr/lib/update-notifier/update-motd-fsck-at-reboot ]; then
    exec /usr/lib/update-notifier/update-motd-fsck-at-reboot
fi#!/bin/sh

if [ -x /usr/lib/update-notifier/update-motd-reboot-required ]; then
    exec /usr/lib/update-notifier/update-motd-reboot-required
fi#!/bin/sh

SERIES=$(lsb_release -cs)
DESCRIPTION=$(lsb_release -ds)

[ "$SERIES" = "precise" ] || exit 0

[ -x /usr/bin/ubuntu-advantage ] || exit 0

if ubuntu-advantage is-esm-enabled; then
    cat <<EOF
This ${DESCRIPTION} system is configured to receive extended security updates
from Canonical:
 * https://www.ubuntu.com/esm
EOF
else
    cat <<EOF
This ${DESCRIPTION} system is past its End of Life, and is no longer
receiving security updates.  To protect the integrity of this system, it’s
critical that you enable Extended Security Maintenance updates:
 * https://www.ubuntu.com/esm
EOF
fi
echo

	████████╗██████╗ ██╗   ██╗██╗  ██╗ █████╗  ██████╗██╗  ██╗███╗   ███╗███████╗
	╚══██╔══╝██╔══██╗╚██╗ ██╔╝██║  ██║██╔══██╗██╔════╝██║ ██╔╝████╗ ████║██╔════╝
	   ██║   ██████╔╝ ╚████╔╝ ███████║███████║██║     █████╔╝ ██╔████╔██║█████╗  
	   ██║   ██╔══██╗  ╚██╔╝  ██╔══██║██╔══██║██║     ██╔═██╗ ██║╚██╔╝██║██╔══╝  
	   ██║   ██║  ██║   ██║   ██║  ██║██║  ██║╚██████╗██║  ██╗██║ ╚═╝ ██║███████╗
	   ╚═╝   ╚═╝  ╚═╝   ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝╚═╝     ╚═╝╚══════╝
				-- Linux Challenges --
bob@ip-10-10-232-233:/etc/update-motd.d$ 
```

Flag12: 01687f0c5e63382f1c9cc783ad44ff7f



### #3 Find the *difference* between two script files to find flag 13.

let's find the file : 

```shell
bob@ip-10-10-232-233:~$ find / -name flag13 2>/dev/null
/home/bob/flag13
```

the command `diff` is used to find the difference between 2 files

```shell
bob@ip-10-10-232-233:~$ cd flag13
bob@ip-10-10-232-233:~/flag13$ ls
script1  script2
bob@ip-10-10-232-233:~/flag13$ diff script1 script2
2437c2437
< Lightoller sees Smith walking stiffly toward him and quickly goes to him. He yells into the Captain's ear, through cupped hands, over the roar of the steam... 
---
> Lightoller sees 3383f3771ba86b1ed9ab7fbf8abab531 Smith walking stiffly toward him and quickly goes to him. He yells into the Captain's ear, through cupped hands, over the roar of the steam... 
bob@ip-10-10-232-233:~/flag13$ 
```

flag13 : 3383f3771ba86b1ed9ab7fbf8abab531



### #4 Where on the file system are logs typically stored? Find flag 14

Ok so the logs are stored is in `/var/log`

```shell
bob@ip-10-10-232-233:~/flag13$ cd /var/log
bob@ip-10-10-232-233:/var/log$ ls
Xorg.0.log             cloud-init.log    kern.log.2.gz
Xorg.0.log.old         cups              lastlog
alternatives.log       dist-upgrade      lxd
alternatives.log.1     dpkg.log          mysql
amazon                 dpkg.log.1        speech-dispatcher
apache2                flagtourteen.txt  syslog
apt                    fontconfig.log    syslog.1
auth.log               fsck              syslog.2.gz
auth.log.1             gdm3              syslog.3.gz
auth.log.2.gz          gpu-manager.log   unattended-upgrades
btmp                   hp                wtmp
btmp.1                 kern.log          wtmp.1
cloud-init-output.log  kern.log.1        xrdp-sesman.log
```

ok let's cat it. the file is very long but at the end, there is the flag

```shell
............
Affronting everything discretion men now own did. Still round match we to. Frankness pronounce daughters remainder extensive has but. Happiness cordially one determine concluded fat. Plenty season beyond by hardly giving of. Consulted or acuteness dejection an smallness if. Outward general passage another as it. Very his are come man walk one next. Delighted prevailed supported too not remainder perpetual who furnished. Nay affronting bed projection compliment instrument. 

71c3a8ad9752666275dadf62a93ef393
bob@ip-10-10-232-233:/var/log$
```

flag14 : 71c3a8ad9752666275dadf62a93ef393