## QNAP-Collectd Container

**Prerequisites**
- Enable SNMPv2 on your QNAP with the default Community "QNAP-collectd" and Trap address "[YOURNASIP]"
- Enable SSH on your QNAP and ssh into your NAS
- Install the ContaineSstation application to have access to docker and docker-compose on your QNAP

**Getting started:**

- ssh into your QNAP-NAS


    ssh admin@[YOURNASIP]

- Assuming you are using Containerstation: Download git repo to your /Container share


    cd /share/Container
    wget https://github.com/zottelbeyer/QNAP-collectdinfluxdbgrafana/archive/master.zip
    unzip QNAP-collectdinfluxdbgrafana-master.zip
    cd QNAP-collectdinfluxdbgrafana-master

- Start the Docker-Compose


    docker-compose up -d --build


- Open your browser to [YOURNASIP]:3000
- Login in with default credentials "user" "password"

**Modifying**

- You can change the default Grafana Username/Password in the .env file
- You can modify the collectd config by editing qnap-collectd/collectd.conf.d/sample.conf

**A Note on Security**

This repo is not yet optimized to be secure. Use at your own risk and **DO NOT EXPOSE IT TO THE INTERNET!**

Sources Utilized:
- https://github.com/signalfx/docker-collectd
- https://www.tumfatig.net/20180220/monitoring-openbsd-using-collectd-influxdb-grafana/
- https://grafana.com/grafana/dashboards/4775
- https://collectd.org/documentation/manpages/collectd.conf.5.shtml
