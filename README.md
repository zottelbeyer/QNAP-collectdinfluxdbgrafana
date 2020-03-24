## QNAP Collectd Influxdb Grafana Container and Dashboard

![Dashboard Image](https://i.imgur.com/zK1JTRe.png)

**Prerequisites**
- [Enable SNMPv2](https://docs.qnap.com/nas/QTS4.4.1/ENG/GUID-1309558B-BFEF-496D-B2DE-6B48D0DE528F.html) on your QNAP with the default Community "QNAP-collectd" and Trap address "[YOURNASIP]"
- [Enable SSH](https://docs.qnap.com/nas/QTS4.4.1/ENG/GUID-F27FD4D2-154F-4D9C-B0B1-7121544F427F.html) on your QNAP and ssh into your NAS
- Install the ContaineSstation application to have access to docker and docker-compose on your QNAP

**Getting started:**

- ssh into your QNAP-NAS

```
ssh admin@[YOURNASIP]
```

- Assuming you are using Containerstation: Download git repo to your /Container share and extract it.


```
cd /share/Container
wget https://github.com/zottelbeyer/QNAP-collectdinfluxdbgrafana/archive/master.zip
unzip QNAP-collectdinfluxdbgrafana-master.zip
cd QNAP-collectdinfluxdbgrafana-master
```


- Start the Docker-Compose

```
    docker-compose up -d --build
```

- Open your browser to [YOURNASIP]:3000
- Login in with default credentials "user" "password"

**Modifying**

- You can change the default Grafana Username/Password in the .env file
- You can modify the collectd config by editing qnap-collectd/collectd.conf.d/sample.conf

**A Note on Security**

This repo is not yet optimized to be secure. Use at your own risk and **DO NOT EXPOSE IT TO THE INTERNET!**

**Dashboard**

The Dasboard itself can be found at https://grafana.com/grafana/dashboards/11968

**Testing**

This setup has been tested on my TS-832X with 8 Disks and a NVMe SSD Cache. Let me know if this config works for you.

**Known Issues/ToDo**

- [ ] Add SNMP Values for Cache Hit-Rate
- [ ] Add SNMP Values for NVMe Temp
- [ ] Add SNMP Values for FAN RPM
- [ ] Workaround collectd.conf not being able to use Docker ENVs

Sources Utilized:
- Many different collectd docker containers
- https://github.com/signalfx/docker-collectd
- https://www.tumfatig.net/20180220/monitoring-openbsd-using-collectd-influxdb-grafana/
- https://grafana.com/grafana/dashboards/4775
- https://collectd.org/documentation/manpages/collectd.conf.5.shtml
