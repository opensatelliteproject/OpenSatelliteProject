# OpenSatelliteProject
Project Information
TODO: Write this page.



# Configuring Syslog for GOES Dump Headless

Edit the file `/etc/rsyslog.conf` and find this lines:
```
# provides UDP syslog reception
#module(load="imudp")
#input(type="imudp" port="514")
```

Uncomment the last two save and restart rsyslog:
```
# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")
```

`service rsyslog restart`
