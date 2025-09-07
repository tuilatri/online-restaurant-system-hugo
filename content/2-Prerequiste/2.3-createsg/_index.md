---
title : "Create Security Groups"
date : "2025-09-06" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---

### Create Security Layers (Security Group)

ℹ️ **Objective**

*   A **Security Group** acts as a virtual firewall at the instance level to control inbound and outbound traffic.
*   In this section, we will create 2 separate Security Groups for the two core components in our architecture:
    1.  **Web Server (`ors-sg`):** Receives traffic from users and allows administrators to connect.
    2.  **Database (`ors-db-sg`):** Only allows connections from the Web Server and administrators, ensuring absolute data security.

---

🔒 **Steps to Follow**

#### **1. Create Security Group for the Web Server (`ors-sg`)**

This is the security layer for the EC2 instances that will run the Node.js backend application.

*   **Step 1: Start creating a Security Group**
    *   From the **VPC Dashboard** interface, select **Security Groups** from the left menu.
    *   Click on **Create security group**.

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-create.png" title="Create a new Security Group" >}}

*   **Step 2: Configure basic information**
    *   **Security group name:** `ors-sg`
    *   **Description:** `Security group for Online Restaurant Web Server`
    *   **VPC:** Select the `ors-vpc` VPC created in the previous step.

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-webserver-config.png"  title="Basic information for the Web Server SG" >}}

*   **Step 3: Set up Inbound Rules**
    *   In the **Inbound rules** section, click **Add rule** and configure the following 4 rules:

| Type | Protocol | Port range | Source | Description |
| :--- | :--- | :--- | :--- | :--- |
| SSH | TCP | 22 | My IP | Allows **you** to connect via SSH from your current IP to manage the server. |
| HTTP | TCP | 80 | Anywhere-IPv4 | Allows users to access the web server via HTTP. |
| HTTPS | TCP | 443 | Anywhere-IPv4 | Allows users to access securely via HTTPS. |
| Custom TCP | TCP | 5000 | Anywhere-IPv4 | Opens the port for the Node.js application (for testing and load balancing). |

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-webserver-inbound.png" title="Configure Inbound Rules for the Web Server SG" >}}

{{% notice info %}}
When selecting **My IP** as the **Source**, AWS will automatically fill in your public IP address. This enhances security by only allowing administrators to access from a trusted location.
{{% /notice %}}

*   **Step 4:** Scroll to the bottom and click **Create security group**.

---

#### **2. Create Security Group for the Database (`ors-db-sg`)**

This security layer is designed to "lock down" the database, allowing only absolutely necessary connections.

*   **Step 1: Configure basic information**
    *   Click **Create security group** again.
    *   **Security group name:** `ors-db-sg`
    *   **Description:** `Security group for Online Restaurant Database`
    *   **VPC:** Select the `ors-vpc` VPC.

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-db-config.png" title="Basic information for the Database SG" >}}

*   **Step 2: Set up Inbound Rules**
    *   Click **Add rule** and configure the following 2 rules:

| Type | Protocol | Port range | Source | Description |
| :--- | :--- | :--- | :--- | :--- |
| MYSQL/Aurora | TCP | 3306 | Custom -> `ors-sg` | **Important:** Only allows web servers (belonging to `ors-sg`) to connect to the DB. |
| MYSQL/Aurora | TCP | 3306 | My IP | Allows **you** to connect to the DB from your personal machine for management (e.g., using MySQL Workbench). |

{{% notice success %}}
**Important Security Principle:** By selecting another Security Group (`ors-sg`) as the **Source**, we create a dynamic rule. Any server belonging to `ors-sg` can connect to the database without needing to specify a particular IP address. This is the best practice for security in a cloud environment.
{{% /notice %}}

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-db-inbound.png" title="Configure Inbound Rules for the Database SG" >}}

*   **Step 3:** Click **Create security group**.

After completion, you will have 2 correctly configured Security Groups ready to protect your project's resources.