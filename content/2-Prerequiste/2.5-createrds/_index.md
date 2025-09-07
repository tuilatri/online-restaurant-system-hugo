---
title : "Create RDS MySQL Database"
date : "2025-09-06"
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

### Initialize the Database with Amazon RDS

ℹ️ **Objective**

*   Create a managed **MySQL** database using the **Amazon RDS** service.
*   Place this database within the created **VPC (`ors-vpc`)** network environment and protect it with a **Security Group (`ors-db-sg`)**.
*   Use the **Free Tier** to save costs during this hands-on workshop.

---

🔒 **Steps to Follow**

#### **1. Access the RDS Service**

*   From the **AWS Management Console** interface, search for and select the **RDS** service.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-search.png" title="Search for the RDS service" >}}

#### **2. Start Creating the Database**

*   In the **RDS Dashboard**, click the **Create database** button.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-dashboard.png" title="Click Create database" >}}

#### **3. Configure Engine and Template**

*   **Choose a database creation method:** Select **Standard create**.
*   **Engine options:** Select **MySQL**.
*   **Templates:** Select **Production**.

{{% notice info %}}
Choosing **Free tier** will automatically limit the configuration options (e.g., you can only select a Single DB instance) to ensure you do not incur unexpected costs.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-engine-template.png" title="Select Standard create, MySQL, and Free Tier" >}}

#### **4. Configure Settings**

*   **DB instance identifier:** `ors-db`
*   **Master username:** `admin`
*   **Master password:** Enter your password (e.g., `0812-haminhtri`).
*   **Confirm password:** Re-enter your password.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-settings.png" title="Configure login credentials for the Database" >}}

#### **5. Configure Connectivity**

This is a crucial step to place the database in the correct network environment.

*   **Virtual private cloud (VPC):** Select `ors-vpc`.
*   **DB Subnet Group:** Leave the default; AWS will automatically create a new Subnet Group suitable for your VPC.
*   **Public access:** Select **Yes**.
*   **VPC security group (firewall):** Select **Choose existing**.
*   **Existing VPC security groups:** Keep the `default` Security Group and select `ors-db-sg`.

<!-- {{% notice warning %}}
**Reason for choosing Public access = Yes:**
In this workshop, we need to connect to the database from our personal computer (using MySQL Workbench) to import initial data. This is secured by the **Security Group `ors-db-sg`**, which only allows access from your IP (the **My IP** rule created in step 2.3). In a real Production environment, you should select **No** and access via a Bastion Host.
{{% /notice %}} -->

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-connectivity.png" title="Configure network connectivity for RDS" >}}

#### **6. Configure Additional Configuration**

*   Expand the **Additional configuration** section.
*   **Initial database name:** `ors`
*   This is the initial schema name that the application will use.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-additional-config.png" title="Set the initial schema name" >}}

#### **7. Finalize, Create, and Check**

*   Scroll to the bottom and click **Create database**.
*   The database initialization process can take 5 to 10 minutes. Please wait until the **Status** column changes to **Available**.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-creating.png" title="Wait for the Database to be created" >}}

*   Once completed, click on the newly created `ors-db` instance.
*   In the **Connectivity & security** tab, find and copy the **Endpoint** value. This is the address used to connect to your database.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-endpoint.png" title="Get the Endpoint information for connection" >}}