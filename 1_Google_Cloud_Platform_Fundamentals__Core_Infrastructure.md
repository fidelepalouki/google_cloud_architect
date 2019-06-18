# Cloud Computing

- On-demand self-service (No human needed to get services)
- Broad network access (Access from anywhere)
- Resource pooling (Provider shares resources to customers)
- Rapid elasticity (Get more resources quickly as needed)
- Measured service (Pay only what you consume)

## IAM

### Can be done through:

- Web based console
- SDK and it's CLI tools
- API
- Mobile app

### IAM structure

- Can organize projects into folders and apply policies directly on those folders
- Resources inherit policies from their patent
- Can also have an organization node

### IAM three steps

- Who
- Can do what
- on which resource

Permissions are grouped together into roles to make them easy to use and manage

### 3 types of roles

- Primitive
- Predefined
- Custom

### Affect roles or premsions to persons or VM via service accounts

- Cloud client libraries
- Google APIs clients library

## VPC

## Characteristics

- Each VPC network is contained in a GCP project
- You can provision GCP resources, connect them to each other or isolate them from one another
- Subnets can expand across multiple zones within a region. They have a regional scope.

- Use VPC Peering to interconnect network in GCP project
- Use Shared VPC to share network, or individual subnets, with other GCP protects

### Compute Engine

- Can add boot script to install needed Software for instance

#### Create a VM instance using the console

```bash

```

#### See the zones of the current region

```bash
gcloud compute zones list | grep us-central1
```

#### Set the default zone

```bash
gcloud config set compute/zone us-central1-b
```

#### Create my-vm-2 instance

```bash
gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"
```

my-vm-1 and my-vm-2 IPs share the first 3 bytes (They reside on the same subnet in their Google Cloud VPC even though they are in different zones)

#### Connect between VM instances

#### SSH into my-vm-2 and ping command to confirm that my-vm-2 can reach my-vm-1 over the network

```bash
ping my-vm-1
```

#### Use the ssh command to open a command prompt on my-vm-1:

```bash
ssh my-vm-1
```

#### Install an nginx web server in my-vm-1

```bash
sudo apt-get install nginx-light -y
```

#### Edit the file

```bash
sudo nano /var/www/html/index.nginx-debian.html
```

```bash
curl http://localhost/
```

```bash
exit
```

#### The exit bring us tho the vm-2 terminal. Let's confirm that my-vm-2 can reach the web server on my-vm-1

```bash
curl http://my-vm-1/
```

Open the vm-m external IP link to see the web page

#### Cloud load balancing

### Cloud Storage

#### Storage classes

- Multi-regional
- Regional
- Nearline
- ColdLine

#### Storage options

- Cloud Storage
- Cloud SQL (MySQL, PostGreSQL)
- Cloud Spanner
- Cloud Bigtable
- Cloud Datastore (now Firestore)
- Cloud Bigquery

### Cloud storage and Cloud sql

#### Create a VM and add a startup script

```bash
apt-get update
apt-get install apache2 php php-mysql -y
service apache2 restart
```

#### Create a cloud storage bucket with

```bash
export LOCATION=US
```

```bash
gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID
```

```bash
gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.PNG my-excellent-blog.PNG
```

```bash
gsutil cp my-excellent-blog.PNG gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.PNG
```

#### Modify the Access Control List of the object you just created so that it is readable by everyone:

```bash
gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.PNG
```

#### Create Cloud Sql instance

- Add a user account in Users
- Add network access in Connection

#### SSH into the bloghost VM

```bash
cd /var/www/html
```

```bash
sudo nano index.php
```

#### index.php

