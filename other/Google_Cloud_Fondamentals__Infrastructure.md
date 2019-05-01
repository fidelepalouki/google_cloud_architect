# 19/04/2019 - On demand session at Station F, Instructor: Julien Furgerot @Sfeir

### Cloud Computing

- On demand self-service
- Broad network access
- Resource pooling
- Rapid elasticity
- Measured service

### Range from to

- Compute Engine
- Kubernetes Engine
- App Engine
- Cloud Functions
- Managed services
- Managed services

### Regions and zones

- Zone
- Region
- Multi-region

### Security

- Cloud security requires collaboration
- Google => Infrastructure and network security
- You => Data, access policies

### Group resources according to your organization structure

- Org node
- Folders
- Projects
- Resources (VMs, Storage)

### Project

- Track resource and quota usage
- Enable billing
- Manage permissions and credentials
- Enable services and APIs

- Project ID => by you, immutable
- Project name => by you, editable
- Project number => assigned by gcp, immutable

### Organisation structure

- Permissions given at a higher level cannot be taken away

### IAM

- Who:
  - Google account or Cloud identity user
  - Service account
  - Google group
  - Cloud Identity or G Suite domain
- Can do what:

  - Role (service (compute, ..), resource (instances, ...), verb (list, delete, create, ...))
  - Predefined roles (Instance admin)

- On which resource

### Roles

- Primitive
  - Viewer (read-only access)
  - Editor (deploy apps, modify code, configure services, ...)
  - Owner (Invite, remove members, delete projects, ...)
  - Billing administrator
  - A project can have multiple owner, editors, viewers, and billing adminstrators
- Predefined (offer )
  - InstanceAdmin
  - InstanceOperator
- Custom
  - Instance operator

### Service accounts

- Provide an indentity for carrying out server-to-server interactions in a project
- Used to authenticate from one service to another
- Used to control privileges used by resources
- Identified with an email address

- Service accounts authenticate using keys (App Engine, Compute Engine)
- You can assing a predefined or custom IAM role to the service account

- Service accounts can be used to access resources from 2 or more different projects
- Can be changed without recreating VMs

* Forsety security

### What can you use to manage your GCP administrative users

- Gmail accounts and groupd
- Users in Gsuite directory
- Cloud and groups in Cloud Identity Domain

### Four ways to interact with GCP

- Cloud Platform Console
- Cloud Shell and Cloud SDK (CLI)
- Cloud Console Mobile App (iOS, Android)
- REST-based API

###

- APIs Explorer: interactive tool that lets you easily try Google APis using a browser

- Cloud Clients Libraries (Community-owned, hand crafted client libraries)
- Google API Client Libraries (automatically generate by google)

### VPC

- Each VPC is contained in a project
- Google Cloud VPC networks are global, subnets are regional

### VMs

#### Machine types

- High CPU
- High memory
- Standard
- Shared-core machine

#### Persistent disks

- Standard
- SSD
- Snapshots are possible
- Local SSD

- Resize disks with no downtime
- Instance metadata and startup/shutdown scripts

- I/O throughput increases with persistent disk size!!!

- Big VMs for memory and compute intensive applications
- Use autoscaling for rsilient scalable applications

### Important VPC capabilities

You control the topology of your VPC network

- Use its route table to forward traffic within the network, even accross subnets
- Firewall t control what network traffic is allowed
- Shared VPC: share a network of individual subnets with other GCP projects
- VPC Peering: to interconnect networks in GCP projects

- Global Cloud Load Balancing

  - Users get a single, global anycast IP address
  - Traffic goes over the Google backbone from the closest point-of-presence to the user
  - Backends are selected based on load
  - Only healthy backends receive traffic
  - No pre-warming is required

- Load balancing options

  - Global HTTPS
  - Global SSL Proxy
  - Global TCP Proxy
  - Regional
  - Regional internal (internal to the VPC)

- Cloud CDN: CDN Interconnet if you want to use a different CDN
- Cloud DNS is highly avaiable
  - Creage managez zones, then add, edit, delete DNS records
  - Programmatically manage zones and records using RESTFul API or CLI interface

Many interconnect options
Peering: physical connection between you and Google
Interconnect: connect your private network to Google network

- VPN
- Direct Peering
- Carrier Peering
- Dedicated Interconnect
- Partner Interconnect

### Lab

```bash
gcloud compute zones list | grep us-central1
```

```bash
gcloud config set compute/zone us-central1-b
```

```bash
gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"
```

#### From my-vm-2

```bash
ping my-vm-2
```

```bash
Ctrl + C
```

```bash
ssh my-vm-1
```

```bash
sudo apt-get install nginx-light -y
```

```bash
sudo nano /var/www/html/index.nginx-debian.html
```

```bash
curl http://localhost/
```

Exit my-vm-1 ssh and return to my-vm-2

```bash
exit
```

```bash
curl http://my-vm-1/
```

