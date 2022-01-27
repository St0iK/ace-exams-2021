# GCP ACE Exam 2021

# Overview of Google Cloud Platform

Public cloud providers offer services that fall into four broad categories.

- **Compute resources**
- **Storage**
- **Networking**
- **Specialised services such as Machine Learning Services**

## Computing Resources

Public cloud services provide a range computing services options.

- **IaaS** (Infrastructure as a service)
  - Users manage the VMs, operating systems, patching etc
- **PaaS** (Platform as a service)
  - provides a runtime environment to execute applications without the need to manage underlying servers, networks, and storage systems.

### Compute Engine **_(IaaS)_**

Compute Engine is a service that allows users to **create VMs**, attach persistent storage to those VMs, and make use of other GCP services, such as Cloud Storage.

Usually refered to a VM as an **_instance_**

We can create a custom image to use, we select a public image, do our changes and create a Snapshot, we can use that Snapshot to create new VMs.

Compute Engine is a good option when you need maximum control over VM instances (specific image. libraries, software).

```bash
**gcloud compute instances create** ace-instance-1 ace-instance-2 -–zone us-central1-a

# Getting information from the default project
gcloud compute project-info describe

## List your VMs
gcloud compute instances list

gcloud compute instances create ace-instance-n1s8 --machine-type=n1-standard-8

gcloud compute instances stop INSTANCE-NAME

gcloud compute instances start  INSTANCE_NAMES
gcloud compute instances start  ch06-instance-1 ch06-instance-2 --async
gcloud compute instances start  ch06-instance-1 ch06-instance-2 **--zone us-central1-c

# Snapshots
gcloud compute snapshots describe SNAPSHOT_NAME
gcloud compute disks create ch06-disk-1 --source-snapshot=ch06-snapshot --size=100 --type=pd-standard

# Images
gcloud compute images create ch06-image-1 –-source-disk ch06-disk-1**
```

**Guidelines for VMs**

- Choose a machine type with the fewest CPUs and the smallest amount of memory that still meets your requirements, including peak capacity. This will minimize the cost of the VM.
- Use the console for ad hoc administration of VMs. Use scripts with gcloud commands for tasks that will be repeated.
- Use startup scripts to perform software updates and other tasks that should be performed on startup.
- If you make many modifications to a machine image, consider saving it and using it with new instances rather than running the same set of modifications on every new instance.
- **If you can tolerate unplanned disruptions, use preemptible VMs to reduce cost.**
- Use **SSH** or **RDP** to access a VM to perform operating system–level tasks.
- Use Cloud Console, Cloud Shell, or Cloud SDK to perform VM-level tasks.
- Use labels and descriptions. This will help you identify the purpose of an instance and also help when filtering lists of instances.
- Use managed instance groups to enable autoscaling and load balancing. These are key to deploying scalable and highly available services.
- Use GPUs for numeric-intensive processing, such as machine learning and high-performance computing. For some applications, GPUs can give greater performance benefit than adding another CPU.
- Use snapshots to save the state of a disk or to make copies. These can be saved in Cloud Storage and act as backups.
- Use preemptible instances for workloads that can tolerate disruption. This will reduce the cost of the instance by up to 80 percent.

**Preemptible Virtual Machines**

Google can take them away at anytime. Cheap and good for processing

If an application is fault-tolerant and can withstand possible instance interruptions (with a 30 second warning), then using preemptible VM instances can reduce Google Compute Engine costs significantly.

- _May terminate at any time_. If they terminate within 10 minutes of starting, you will not be charged for that time.
- Will be terminated within _24 hours_.
- May not always be available. Availability may vary across zones and regions.
- Cannot migrate to a regular VM.
- Cannot be set to automatically restart.
- Are not covered by any service level agreement (SLA).

🤩 _Preemptible VMs can save you up to 80 percent of the cost of a VM_

**GPUs**

