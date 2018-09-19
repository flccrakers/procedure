# INSTALL CANON LBP 6000 on ubuntu 16.04 in CUPS

| Infos           | Value          |
| ----------------|:---------------|
| Operating system| Ubuntu         |
| OS version      | 16.04          |
| Creation date   | 2008           |
| Last revision   | 2018-04-15     |
| Authors         | Frank Hoonakker|

The goal of this document is to show how to install CANON LBP 6000 on ubuntu 16.04 in CUPS

## Install cups

First of all, install CUPS

```
sudo apt install cups cups-client
```

Add the current user to be able to add printers

```
sudo adduser $USER lpadmin
```

restart (or start) cups
```
sudo service cups restart
or
sudo service cups start
```

## Configure CUPS

You will need to be able to access cups from outside of localhoist, by using the IP adress of your server

First backup your file:
```
sudo cp /etc/cups/cupsd.conf /etc/cups/cupsd.conf.original
sudo chmod a-w /etc/cups/cupsd.conf.original
```

Then edit cupsd.conf:
```
sudo nano  /etc/cups/cupsd.conf
```

Search for this code:
```
# Only listen for connections from the local machine.
Listen localhost:631
Listen /var/run/cups/cups.sock
```

replace the localhost with the IP adress your want (or / and) the host name

Also allow to acces remotly:
In sections **\<Location />**, **\<Location /admin>** and **\<Location /admin/conf>**

Add the following line:
```
Allow all
```

restart server cups by typing:
```
sudo service cups restart
```

## install drivers for CANON LBP 6000

### Find the drivers


### Install the drivers

Install the libraries needed:
```
sudo apt install libatk1.0-0 libglade2-0 libgtk2.0-0 libpango1.0-0 libxcursor1 libxfixes3 libxi6 libxinerama1 libxrandr2
```


Le cas echéant installer les modules complémentaires: 
```
sudo apt-get -f install
```


Enter the directory where the drivers are unziped and with the correct version (64 or 32 bits) and lanch:

```
sudo dpkg -i *
```

## Install the printer

**If not done yet at this point power the printer.**
Verify that the /dev/usb/lp0 exist after pluning the usb. Also check that lsub list you printer

```
sudo /usr/sbin/lpadmin -p LBP6000 -m CNCUPSLBP6018CAPTK.ppd -v ccp://localhost:59787 -E
sudo /usr/sbin/ccpdadmin -p LBP6000 -o /dev/usb/lp1
````


Start the ccp service


```
sudo /etc/init.d/ccpd start
```

Check the status of the ccpd service.

```
sudo /etc/init.d/ccpd status
```

It should print something like the following (with two numbers):
```
/usr/sbin/ccpd: 7556 7555

```

Check if the connection work

```
captstatusui -P LBP6000
```

In the windows open, it should print **ready** or **power saving** without any error.

It's done, you can print now.

## Automation on starting of ccpd and cups when restarting computer

 Open and modify the folowing file :

 ```
 sudo nano /lib/udev/rules.d/70-printers.rules
 ```

 check that the content of the file is the following (or just copy past it)

 ```
 # Low-level USB device add trigger
 #ACTION=="add", SUBSYSTEM=="usb", ATTR{bInterfaceClass}=="07", ATTR{bInterfaceSubClass}=="01", TAG+="udev-configure-printer", RUN+="udev-configure-printer add %p"
 # usblp device add trigger (needed when usblp is already loaded)
 #ACTION=="add", KERNEL=="lp*", TAG+="udev-configure-printer", RUN+="udev-configure-printer add %p"

 # Low-level USB device remove trigger
 #ACTION=="remove", SUBSYSTEM=="usb", ENV{ID_USB_INTERFACES}=="*:0701*:*", RUN+="udev-configure-printer remove %p"
 ```

 Do the same for the following file:

 ```
 sudo nano /etc/init/ccpd-restart.conf
 ```

 with the following content:

 ```
 # ccpd-restart - Relance ccpd si l'imprimante est allumée avant le PC.

 description    "restart daemon ccpd for Canon printer LBP-serie"

 start on started cups
 stop on runlevel [016]

 script
     if [ -e /dev/usb/lp* ]; then
                 /etc/init.d/ccpd stop
 		sleep 5
 		/etc/init.d/ccpd start
     fi
 end script
 ```

 restart ccpd

 ```
 sudo /etc/init.d/ccpd start
 ```



