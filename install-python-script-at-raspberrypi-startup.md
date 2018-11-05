# Add a python script at startup of raspberypy

| Infos           | Value          |
| ----------------|:---------      |
| Operating system| Raspbian       |
| OS version      | any            |
| Creation date   | 2018-11-05     |
| Last revision   | 2018-11-15     |
| Authors         | Frank Hoonakker|

The way to install a python script at the starting of rasberry pi by using initi.d



The method to run a program on your Raspberry Pi at startup is to add the program (to be run on boot) to the /etc/init.d directory.  This directory contains the scripts which are started during the boot process (in addition, all programs here are  executed when you shutdown or reboot the system).

Add the program to be run at startup to the init.d directory using the following lines:
```
sudo cp /home/pi/sample.py /etc/init.d/
```
Move to the init directory and open the sample script
```
cd /etc/init.d
sudo nano sample.py
```
Add the following lines to the sample script to make it a Linux Standard Base (LSB) (A standard for software system structure, including the filesystem hierarchy used in the Linux operating system) init script. After the ``` #!/usr/bin/python3``` code'''
```
# /etc/init.d/sample.py
### BEGIN INIT INFO
# Provides:          sample.py
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start daemon at boot time
# Description:       Enable service provided by daemon.
### END INIT INFO
```
init.d scripts require the above runtime dependencies to be documented so that it is possible to verify the current boot order, the order the boot using these dependencies, and run boot scripts in parallel to speed up the boot process.  

You can learn to write init.d scripts following the guide here.

Initd Configure systemd Run a Program On Your Raspberry Pi At Startup

Make the sample script in the init directory executable by changing its permission.
```
sudo chmod +x sample.py
```
Run this command:
```
sudo update-rc.d sample.py defaults
```
Now reboot to hear the Pi speak at startup.
```
sudo reboot
```
The script start now at the startup. 