GPUs are used for compute-intensive operations; a common use case for using GPUs is machine learning. It is best to use an image that has GPU libraries installed.

**Instance Groups**

- Instance groups are sets of VMs that are managed as a single entity
- groups of identical VMs

```bash
# Instance Groups, start from a template
gcloud compute instance-templates create ch06-instance-template-1 --source-instance=ch06-instance-1
```

### Kubernetes Engine

Container orchestration system

Kubernetes Engine is designed to allow users to easily run **containerised applications on a cluster of servers.**

Kubernetes Engine is a GCP product that allows users to describe the compute, storage, and memory resources they’d like to run their services. Kubernetes Engine then provisions the underlying resources.

Kubernetes monitors the health of servers in the cluster and automatically repairs problems, such as failed servers. Kubernetes Engine also supports autoscaling, so if the load on your applications increases, Kubernetes Engine will allocate additional resources.

**Kubernetes Engine is a good choice for large-scale applications that require high availability and high reliability.**

**Instance groups** let you manage similar VMs as a **single unit,** but they are more restricted. For example they are the same VM. No support for Deployments

**Kubernetes Cluster Architecture**

- **Cluster Master** Node
  - All interactions are done through the master
  - Manages the cluster - Runs services, API Server, Scheduler
  - The master determines what containers and workloads are run on each node
- **Cluster Worker** Node(s)

Kuberbetes Objects

- **Pods:** Kubernetes deploys containers in groups called _pods._ Single instance running a service. One container one service
- **Services:** Indirection layer to connect to Pods
  - _(Pods can be restarted and change IP Addresses, we access pod through a Service)_
- Volumes
- Namespaces
- **Deployments:** Sets of Identical Pods, you describe the desired state, and the controller runs it
  - Progressing
  - Completed
  - Failed
- **Replicas:** Used by Deployment, makes sure that the correct number of Identical Pods are running
- **Job:** Is an Abstraction of a workload, create a pod to run the job until it is completed

**Kubernetes High Availability**

One way Kubernetes maintains cluster health is by shutting down pods that become starved for resources. Kubernetes supports something called **eviction policies** that set **thresholds** for resources. **When a resource is consumed beyond the threshold, then Kubernetes will start shutting down pods.**

```bash
# create a cluster
gcloud container clusters create ch07-cluster --num-nodes=3 --region=us-central1

gcloud container clusters get-credentials --zone us-central1-a standard-cluster-1
# Use kubectl to manage the container
kubectl get nodes
kubectl describe nodes
kubectl describe pods

gcloud container clusters resize standard-cluster-1 --node-pool default-pool --size 5 --region=us-central1
```

**Node Pools**