```bash
<html>
<head><title>Welcome to my excellent blog</title></head>
<body>
<h1>Welcome to my excellent blog</h1>
<?php
$dbserver = "CLOUDSQLIP";
$dbuser = "blogdbuser";
$dbpassword = "DBPASSWORD";
// In a production blog, we would not store the MySQL
// password in the document root. Instead, we would store it in a
// configuration file elsewhere on the web server VM instance.

$conn = new mysqli($dbserver, $dbuser, $dbpassword);

if (mysqli_connect_error()) {
        echo ("Database connection failed: " . mysqli_connect_error());
} else {
        echo ("Database connection succeeded.");
}
?>
</body></html>
```

#### Restart the web server

```bash
sudo service apache2 restart
```

### Google Kubernetes Engine

####

```bash
export MY_ZONE=us-central1-a
```

```bash
gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
```

```bash
kubectl version
```

#### Launch a single instance of the nginx server

```bash
kubectl run nginx --image=nginx:1.10.0
```

```bash
kubectl get pods
```

#### Expose the nginx container to the Internet

```bash
kubectl expose deployment nginx --port 80 --type LoadBalancer
```

```bash
kubectl get services
```

#### Scale

```bash
kubectl scale deployment nginx --replicas 3
```

```bash
kubectl get pods
```

```bash
kubectl get services
```

### APIs

- Cloud Endpoints
- Apigee Edge

### App Engine

- Standard environment
- Flexible environment

App Engine Flexible evironment treats containers as a mean to an end, but for Kubernetes Engine, containers are fundamental organizing principles

```bash
git clone https://github.com/GoogleCloudPlatform/appengine-guestbook-python
```

```bash
cd appengine-guestbook-python
ls -l
```

```bash
cat app.yaml
```

```bash
dev_appserver.py ./app.yaml
```

```bash
gcloud app deploy ./index.yaml ./app.yaml
```

### Cloud Source Repositories

### Deployment Manager

### Stackdriver

- Monitoring
- Logging
- Trace
- Debug
- Error Reporting
- Profiler

### Cloud Functions - [Smart Parking Story extract: ](https://cloud.google.com/blog/products/gcp/implementing-an-event-driven-architecture-on-serverless-the-smart-parking-story)

You cannot get an accurate view of the current order by looking at only one event. If you only looked at #2 (Address Changed), you wouldn’t know the item quantities. If you only looked at #3 (Quantity Change), you wouldn’t have the address.

To get an accurate view, you need to "replay" all the events for the order. In event-driven architecture, this process is often referred to as "hydrating." Alternatively, you can maintain an aggregate view of the order (the current state of the order) in the database and update it whenever a new command arrives. Both of these methods are correct. In fact, many event-driven architectures use both hydration and aggregates.

### Deployment Manager & Stackdriver

```bash
export MY_ZONE=us-central1-a
```

```bash
gsutil cp gs://cloud-training/gcpfcoreinfra/mydeploy.yaml mydeploy.yaml
```

#### mydeploy.yaml

```yaml
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

#### Subtitute

```bash
sed -i -e 's/PROJECT_ID/'$DEVSHELL_PROJECT_ID/ mydeploy.yaml

sed -i -e 's/ZONE/'$MY_ZONE/ mydeploy.yaml
```

```bash
cat mydeploy.yaml
```

#### Build a deployment from the template

```bash
gcloud deployment-manager deployments create my-first-depl --config mydeploy.yaml
```

#### Update deployment template

```bash
nano mydeploy.yaml
```

```yaml
metadata:
  items:
    - key: startup-script
      value: 'apt-get update; apt-get install nginx-light -y'
```

#### Rebuild

```bash
gcloud deployment-manager deployments update my-first-depl --config mydeploy.yaml
```

#### SSH into my-vm, execute this command to create a CPU load. This Linux pipeline forces the CPU to work on compressing a continuous stream of random data

```bash
dd if=/dev/urandom | gzip -9 >> /dev/null &
```

#### Terminate the workload generator

```bash
kill %1
```

#### BQ

```bash
bq query "select string_field_10 as request, count(*) as requestcount from logdata.accesslog group by request order by requestcount desc"
```
