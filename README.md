# OpenSatelliteProject
Project Information
TODO: Write this page.

# GOES Dump Configuration File

In latest commit we added support to configuration file on GOES Dump. It's named `user.config`

On Linux the configuration file is under: `~/.local/share/OpenSatelliteProject/goesdump.exe_Url_XXXX/`
On Windows the configuration file is under: **TODO**

The properties are:

* `EnableEMWIN` => Enable EMWIN Processing (default: **false**)
* `EnableDCS`   => Enable DCS Processing (default: **false**)
* `ChannelDataServerName` => Channel Data Server Hostname (default: **localhost**)
* `ChannelDataServerPort` => Channel Data Server Port (default: **5001**)
* `ConstellationServerName` => Constellation Data Server Hostname (default: **localhost**)
* `ConstellationServerPort` => Constellation Data Server Port (default **9000**)
* `StatisticsServerName` => Statistics Data Server Hostname (default: **localhost**)
* `StatisticsServerPort` => Statistics Data Server Port (default: **5002**)
* `HTTPPort` => HTTP Server Port (default: **8090**)
* `GenerateFDFalseColor` => Generate FullDisk False Color Images (default: **true**)
* `GenerateNHFalseColor` => Generate Northern Hemisphere False Color Images (default: **true**)
* `GenerateSHFalseColor` =>  Generate Northern Hemisphere False Color Images (default: **true**)
* `GenerateXXFalseColor` =>  Generate Northern Hemisphere False Color Images (default: **true**)
* `GenerateUSFalseColor` =>  Generate Northern Hemisphere False Color Images (default: **true**)

Property Syntax:
```xml
  <setting name="{PROPERTY_NAME}" serializeAs="String">
    <value>{PROPERTY_VALUE}</value>
  </setting>
```

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
