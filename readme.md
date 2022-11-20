# Lab 11
# Raqin(A01265891) and Dennis

once you have setup both server-one and backup server you can begin writing the scripts


# Backup

put this script in ```~/.local/bin/backup```

This script will backup files to a specific machine. To do this you can use rsync (rsync copies from the local host). wiht Rsync you can use the option 'aucz', 'a' will archive your files, 'u' will prevent the copy of files from the source if the files in the destination are newer.'c' changes the way rsync checks files,'z' compresses the files and -e will specify the remote shell to use

you can backup using the code

```rsync -aucz -e "ssh -i $HOME/backup_key" ${directory} root@${ip_address}:~/backup```

# backup.service

this is a unit file

put this file in ```/etc/systemd/systemd\/backup.service```
this script is used to backup files locally every Friday at 01:00 using rsync

```type=oneshot```               -this will execute the script only once

```Execstart=/opt/wthr/wthr```   -this will execute the wthr script and declares the location of the script when it is transferred

```Wantedby=multi-user.target``` -declares a weaker dependency that waits for multiuser target to startup

# backup.timer
this is a unit file
put this file in ```/etc/systemd/system/backuptimer```
this script is used for the timer to start for the backup.service script

```OnCalendar=Fri *-*-* 01:00:00```    -sets the timer for friday at 01:00:00
```persistent = true```                -the timer will start as soon as the server
goes up if it ever goes down
```Unit=backup.service```              

```RandomizedDelaySec=3m```            -will delay the timer by 3 minutes

```Wantedby=timers.target```           -sets all timer units that have to be active
after boot

# Testing
to test if your scripts work you can do
```sudo systemctl status backup```

and

```sudo systemctl status backup```

on server-one

output:


# Weather


# wthr

use curl to get information from wttr.in 
you can do this  using the code below in a script:

 ```curl -s wttr.in/Vancouver -o /etc/motd```

 this will show you weather information for vancouver

# wthr.service
this is a unit file
put this script in the weather directory on your  local machine
this script will end up in /etc/systemd/system in the digital ocean server
this script is used to get curl and wtter everday at  05:00

```type=oneshot```               -this will execute the script only once

```Execstart=/opt/wthr/wthr```   -this will execute the wthr script and declares the location of the script when it is transferred

```Wantedby=multi-user.target``` -declares a weaker dependency that waits for multiuser target to startup


# wthr.timer
this is a unit file
put this script in the weather directory on your local machine
this script will end up in /etc/systemd/system in the digital ocean server
this script is used for the timer to start for the wthr.service script

```onCalendar=*-*-* 05:00:00```    -sets the timer for everyday at 05:00:00

```persistent = true```            -the timer will start as soon as the server goes 

up if it ever goes down

```Wantedby=timers.target```       -sets all timer units that have to be active after boot