### Storage classes

- Multi-regional: 99.95%
- Regional: 99.90%
- Nearline: 99.00%
- Coldline 99.00%
- Icecold: 0.12cts per GB per month

### Bring data into Cloud Storage

- Online transfer
- Storage Transfer Service: scheduled, managed batch transfers
- Transfer Appliance (beta): Rackable applicances to securely ship your data

### Cloud storage === Staging for other GCP Services

### Cloud Bigtable

- Fully managed NoSQL, wide-column database service for terabyte applications
- Accessed using HBase API
- Native compatibility with big data, Hadoop ecosystems

Why Bigtable ?

- Replicated storage
- Data encyption in-flight and at rest
- Role-based ACLs
- Drives major applications such as Google Analytics and Gmail

Access patterns

- Application API
- Streaming
- Batch Procesing

### Cloud SQL and Spanner

- Vertical scaling (read and write)
- Horizontal scaling (read)

### Cloud Spanner

Horizontally scalable RDBMS. It supports

- Automatic replication
- Strong global consistency
- Managed instances with high avaiability
- SQL (ANSI 2011 with extensions)

### Cloud Datastore (rebranded as Firestore)

Horizontally scalable NoSQL database

- Built-in redundancy
- Supports ACID transactions

### Lab

```bash
export LOCATION=EU
```

```bash
gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID
```

```bash
gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png
```

```bash
gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
```

```bash
gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
```

### Containers

- Hypervisor-based virtualization (Hardware -> Hypervisor -> Guest OS -> Libs -> App)
- Container-based virtualization (Hardware -> Kernel -> Container runtine -> Libs -> App)

Why use containers ?

- Consistency: accross dev, testing and prod envs
- Loose coupling: between application and OS layer
- Workload migration: simplified between on-prem and cloud env
- Agility: Agile development and operations

### Kubernetes (container cluster orchestration system)

- Automates deployment, scaling and operations for container clusters
- Built for a multi-cloud world

Eases application management

- Workload portability (many envs accros cloud providers)
- Rolling upates
- Persitent storage

Makes applications more elastic

- Multi-zones clusters
- Load balancing
- Autoscaling

### Kubernetes Engine

- Fully manange cluster management and orchestration system for running containers
- Uses a declarative syntax to manage applications

Why use ?

- Decouples operational, development concerns
- Manages and maintains
- Easily update Kubernetes versions as they are released

Complementary services

- Google Cloud Builder
- Google Cloud Container Registry

### Lab Kubernetes

```bash
export MY_ZONE=us-central1-a
```

```bash
gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
```

```bash
kubectl run nginx --image=nginx:1.10.0
```

```bash
kubectl get pods
```

```bash
kubectl expose deployment nginx --port 80 --type LoadBalancer
```

```bash
kubectl get services
```

```bash
Open the cluster external IP address into the address bar
```

```bash
kubectl scale deployment nginx --replicas 3
```

```bash
kubectl get pods
```

```bash
kubectl get services
```

## App Engine

- Pass Engine is a PassS for building scalable applications
- Makes deployment, maintenance, and scalability easy so you can focus on innovation
- Especially suited for building scalable web applications and mobile backends

- Standard Environment

  - Easily deploy your applications
  - Autoscale workloads to meet demand
  - Economical (Free daily quota, usage base pricing)
  - SDK for development, testing and deployment
  - Limitations:

    - Specific versions of Java, Python, PHP, Go, NodeJs
    - Your application must conform to sandbox constraints:
      - No writing to local file system
      - All requests time out at 60s
      - Third-party software installations are limited

  - Other env:
    - Memcache
    - Task queues
    - Scheduled tasks
    - Search
    - Logs

- Flexible Environment
  - Build and deploy containerized apps with a click
  - No sandbox constraints
  - Can access App Engine resources
  - Standard runtimes: Python, J
  - Custom runtime support: any language

### Cloud Endpoints

API: simple, versioned interface

- Distributed API management through an API console
- Expose your API using a RESTful interface
- Control access and validate calls with JSON Web Tokens (JWT) and Google API keys (Identify web, mobile users with Auth0 and Firebase Authentification)
- Generate client libraries

Supported platforms

- Runtime environments

  - App Engine Flexible
  - Kubernetes Engine
  - Compute Engine

- Clients
  - Android
  - ios
  - Javascript

### Apigee (help securise and monetize your applications)

- A platform for making APIs avaiable to your customers and partners
- Contains analytics, monetization, and a developer portal

Cloud Endpoints helps your create and deploy APIs, Apigee helps secure and monetize your APIs

### Lab App Engine

app.yaml

```yaml
runtime: python27
api_version: 1
threadsafe: true
# [START handlers]
handlers:
  - url: /favicon\.ico
    static_files: favicon.ico
    upload: favicon\.ico
  - url: /bootstrap
    static_dir: bootstrap
  - url: /.*
    script: guestbook.app
# [END handlers]
# [START libraries]
libraries:
  - name: webapp2
    version: latest
  - name: jinja2
    version: latest
# [END libraries]
```

