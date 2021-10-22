# GCP ACE Exam 2021

# Overview of Google Cloud Platform

Public cloud providers offer services that fall into four broad categories.

- **Compute resources**
- **Storage**
- **Networking**
- **Specialised services such as Machine Learning Services**
    - AutoML, a machine learning service
    - Cloud Natural Language, a service for analyzing text
    - Cloud Vision for analyzing images
    - Cloud Inference API, a service for computing correlations over time-series data

## Computing Resources

Public cloud services provide a range computing services options.

- **Iaas** (Infrastructure as a service)
    - Users manage the VMs, operating systems, patching etc
- **PaaS** (Platform as a service)
    - provides a runtime environment to execute applications without the need to manage underlying servers, networks, and storage systems.

### Compute Engine

Compute Engine is a service that allows users to **create VMs**, attach persistent storage to those VMs, and make use of other GCP services, such as Cloud Storage.

### Kubernetes Engine

Kubernetes Engine is designed to allow users to easily run **containerised applications on a cluster of servers.** 

Kubernetes Engine is a GCP product that allows users to describe the compute, storage, and memory resources they’d like to run their services. Kubernetes Engine then provisions the underlying resources.

Kubernetes monitors the health of servers in the cluster and automatically repairs problems, such as failed servers. Kubernetes Engine also supports autoscaling, so if the load on your applications increases, Kubernetes Engine will allocate additional resources.

### App Engine

With App Engine, developers and application administrators don’t need to concern themselves with configuring VMs. Developers create applications in a popular programming language such as *Java, Go, Python, or Node.js* and deploy that code to a serverless application environment.

App Engine is available in two types: 

- **Standard**
    - language-specific sandbox
    - you do not need operating system packages or other compiled software
- **Flexible**
    - run **Docker containers**
    - if you need libraries or other third-party software installed

### Cloud Functions

Google Cloud Functions is a lightweight computing option that is well suited to event-driven processing. Cloud Functions runs code in response to an event, like a file being uploaded to Cloud Storage or a message being written to a message queue.

## Storage Resources

### Cloud Storage

Cloud Storage is GCP’s **object storage system**. Objects can be any type of file or binary large object. Objects are organized into **buckets**, which are analogous to directories in a file system. It is important to remember that *Cloud Storage is not a file system*.

- multi-regional
- regional
- nearline (less than once per month)
- coldline (less than once per year)

### Persistent Disk

Persistent disks provide block storage on solid-state drives (**SSDs**) and hard disk drives (**HDDs**).

### Cloud Storage for Firebase

**Mobile** app developers may find Cloud Storage for Firebase to be the best combination of cloud object storage and the ability to support uploads and downloads from mobile devices with sometimes unreliable network connections.

### Cloud Filestore

Filestore implements the Network File System (NFS) protocol so system administrators can easily mount shared file systems on virtual servers.

## Databases

### Cloud SQL

- Relational - MySQL or PostgreSQL

### Cloud Bigtable

- **NoSQL** model known as a **wide-column** data model - **petabyte**-scale applications that can manage up to billions of rows and thousands of columns
- Does **NOT** support transactions

### Cloud Spanner

- **globally distributed relational database,** strong consistency and **transactions**
    - *global transactions*
- ability to scale horizontally
- **high availability**
- enterprise applications that demand **scalable**, **highly available relational database services**

### Cloud Datastore

- **NoSQL document database**
- Managed service (replication, backups, and other database administration tasks)
- Documents allow for flexible schemas
- will scale automatically based on load
- It will also shard, or partition, data as needed to maintain performance
- supports **transactions, indexes,** SQL-like queries
- Cloud Datastore is well suited to applications that demand **high scalability** and **structured data** and *do not always need strong consistency when reading data.*

### Cloud Memorystore

- Reduce latency with scalable, secure, and highly available **in-memory** service for **Redis** and **Memcached**.
- Max size: 300 GB

```bash
# Create a Memorystore for Redis instance: 
gcloud redis instances create myinstance --size=2 --region=us-central1 --redis-version=redis_5_0
```

### Cloud Firestore

- GCP-managed NoSQL database service designed as a backend for highly scalable web and mobile applications
- **offline support**
- **synchronization**, and other features for managing data across mobile devices, IoT devices, and backend data stores.

**Real Time Updates**

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

### Identity Management

IAM

### Development Tools

### Additional Components

- Stackdriver
- Monitoring
- Logging
- Error Reporting
- Trace
- Debugger
- Profiler

[Exam Essentials](GCP%20ACE%20Exam%202021%206593227f60ba4ff68db3950c7b822f21/Exam%20Essentials%20a80040367bef43e091a0b96053cca410.md)