A *node pool* is a group of [nodes](https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-architecture#nodes) within a cluster that all have the same configuration. Node pools use a [NodeConfig](https://cloud.google.com/kubernetes-engine/docs/reference/rest/v1/NodeConfig) specification.

When you create a [cluster](https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-architecture), the number of nodes and type of nodes that you specify becomes the *default node pool*. Then, you can add additional *custom node pools* of different sizes and types to your cluster. All nodes in any given node pool are identical to one another.

### App Engine _(serverless)_

With App Engine, developers and application administrators don’t need to concern themselves with configuring VMs. Developers create applications in a popular programming language such as _Java, Go, Python, or Node.js_ and deploy that code to a serverless application environment.

App Engine is available in two types:

- **Standard**
  - language-specific sandbox
  - designed for applications written in one of the supported languages.
  - you do not need operating system packages or other compiled software
  - App Engine standard environment scales down to no running instances if there is no load
- **Flexible**
  - run **Docker containers**
  - if you need libraries or other third-party software installed
  - well suited for applications that can be decomposed into services and where each service can be containerized
  - There will always be at least one container running with your service, and you will be charged for that time even if there is no load on the system.
  **Structure**
  **Application**
  - **Service (micro-service)**
    - **\*Version** (if you change code or config, it creates a new version)\*
      - **Instance**
      - Instance
    - _Version_
      - Instance
  - **Service**
    - _Version_
      - Instance
      - Instance
      - Instance

Number of Instances, depends on the configuration & the load, as the load increases Google can add more isntances.

```bash
gcloud app deploy app.yml

# stop serving a version
gcloud app versions stop v1 v2
```

**Scaling**

Scaling based on load → Dynamic instances(autoscaling or basic scaling)

Resident → fixed number of instances(manual scaling)

Splitting traffic

- Ip Adddress
- HTTP cookie
- Random selection

### Cloud Functions _(serverless)_

Google Cloud Functions is a lightweight computing option that is well suited to **event-driven processing**. Cloud Functions runs code in response to an **event**, like a file being uploaded to **Cloud Storage** or a message being written to a **message queue**.

- The functions execute in a secure, isolated execution environment.
- auto-scaling
- The execution of one function is independent of all others.

_Best for single purpose applications_

Basic Concepts

- Events
  - Event is an action that happens in GCP
  - File uploaded, message arrived
    - Cloud Storage
    - Cloud Pub/Sub
    - HTTP
    - Firebase
    - Stackdriver Logging
- Triggers
  - Associated Function that will run on the event
  - Memory from 128MB to 2GB

```bash
gcloud functions deploy
gcloud functions delete
```

## Storage Resources

### Cloud Storage

Cloud Storage is GCP’s **object storage system**. Objects can be any type of file or binary large object. Objects are organized into **buckets**, which are analogous to directories in a file system. It is important to remember that _Cloud Storage is not a file system_.

- multi-regional
- regional
- nearline (less than once per month)
- coldline (less than once per year)
- Archive (long term data retention)

_Multiregional and regional storage objects can be changed to nearline or coldline. Nearline can be changed only to coldline._

### Persistent Disk

Persistent disks provide block storage on solid-state drives (**SSDs**) and hard disk drives (**HDDs**).

### Cloud Storage for Firebase

**Mobile** app developers may find Cloud Storage for Firebase to be the best combination of cloud object storage and the ability to support uploads and downloads from mobile devices with sometimes unreliable network connections.

### Cloud Filestore

Filestore implements the Network File System (NFS) protocol so system administrators can easily mount shared file systems on virtual servers.

## Databases

### Cloud SQL

- Supports transactions (operations guaranteed to succeed or fail)
- Relational - MySQL or PostgreSQL
- Enable binary logging: Gives point in time restore
- cannot scale horizontally, only vertically

```bash
gcloud sql connect ace-exam-mysql –user=root
gcloud sql backups create ––async ––instance ace-exam-mysql
gcloud sql instances patch ace-exam-mysql –backup-start-time 01:00
```

### Cloud Spanner

- fully managed
- **globally distributed relational database,** strong consistency and **transactions**
  - _global transactions_
- ability to scale horizontally
- **high availability**
- enterprise applications that demand **scalable**, **highly available relational database services**

### BigQuery

- managed analytics service, no need to configure instances
- which provides storage plus query, statistical, and machine learning analysis tools

```bash
bq ––location=[LOCATION] query ––use_legacy_sql=false ––dry_run [SQL_QUERY]
```

### Cloud Datastore

- **NoSQL document database**
- Managed service (replication, backups, and other database administration tasks)
- Documents allow for flexible schemas
- will scale automatically based on load
- It will also shard, or partition, data as needed to maintain performance
- supports **transactions, indexes,** SQL-like queries
- Cloud Datastore is well suited to applications that demand **high scalability** and **structured data** and _do not always need strong consistency when reading data._

```bash
gcloud datastore export –namespaces='(default)' gs://ace_exam_backups
```

### Cloud Bigtable

- **NoSQL** model known as a **wide-column** data model - **petabyte**-scale applications that can manage up to billions of rows and thousands of columns
- Does **NOT** support transactions
- **petabyte-scale** no-sql database that is very good at storing and analyzing time-series data.
- designed for applications with high data volumes and a high-velocity ingest of data. Time series, IoT, and financial applications

```bash
gcloud components update
gcloud components install cbt
```

### Cloud Firestore

- GCP-managed NoSQL database service designed as a backend for highly scalable web and mobile applications
- **offline support**
- **synchronization**, and other features for managing data across mobile devices, IoT devices, and backend data stores.

**Real Time Updates**

### **Cloud Dataproc**

managed **Apache Spark** and **Apache Hadoop** service.

Spark

- Analysis and machine learning
- Hadoop is well suited to batch, big data applications

```bash
gcloud dataproc clusters create cluster-bc3d ––zone us-west2-a
```

### Cloud Memorystore

- Reduce latency with scalable, secure, and highly available **in-memory** service for **Redis** and **Memcached**.
- Max size: 300 GB

```bash
# Create a Memorystore for Redis instance:
gcloud redis instances create myinstance --size=2 --region=us-central1 --redis-version=redis_5_0
```

## GCP Resource Hierarchy

- **Organization**
  - root - corresponds to company/organisation
  - Super Admins, can assign
    - **Organization Administrator** Identity role
      - _Defining the structure of the resource hierarchy_
      - _Defining identity access management policies over the resource hierarchy_
      - _Delegating other management roles to other users_
    - **Access Management** (IAM) role
    - - **Project Creator**
    - - **Billing Account Creator**
- **Folder**
  - Organizations contain folders.
  - Folders can contain other folders or projects.
  - A single folder may contain both folders and projects
- **Project**
  - It is in projects that we **create resources**, use GCP **services**, manage **permissions**, and **manage billing options**.
  ### Organization Policies
  - contrains on resources
  - Organization Policies define what can be done with a resource
    - allow values
    - deny values etc
  - IAM define who can do things
  Policies are managed through the Organization Policies form in the IAM & admin form.
  Multiple policies can be in effect for a folder or project

## Roles in GCP

Role is a collection of Permissions

Identity refers to a user or a Service Account, they both can have roles(list of permissions)

Types of Roles

- **Primitive**
  - Owner
  - Editor
  - Viewer
- **Pre-defined**
  - granular access to resources, they are specific to products/services in GCP
- **Custom**

Principle of least privilege, always better to use Pre-defined

**Managing Identity and Access Management**

When you work with IAM, there are a few common tasks you need to perform:

- Viewing account IAM assignments
- Assigning IAM roles
- Defining custom roles

```bash
gcloud projects get-iam-policy ace-exam-project
```

```bash
# , to grant the role App Engine Deployer to a user identified by jane@acexam.com
gcloud projects add-iam-policy-binding ace-exam-project --member user:jane@aceexam.com --role roles/appengine.deployer
```

**_Permissions cannot be assigned to users, only to roles_**

⚡️ **Service Accounts**, so we can have applications act on behalf of a user, and they can access to what they need. Let's say that we have an application that needs access to a DB.

- When we assign a role to a service account, we are treating it as an identity.
- When we give users permission to access a service account, we are treating it as a resource.

What is a **member**:

- Google Account (for end users)
  - could be [gmail.com](http://gmail.com/) or other domains
- Service account
- Google group - Google Workspace
- Cloud Identity
  - Identity as a Service (IDaaS)
  - Used to work with other identity providers (e.g. Active Directory) - [doc](https://cloud.google.com/identity/docs/overview)

What is a **Policy**

- binds one or more members to a role

> **Google groups** **can help you manage users at scale.** Each member of a Google group inherits the Identity and Access Management (IAM) roles granted to that group. This inheritance means that you can use a group's membership to manage users' roles instead of granting IAM roles to individual users.

⭐️ 100 service accounts per project

🤑 **Billing Accounts**

All project must have a billing account

Roles are associated with billing

From the billing account section you can modify and assign a billing account to projects

- **Billing Account Creator**, which can create new self-service billing accounts
- **Billing Account Administrator**, which manages billing accounts but cannot create them
- **Billing Account User**, which enables a user to link projects to billing accounts
- **Billing Account Viewer**, which enables a user to view billing account cost and transactions

**Billing Budgets and Alerts**

It is really useful to set budget alerts 50 percent, 90 percent, and 100 percent etc

**Exporting Billing Data**

You can export billing data for later analysis or for compliance reasons. Billing data can be exported to either a **BigQuery** database or a **Cloud Storage** file.

## Networking Components

### Networking Services

- Virtual Private Cloud
- Cloud Load Balancing
- Cloud Armor
  - DDoS attacks
- Cloud CDN
- Cloud Interconnect
  - connecting your existing networks to the Google network
- Cloud DNS
  - Domain Name Service, mapping from domain names, such as [example.com](http://example.com/), to IP addresses, such as 74.120.28.18.

### **Virtual Private Cloud**

- VPCs are software versions of physical networks that **link resources in a project.**
- GCP automatically creates a VPC when you create a project.
- You can create additional VPCs and modify the VPCs created by GCP.
- **VPCs are global resources**
- VPCs contain **subnets**(subnetworks) for each region
  - Subnets have IP Range associated with them
  - Internal addresses the resources use to communicate between them
- **_Shared VPC:_** **You can have VPC with an organisation**
  - **allows an [organization](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy) to connect resources from multiple projects to a common [Virtual Private Cloud (VPC) network](https://cloud.google.com/vpc/docs/vpc), so that they can communicate with each other securely and efficiently using internal IPs from that network.**

```bash
gcloud compute networks create ace-exam-vpc1 --subnet-mode=auto
```

CDIR Notation, the smaller the last bit, the more IP Addresses we get

### **Firewall Rules**

Firewall rules are defined at the network level and used to control the flow of network traffic to VMs.

- **Direction:** Either ingress or egress.
- **Priority:** Highest-priority rules are applied; from 0 to 65535. 0 is the highest priority, and 65535 is lowest.
- **Action:** Either allow or deny.
- **Target:** An instance to which the rule applies. Targets can be all instances in a network, instances with particular network tags, or instances using a specific service account.
- **Source/destination:** Source applies to ingress rules and specifies source IP ranges, instances with particular network tags, or instances using a particular service account. You can also use combinations of source IP ranges and network tags and combinations of source IP ranges and service accounts used by instances. The IP address 0.0.0.0/0 indicates any IP address. The Destination parameter uses only IP ranges.
- **Protocol and port:** A network protocol such as TCP, UDP, or ICMP and a port number. If no protocol is specified, then the rule applies to all protocols.
- **Enforcement status:** Firewall rules are either enabled or disabled.

```bash
gcloud compute firewall-rules create ace-exam-fwr2 –-network ace-exam-vpc1 –-allow tcp:20000-25000
```

### **Virtual Private Network**

VPNs allow you to securely **send network traffic from the Google network to your own network.** You can create a VPN using Cloud Console or the command line.

### **Cloud DNS**

Cloud DNS is a Google service that provides domain name resolution. At the most basic level, DNS services map domain names, such as [example.com](http://example.com/), to IP addresses, such as 35.20.24.107.

- **A** record _maps_ a **hostname to IP addresses in IPv4**.
- **AAAA** records are used in IPv6 to map names to IPv6 addresses.
- **CNAME** records hold the **canonical name**, which contains alias names of a domain.

### **Types of Load Balancers**

- **Global** versus **regional** load balancing
  - **Global**
    - **HTTP(S)**, which balances HTTP and HTTPS load across a set of backend instances
    - **SSL Proxy**, which terminates SSL/TLS connections, which are secure socket layer connections.
    - **TCP Proxy**, which terminates TCP sessions at the load balancer and then forwards traffic to backend servers.
  - **Regional**
    - **Internal TCP/UDP**, which balances TCP/UDP traffic on private networks hosting internal VMs
    - **Network TCP/UDP**, which enables balancing based on IP protocol, address, and port. This load balancer is used for SSL and TCP traffic not supported by the SSL Proxy and TCP Proxy load balancers, respectively.
- **External** versus **internal** load balancing
  - **\*Internal TCP/UDP** load balancer is the only internal load balancer\*
- **Traffic type**, such as HTTP and TCP

```bash
gcloud compute forwarding-rules create ace-exam-lb --port=80  --target-pool ace-exam-pool
```

### **Deployment Manager**

> like Terraform for GCP

Deployment Manager configuration files are written in **YAML** syntax. The configuration files start with the word **resources**, followed by resource entities, which are defined using three fields:

- **name**, which is the name of the resource
- **type**, which is the type of the resource, such as compute.v1.instance
- **properties**, which are key-value pairs that specify configuration parameters for the resource. For example, a VM has properties to specify machine type, disks, and network interfaces.

```bash
resources:
- type: compute.v1.instance
  name: ace-exam-deployment-vm
  properties:
     machineType: https://www.googleapis.com/compute/v1/projects/[PROJECT_ID]/zones/us-central1-f/machineTypes/f1-micro
     disks:
     - deviceName: boot
       type: PERSISTENT
       boot: true
       autoDelete: true
```

```bash
gcloud deployment-manager deployments create quickstart-deployment --config vm.yaml
```

---

## Development Tools

### **Cloud Logging/**Stackdriver

To use Stackdriver, we create a workspace and assign a project to it.

> store, search, analyze, monitor, and alert on logging data and events from Google Cloud and Amazon Web Services.

- **Access Transparency**
  - logs record the actions that Google personnel take when accessing customer content
- **Cloud Audit Logs**
  - **Admin Activity** audit logs
    - log entries for API calls or other actions that modify the configuration or metadata of resources
    - e.g. create new VM
  - **Data Access** audit logs
    - API calls that read the configuration or metadata of resources
    - Data Access audit logs– except for BigQuery Data Access audit logs– are disabled by default because audit logs can be quite large.
  - **System Event audit logs**
    - log entries for Google Cloud actions that modify the configuration of resources
    - are generated by Google systems; they are not driven by direct user action.
  - **Policy Denied audit logs**
    - logs when a Google Cloud service denies access to a user or service account because of a security policy violation.
    - generated by default and your Cloud project is charged for the logs storage.

Error Reporting

Trace

Debugger

Profiler

### **Identity-Aware Proxy**

> guard access to your applications and VMs

- Control access to your cloud-based and on-premises applications and VMs running on Google Cloud
- IAP lets you establish a central authorization layer for applications accessed by HTTPS, so you can use an application-level access control model instead of relying on network-level firewalls.
- Implement a zero-trust access model
- Is a free services with [some](https://cloud.google.com/iap#section-4) paid features

## Zones and Regions

**Zones are data center–like resources (asia-east1-a, asia-east-b)**, but they may be comprised of one or more closely coupled data centers.

They are located within **regions**. A region is a geographical location, such as **asia-east1**, **europe-west2**, and **us-east4**.

The zones within a region are linked by low-latency, high-bandwidth network connections.

```bash
# You can get a list of zones with the following command:
gcloud compute zones list
```

[Practice Questions Learnings](https://www.notion.so/Practice-Questions-Learnings-055def161ca04154af698701b8fcc533)

[Google Best Practices](https://www.notion.so/Google-Best-Practices-c6dc248ca612435abf8c0660caece8f3)

[Google Associate Cloud Engineer notes · pistocop](https://www.notion.so/Google-Associate-Cloud-Engineer-notes-pistocop-154789dbd4ad48ddbce615edd1294b74)
