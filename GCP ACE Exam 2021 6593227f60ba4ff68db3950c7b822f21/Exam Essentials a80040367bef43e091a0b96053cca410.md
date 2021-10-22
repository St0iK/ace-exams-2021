# Exam Essentials

- Pay-as-You-Go-for-What-You-Use Model
    - pay for the resources you use
- Elastic Resource Allocation
    - remove add resources, autoscaling

### Questions

- What is the fundamental unit of computing in cloud computing?
    - VM
- All block storage systems use what block size?
    - Block size can vary.
- Cloud Filestore is based on what file system technology?
    - Network File System (NFS)
- When setting up a network in GCP, your network the resources in it are treated as what?
    - Virtual Private Cloud
- You are deploying a new relational database to support a web application. Which type of storage system would you use to store data files of the database?
    - Block storage
    

---

- Understand what is meant by serverless
    - It still runs on a server, but you don't manage it !
- Understand the differences between Compute Engine, Kubernetes Engine, App Engine, and Cloud Functions
- Understand the difference between object and file storage
- Know the different kinds of databases
- Understand virtual private clouds
    - **A VPC is a logical isolation of an organization’s cloud resources within a public cloud**. In GCP, **VPCs are global; they are not restricted to a single zone or region**. All traffic between GCP services can be transmitted over the Google network without the need to send traffic over the public Internet.
- Understand load balancing
- Understand developer and management tools
- Know the main differences between on-premises and public cloud computing

### Questions

- Your company has deployed 100,000 Internet of Things (IoT) sensors to collect data on the state of equipment in several factories. Each sensor will collect and send data to a data store every 5 seconds. Sensors will run continuously. Daily reports will produce data on the maximum, minimum, and average value for each metric collected on each sensor. There is no need to support transactions in this application. Which database product would you recommend?
    - Cloud Bigtable
- You are tasked with mapping the authentication and authorization policies of your on-premises applications to GPC’s authentication and authorization mechanisms. The GCP documentation states that an identity must be authenticated in order to grant privileges to that identity. What does the term identity refer to?
    - User
        - Identities are abstractions of users. They can also represent characteristics of processes that run on behalf of a human user or a VM in the GCP.
        - Roles are collections of privileges that can be granted to identities.
- Data scientists in your company want to use a machine learning library available only in **Apache Spark**. They want to minimize the amount of administration and DevOps work. How would you recommend they proceed?
    - Use **Cloud Dataproc**
- Database designers at your company are debating the best way to move a database to GCP. The database supports an application with a global user base. Users expect support for transactions and the ability to query data using commonly used query tools. The database designers decide that any database service they choose will need to support ANSI 2011 and global transactions. Which database service would you recommend?
    - Cloud Spanner
        - SQL, global transations
        - *Cloud SQL supports standard SQL but does not have global transaction.*
- Which specialized service supports both batch and stream processing workflows?
    - Cloud Dataproc

---