```bash
dev_appserver.py ./app.yaml
```

```bash
gcloud app deploy ./index.yaml ./app.yaml
```

### Developing, Deploying, Monitoring

#### Develop

- Cloud Source Repositories
  - Supports collaborative development of cloud apps
  - Includes integration with Stackdriver Debugger
- Cloud Functions
  - Create single-purpose functions that respond to events without a server or runtime
  - Written in Javascirpt, exceute in managed Node.js environment on GCP

#### Deployment (Infrastructure as code)

- Infrastructure management service
- Create a .yaml template describing your env and use Deployment Manager to create resources
- Provides repeatable deployments

#### Monitoring: Proactive instrumentation

- Stackdriver:
  - Monitoring:
    - Platform, system, and application metrics
    - Uptime/health checks
    - Dashboard and alerts
  - Logging
    - Platform, system and application logs
    - Log search, view, filter and export
    - Log-based metrics
  - Debugger (App Engine, Source Repository)
    - Debug applications
  - Error Reporting
    - Error notifications
    - Error dashboard
  - Trace -> OpenCensus
    - Latency reporting and sampling
    - Per-URL latency and statistics
  - Profiler(beta)
    - Continuous profiling of CPU and memory consumption

### Lab Monitoring

```bash
gsutil cp gs://cloud-training/gcpfcoreinfra/mydeploy.yaml mydeploy.yaml
```

mydeploy.yaml

```yaml
resources:
  - name: my-vm
    type: compute.v1.instance
    properties:
      zone: ZONE
      machineType: zones/ZONE/machineTypes/n1-standard-1
      metadata:
        items:
          - key: startup-script
            value: 'apt-get update'
      disks:
        - deviceName: boot
          type: PERSISTENT
          boot: true
          autoDelete: true
          initializeParams:
            sourceImage: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-9-stretch-v20180806
      networkInterfaces:
        - network: https://www.googleapis.com/compute/v1/projects/PROJECT_ID/global/networks/default
          accessConfigs:
            - name: External NAT
              type: ONE_TO_ONE_NAT
```

```bash
sed -i -e 's/ZONE/'$MY_ZONE/ mydeploy.yaml
```

```bash
gcloud deployment-manager deployments create my-first-depl --config mydeploy.yaml
```

Update startup script in mydeploy.yaml

```bash
  value: "apt-get update; apt-get install nginx-light -y"
```

Update deployment

```bash
gcloud deployment-manager deployments update my-first-depl --config mydeploy.yaml
```

SSH into my-vm and execute this command to create a CPU load:

```bash
dd if=/dev/urandom | gzip -9 >> /dev/null &
```

Terminate the workload generator

```bash
kill %1
```

### Big Data and Machine Learning in the Cloud

#### Big Data

- Cloud Dataproc
  - Managed way to run Hadoop and Spark/Hive/pig on GCP
  - Create clusters in 90s or less on average
  - Scale clusters up and down even when jobs are running
  - Why ?
    - Easily migrate on-rem Hadoop jobs to the cloud
- Cloud Dataflow
  - Source -> Transforms -> Sink
  - Processes data using Compute Engine instances
    - Clusters are sized for you
    - Automated sclaing, no instance provisionning
    - Streaming and Batch data
    - Why ?
      - ETL (Extract Transform Load) pipelines to move, filter, enrich, shape data
      - Data analysis: batch computation or streaming
      - Orchestration
      - Integrates with GCP services like Cloud Storage, Pub/Sub, BigQuery, and BigTable (Open source Java and Python SDKs)
- Bigquery (fully managed data wharehouse)
  - Provide near real-time interactive analysis of massive datasets
  - Query using SQL Syntax (SQL 2011)
  - No cluster maintenance is required
  - Runs on Google high-performance infrastructure
    - Compute and storage are separated with a terabit network in between
    - You only pay for storage and processing used
    - Automatic discount for long-term data usage
- Cloud Pub/Sub: scalable reliable messagin
  - Supports many-to-many asynchronous messaging
  - Includes support for offline consumers
  - Based on proven Google technologies
  - Integrates with Cloud Dataflow for data processing pipelines
  - Why ?
    - Building block for data ingestion in Dataflow, IoT, Marketing analytics
    - Foundation for Dataflow streaming
    - Push notifications for cloud-based applicatons
    - Connect applications accross GCP (push/pull betweeen Compute Engine and App Engine)
  -
- Cloud Datalab: interactive data exploration
  - Interactive tool for large-scale data exploration, transformation, analysis and visualization
  - Integrate, open-source (built on Jupyter)
  - Why ?

#### Machine Learnings API

#### Cloud ML

#### Tensorflow

### Lab Bigquery
