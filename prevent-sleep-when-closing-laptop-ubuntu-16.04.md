# Preventing sleep or shut down when closing the laptop

| Infos           | Value          |
| ----------------|:---------      |
| Operating system| Ubuntu         |
| OS version      | 16.04          |
| Creation date   | 2008           |
| Last revision   | 2018-04-15     |
| Authors         | Frank Hoonakker|

The code to manage the power when closing laptop

## Process

Edit the file : */etc/systemd/logind.conf*

```
sudo nano /etc/systemd/logind.conf
```

Uncomment or add the line

```
HandleLidSwitch=ignore
```

Restart the systemd

```
sudo service systemd-logind restart
```

 It's done.

 Alternatiely if it's not working put  open */etc/UPower/UPower.conf* and uncomment or add

 ```
 IgnoreLid=true
 ```

 Always restart logind or restart computer
