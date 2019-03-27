## GCP encompenses 3 core features

- Infrastructure
- Platform
- Software

## Append environment variables to the config file

```bash
echo INFRACLASS_REGION=$INFRACLASS_REGION >> ~/infraclass/config
```

```bash
echo INFRACLASS_PROJECT_ID=$INFRACLASS_PROJECT_ID >> ~/infraclass/config
```

```bash
source infraclass/config
echo $INFRACLASS_REGION
echo $INFRACLASS_PROJECT_ID
```

```bash
nano .profile
```

Add

```bash
source infraclass/config
```

## Projects

```bash
gcloud config list
```

```bash
gcloud config list | grep project
gcloud config list project
```

```bash
export PROJEC_1_ID=<ID_1>
export PROJEC_2_ID=<ID_2>
```

```bash
gcloud config set project $PROJECT_1_ID
```

## Jenkins

```bash
sudo /opt/bitnami/ctlscript.sh stop
```

```bash
sudo /opt/bitnami/ctlscript.sh restart
```

## VPC

### VPC Objects

- Projects
- Network
  - (default, auto mode, custom mode)
  - is global and spans all avaible regions
  - Networks don't have IP ranges, so Subnets don't need to fit into an address hierarchy
- Subnetworks
  - 10.0.0.1 => reserved for the router address
  - 10.0.0.255 => reserved for the broadcast address
  - Can be used to manage resources, just like Cloud IAM, labels, tags and managed instance groups
- IP addresses
  - Internal IP & External IP
  - An internal DNS service the symbolic name into the Internal IP address
  - DNS is scoped to the network, so it can translate Web URLs and VMs names of hosts of the same network but not hostnames from VMs in a different network
  - There is also an internal FQDN(fully qualified domain name) for an instance. It uses the format **<hostname|instancename>.c.<project_id>.internal**
  - The DNS name always points to the specific instance no matter if the instance has no longer the same internal IP address after being deleted and recreated
  - Each instance has a metadata server which act as a DNS resolver. It handles all DNS queries for local network resources and routes all other queries to Google's public DNS servers for public name resolution
  - Alias IP ranges

```bash
sudo /sbin/ifconfig
```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```
