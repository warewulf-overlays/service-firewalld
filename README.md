# firewalld overlay

Ths overlay configures firewalld on the node. It depends on a combination of network tags and node tags.

## Interface Zones
Setting zones in the node config

```
wwctl node set --netname=NETNAME --netdev=DEVICE --nettag='zone=ZONE' NODENAME
```

## Additional services

Services entry is expected to be a ; separated list of entries like:
```
ZONE:service1[,service2[,service...]]
```

for example

```
public:samba,gridftp-data,http,https;external:nfs-client
```

Would open samba and gridftp-data, http and https for the public zone and
nfs-client in the external zone.

Note :: `wwctl` will choke on special characters, so create these entries by
manually editing `nodes.conf`. 

Pro tip, set a placeholder for multiple nodes with 

```
wwctl node set --tag=zone=MYPLACEHOLDER NODENAMES
```

then use sed to set the actual value like
```
systemctl stop warewulfd
sed -e 's/MYPLACEHOLDER/"public:samba,gridftp-data,http,https;external:nfs-client"/g' /etc/warewulf/nodes.conf
systemctl start warewulfd
```
