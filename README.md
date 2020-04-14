## QNAP Collectd Influxdb Grafana Container and Dashboard

![Dashboard Image](https://i.imgur.com/i4Dwf4l.png)

**Prerequisites**
- [Enable SNMPv2](https://docs.qnap.com/nas/QTS4.4.1/ENG/GUID-1309558B-BFEF-496D-B2DE-6B48D0DE528F.html) on your QNAP with the default Community "snmp-collectd" and Trap address "[YOURNASIP]"
- [Enable SSH](https://docs.qnap.com/nas/QTS4.4.1/ENG/GUID-F27FD4D2-154F-4D9C-B0B1-7121544F427F.html) on your QNAP and ssh into your NAS
- Make sure your [System Time](https://docs.qnap.com/nas/QTS4.4.1/ENG/GUID-F0F0940F-E013-4056-B6BF-A74DC32B5A3F.html) is set correctly and adjusted for [daylight savings](https://docs.qnap.com/nas/QTS4.4.1/ENG/GUID-13394239-A1B1-4FD3-B7A7-F7100617D78F.html)
- Install the ContainerStation application to have access to docker and docker-compose on your QNAP

**Getting started:**

- ssh into your QNAP-NAS

```
ssh admin@[YOURNASIP]
```

- Assuming you are using Containerstation: Download git repo to your /Container share and extract it.


```
cd /share/Container
wget https://github.com/zottelbeyer/QNAP-collectdinfluxdbgrafana/archive/master.zip
unzip master.zip
cd QNAP-collectdinfluxdbgrafana-master
```


- Start the Docker-Compose

```
    docker-compose up -d --build
```

- **Wait 2 minutes for influxdb to create the database and become ready**
- Open your browser to [YOURNASIP]:3000/dashboards
- Login in with default credentials "user" "password"
- Open the QNAP-collectd Dashboard
- On the Dashboard select ALL (or whichever you want to monitor) in the Dropdown Menues at the top
- Select a refresh rate suiting your needs
- Wait another 2 minutes for all panels to be populated

**Updating**

Requires a git installation with access to the location of your cloned directory.

```
# Stop the Containers and remove old images.
docker-compose down --rmi all
# get updates from git repo (with another machine if necessary)
git pull
# rebuild the stack
docker-compose up -d --build
```

**Modifying**

- You can change the default Grafana Username/Password in the .env file
- You can modify the collectd config by editing qnap-collectd/collectd.conf.d/sample.conf

**A Note on Security**

This repo is not yet optimized to be secure. Use at your own risk and **DO NOT EXPOSE IT TO THE INTERNET!**

**Dashboard**

The Dasboard itself can be found at https://grafana.com/grafana/dashboards/11968

**Troubleshooting**

If your Dashboard doesn't show any data proceed in these steps:

1. Restart your collectd container
```
# ssh into your NAS
docker restart qnap-collectd
```
2. Check influxdb is getting data from collectd
```
# ssh into your NAS
docker exec -it influxdb /bin/bash
influx
> use collectd
> show series
# should return lots of entries
```
3. Check the connection between Grafana and InfluxDB
```
Open your browser to http://[YOURNASIP]:3000/datasources/edit/1/
Run the built in Connection test
```
4. Open an Issue including the results

**Testing**

This setup has been tested on my TS-832X with 8 Disks and a NVMe SSD Cache. Let me know if this config works for you.

**Known Issues/ToDo**

- [x] Add SNMP Values for Cache Hit-Rate
- [ ] Add SNMP Values for NVMe Temp
- [x] Add SNMP Values for FAN RPM
- [ ] Workaround collectd.conf not being able to use Docker ENVs

Sources Utilized:
- Many different collectd docker containers
- https://github.com/signalfx/docker-collectd
- https://www.tumfatig.net/20180220/monitoring-openbsd-using-collectd-influxdb-grafana/
- https://grafana.com/grafana/dashboards/4775
- https://collectd.org/documentation/manpages/collectd.conf.5.shtml
