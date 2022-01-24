# GCP Best Practices

**Organisation** → **Folder** → **Projects**

Use **Cloud Deployment Manager:** to automate project creation

**Cloud Identity:** **Identity-as-a-Service (IDaaS)** If your organization uses an on-premises or third-party identity provider, [synchronize your user directory with Cloud Identity](https://cloud.google.com/solutions/authenticating-corporate-users-in-a-hybrid-environment) to let users access Google Cloud with their corporate credentials. This way, your identity platform remains the source of truth while **Cloud Identity controls how your employees access Google services**.

Rather than directly assigning permissions, you assign **_roles_**. **IAM [roles](https://cloud.google.com/iam/docs/understanding-roles)** are collections of permissions. Use IAM to apply the security principle of **least privilege**, so you grant only the necessary access to your resources.

**Groups:** We recommend collecting users with the same responsibilities into *groups* and assigning IAM roles to the groups rather than to individual users

**Service Accounts:** A [service account](https://cloud.google.com/iam/docs/understanding-service-accounts) is a special type of Google Account that represents a Google Cloud service identity or app rather than an individual user. Like users and groups, service accounts can be assigned IAM roles to grant access to specific resources. Service accounts authenticate with a key rather than a password. Google manages and rotates the service account keys for code running on Google Cloud. **We recommend that you use service accounts for server-to-server interactions.**

**Organization policy:** An organization policy focuses on *what can be done*, providing the ability to set restrictions on specific resources to determine how they can be configured and used.

**Use VPC to define your network**

**Use VPCs and subnets to map out your network, and to group and isolate related resources.** Virtual Private Cloud (VPC) is a virtual version of a physical network. VPC networks provide scalable and flexible networking for your Compute Engine virtual machine (VM) instances, and for the services that leverage VM instances, including Google Kubernetes Engine (GKE), Dataproc, and Dataflow, among others.

**VPC networks are global resources; a single VPC can span multiple regions without communicating over the public internet.** This means you can connect and manage resources distributed across the globe from a single Google Cloud project, and you can create multiple, isolated VPC networks in a single project.

VPC networks themselves do not define IP address ranges. Instead, each VPC network consists of one or more partitions called **_subnetworks_**. Each subnet in turn defines one or more IP address ranges. **Subnets are regional resources;** each subnet is explicitly associated with a single region.

**Manage traffic with firewall rules**

Each VPC network implements a distributed virtual firewall. **Configure [firewall rules](https://cloud.google.com/vpc/docs/firewalls) that allow or deny traffic to and from the resources attached to the VPC**, including Compute Engine VM instances and GKE clusters

**Limit external access**

When you create a Google Cloud resource that leverages VPC, **you choose a network and subnet to place the resource in**.

The resource is assigned an internal IP address from one of the IP ranges associated with the subnet. **Resources in a VPC network can communicate among themselves through internal IP addresses as long as firewall rules permit.**

To communicate with the internet, resources must have an **external, public IP address** or must use [Cloud NAT](https://cloud.google.com/nat/docs/overview) or to connect to other resources outside of the same VPC network, unless the networks are connected in some way—for example, through a **VPN**

**Shared VPC**

Use [Shared VPC](https://cloud.google.com/vpc/docs/shared-vpc) to connect to a common VPC network. Resources in those projects can communicate with each other securely and efficiently across project boundaries using internal IPs.

With Shared VPC and IAM controls, you can separate network administration from project administration. This separation helps you implement the principle of least privilege. For example, a centralized network team can administer the network without having any permissions into the participating projects. Similarly, the project admins can manage their project resources without any permissions to manipulate the shared network.

**Connect On-Premises with Google**

[\*\*Cloud Interconnect](https://cloud.google.com/network-connectivity/docs/interconnect):\*\* Low latency, high availability

**Cloud VPN:** bit more simple

**Cloud Audit Logs** "who did what, where, and when"

- **_Admin Activity_** logs contain log entries for API calls or other administrative actions that modify the configuration or metadata of resources. **Admin Activity logs are always enabled**
- **_Data Access_** audit logs record API calls that create, modify, or read user-provided data. Data Access audit logs are **disabled by default** because they can be quite large. You can configure which Google Cloud services produce data access logs.

**Export your logs**

- **Cloud Storage (to meet compliance obligations)**
- **BigQuery (to analyze logs)**
- **Pub/Sub**

**Billing Controls**

You can define billing accounts at the organization level, where you link projects under the Organization node to the billing accounts. You can have multiple billing accounts in your organization, though **we** **recommend creating only one central billing account.**

**Analyze your Bills**

Users with appropriate permissions can view a detailed breakdown of costs, transaction history, and more in the Cloud Console. Information is presented per billing account.

The console also contains [interactive billing reports](https://cloud.google.com/billing/docs/reports) that allow you to **filter and break down costs by project, product, and time range**. The Cloud Console functionality is often sufficient for customers with less complicated Google Cloud setups.

However, if you typically require custom analyses and reporting on your cloud expenditure, we recommend [enabling daily exports of Cloud Billing data to a BigQuery dataset](https://cloud.google.com/billing/docs/how-to/export-data-bigquery). Exported data includes any [labels](https://cloud.google.com/resource-manager/docs/creating-managing-labels) that have been applied to resources.

**Timing is important.** We recommend that you [enable exports to BigQuery](https://cloud.google.com/billing/docs/how-to/export-data-bigquery) at the same time you create your Cloud Billing account. Your [BigQuery dataset](https://cloud.google.com/bigquery/docs/datasets-intro) only reflects Google Cloud billing data incurred from the date you set up Cloud Billing export, and after. That is, **Google Cloud billing data is not added retroactively**, so you won't see Cloud Billing data from before you enable export. After the billing data is in BigQuery, finance teams can analyze the data using standard SQL and use tools that integrate with BigQuery, such as [Data Studio](https://cloud.google.com/billing/docs/how-to/visualize-data).

**Implement cost controls**

You can define *budgets* that generate alerts when spending reaches certain thresholds. Alerts take the form of emails and can optionally generate Pub/Sub messages for programmatic notification. **You can apply the budget to the entire billing account or to an individual project that is linked to the billing account.** For example, you could create a budget to generate alerts when total monthly spending for a billing account reaches 50, 80, and 100 percent of the specified budget amount.

You can also use [quotas](https://console.cloud.google.com/iam-admin/quotas) to cap the consumption of a particular resource. For example, you can set a maximum "query usage per day" quota over the BigQuery API to ensure that a project does not overspend on BigQuery.
